---
title: "Optimizing Git policy management at scale"
url: "https://devblogs.microsoft.com/devops/optimizing-git-policy-management-at-scale/"
date: "Wed, 22 Apr 2026 18:20:18 +0000"
author: "Azat Galiev"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>With just a single improvement in the REST API of Azure DevOps, we achieved a massive reduction in CPU usage and execution time when managing Git policies: 2x less CPU and 10-15x faster execution!</p>
<p>This change is already available to all users of <a href="https://azure.com/devops">Azure DevOps</a>, and it&#8217;s time to share a bit more detail: the background, what the change is, and how it helped us improve the performance.</p>
<p>You may find this article useful if you maintain automation that manages Git policy configurations in <a href="https://azure.microsoft.com/en-us/products/devops/repos">Azure Repos</a> using REST API.</p>
<h2>Git policy governance at a big enterprise</h2>
<p>Git policies are crucial for maintaining high quality of code and preventing malicious changes. They define rules enforced before code can land in repos and protected branches.</p>
<p>Azure Repos has a rich policy engine that lets you configure things like a minimum number of reviewers, specific reviewers who must sign off when certain parts of the code are updated, checking for credentials and secrets on push, and much more.</p>
<p>When the number of products, services and repos outgrows a certain point, ensuring the right policies are configured becomes a challenge. Human errors are unavoidable: it&#8217;s very easy to misconfigure something, for example, the service&#8217;s behavior when follow-up changes are pushed to an already approved pull request. Even if the initial state across the repos is correct, configuration often drifts over time when managed manually.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260418182608.webp"><img alt="Screenshot from Azure Repos UI showing &quot;Require a minimum number of reviewers&quot; policy that is configured to require at least one approval on the last iteration when new changes are pushed" class="alignnone size-medium wp-image-72701" height="216" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260418182608-300x216.webp" width="300" /></a></p>
<p>Microsoft offers a wide range of products. Our own code is exclusively hosted in Azure Repos and GitHub, across hundreds of thousands of Git repos, ranging from tiny polyrepos to the largest Git monorepos in the world.</p>
<p>For these reasons, we and many other big enterprises rely on the REST API in Azure DevOps and GitHub to audit and mitigate policy misconfiguration and drift across the entire portfolio of repos. A dedicated service defines the target state of policies and automatically updates them when the actual state drifts from the target.</p>
<p>Let me walk you through how the policies are stored and retrieved.</p>
<h2>How policies relate to projects, repos and branches</h2>
<p>There are two types of Git policies in Azure Repos: push and branch policies. They can also be called repository and pull request policies.</p>
<p>Push policies define the rules of what can enter repos and what cannot, no matter the branch. For example, if someone is trying to push new commits containing any secrets, such a push is rejected, even when pushed to a user branch.</p>
<p>Branch policies, on the other hand, protect specific branches (like <code>main</code>) and require all changes to go via pull requests. For example, they can enforce that code builds and all the tests are green before letting a change land on a protected branch.</p>
<p>In short, different types of policies affect either entire repos or branches inside them.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Drawing-2026-04-18-18.30.31.excalidraw.webp"><img alt="Visualization showing that ADO organization contains projects, and project contains git repos. &quot;Require a minimum number of reviewers&quot; policy affects individual branches like main and releases/v1 while Advanced Security's &quot;Block secrets on push&quot; policy affects entire git repos." class="alignnone size-medium wp-image-72702" height="147" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Drawing-2026-04-18-18.30.31.excalidraw-300x147.webp" width="300" /></a></p>
<p>The policy engine also supports different scopes and inheritance. You can define a policy for a specific branch <code>main</code> in a specific repository, or you can configure a policy that will affect all branches matching <code>releases/*</code> in all repos in a project. The support of cross-repo policies and glob patterns partially solves the maintenance burden described earlier, but is not enough, especially when the repos are spread across many different projects and Azure DevOps organizations.</p>
<p>Two kinds of policies and support of inheritance define the architecture of how the policies are stored.</p>
<p>Each project has its own logical container for the policies, instead of each repo or branch having its own container. This helps to power cross-repo policies as well as support branch glob patterns like <code>releases/*</code>.</p>
<p>With that, all the policy configurations are linked to a specific project, but not a repo or branch. Then how does the engine know which ones to check for a specific operation, like a push or PR completion attempt? This is where the <code>Scope</code> field comes into play.</p>
<h2>Policy scope</h2>
<p>This is a simple string that specifies where in the inheritance hierarchy a given policy is <em>defined</em>. It contains one or two parts separated by a colon:</p>
<ul>
<li>Repo ID &#8211; which repo the policy applies to;</li>
<li>Ref ID (optional) &#8211; which ref (branch) the policy applies to.</li>
</ul>
<p>An example of a <code>Scope</code> value for a branch policy:</p>
<pre><code>2c938d1f6e6f458d816484fc51e7cf74:refs/heads/main
</code></pre>
<p>Such a value makes the branch policy apply only to the <code>main</code> branch in the repo with ID <code>2c938d1f-6e6f-458d-8164-84fc51e7cf74</code>.</p>
<p>A couple of edge cases:</p>
<ul>
<li><code>2c938d1f6e6f458d816484fc51e7cf74</code> &#8211; a push policy for the repo with that ID &#8211; doesn&#8217;t contain the ref part.</li>
<li><code>*:refs/heads/releases/*</code> &#8211; cross-repo branch policy that applies to the branches matching <code>releases/v1</code>, <code>releases/v2</code>, etc.</li>
</ul>
<p>The last piece of the puzzle: the available endpoints to fetch the policies.</p>
<h2>Querying the policies with REST API</h2>
<p>Azure DevOps offers two REST API endpoints to list policy configurations:</p>
<p>The <a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/policy/configurations/list?view=azure-devops-rest-7.2&amp;tabs=HTTP"><code>GET /_apis/policy/configurations</code></a> is an older endpoint that allows querying all the policies in a project, with optional scope filtering <strong>without support of inheritance</strong>. If you pass <code>2c938d1f6e6f458d816484fc51e7cf74:refs/heads/releases/v1</code>, it will only return the policies with that exact value of the scope, but it won&#8217;t return a policy with the scope of <code>*:refs/heads/releases/*</code>, which also applies to that branch.</p>
<p>The other endpoint is <a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/git/policy-configurations/get?view=azure-devops-rest-7.2"><code>GET /_apis/git/policy/configurations</code></a> (mind the <code>/git/</code>). Instead of the single exact value of <code>scope</code>, it accepts the <code>repositoryId</code> / <code>refName</code> pair.</p>
<p>If the first endpoint is useful to fetch which policies are <em>defined</em> at a specific scope, the second one is useful to fetch which <em>apply</em> to a specific scope by using inheritance-aware filtering.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Drawing-2026-04-20-08.40.11.excalidraw.webp"><img alt="Visualization showing git policy inheritance hierarchy: policies at Project scope are inherited by repos, branch folders like releases/* and individual branches like releases/v1. The &quot;GET /_apis/git/policy/configurations&quot; endpoint returns all policies including inherited while the &quot;GET /_apis/policy/configurations&quot; endpoint only returns policies defined at requested scope without inherited policies." class="alignnone size-medium wp-image-72703" height="128" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Drawing-2026-04-20-08.40.11.excalidraw-300x128.webp" width="300" /></a></p>
<p>If you pass <code>repositoryId=2c938d1f-6e6f-458d-8164-84fc51e7cf74</code> and <code>refName=refs/heads/releases/v1</code>, it will return all the policies defined at the following scopes:</p>
<ul>
<li><code>2c938d1f6e6f458d816484fc51e7cf74</code></li>
<li><code>*</code></li>
<li><code>2c938d1f6e6f458d816484fc51e7cf74:refs/heads/releases/v1</code></li>
<li><code>*:refs/heads/releases/v1</code></li>
<li><code>2c938d1f6e6f458d816484fc51e7cf74:refs/heads/releases/*</code></li>
<li><code>*:refs/heads/releases/*</code></li>
<li><code>2c938d1f6e6f458d816484fc51e7cf74:refs/heads/*</code></li>
<li><code>*:refs/heads/*</code></li>
<li><code>2c938d1f6e6f458d816484fc51e7cf74:refs/*</code></li>
<li><code>*:refs/*</code></li>
</ul>
<p>Thanks to filtering by multiple scope values dynamically, it helps to answer the question &#8220;What protects my <code>releases/v1</code> branch in repo X?&#8221;, instead of &#8220;Which policies are defined at the level of <code>releases/v1</code> branch in repo X?&#8221;. It takes into account both the policies defined at that exact scope and any policies inherited from parent scopes.</p>
<p>With that context, here&#8217;s the problem with policy management at scale.</p>
<h2>The Problem: Querying all the policies for one repo</h2>
<p>The enterprise policy management service described earlier has one repo as a unit of work. It takes one specific repo, computes the target state of policies using predefined rules, retrieves the effective state currently configured in Azure DevOps, and then issues a set of policy create/update/delete requests if the effective state is misaligned with the target state. The service needs to see every single policy that applies to the repo as a whole, as well as any branch in that repo.</p>
<p>The only problem is&#8230; There was no way to ask the REST API &#8220;Give me all the policies that apply to a given repo and any of its branches&#8221;. At least, not until now.</p>
<p>As previously mentioned, the first endpoint (<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/policy/configurations/list?view=azure-devops-rest-7.2&amp;tabs=HTTP"><code>GET /_apis/policy/configurations</code></a>) can only filter by the exact value of the scope, but many possible scope values can match a repo, and you can&#8217;t predict them all. You can pass <code>repositoryId</code> without specifying <code>refName</code> to the second endpoint (<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/git/policy-configurations/get?view=azure-devops-rest-7.2"><code>GET /_apis/git/policy/configurations</code></a>), but you will only see the policies that apply to the repository as a whole: direct and inherited push policies. Branch policies aren&#8217;t included because they don&#8217;t affect the whole repository.</p>
<p>How did our service work before the change? The only option was to query all the policies for the entire project using the first endpoint, and perform client-side filtering. This is not critical for most projects, but becomes a big challenge for the ones with thousands of repos and hundreds of thousands of policies. The service ended up serializing and returning hundreds of megabytes of data for every request, and the client had to deserialize and process it. This implied massive overhead, much longer execution time, and more CPU cycles spent by the server and the client.</p>
<h2>The Solution</h2>
<p>The second inheritance-aware endpoint (<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/git/policy-configurations/get?view=azure-devops-rest-7.2"><code>GET /_apis/git/policy/configurations</code></a>) now supports a special value <code>~all</code> for the <code>refName</code> parameter. When passed together with <code>repositoryId</code>, it returns every single branch policy affecting any branch in the repo, as well as any push policy affecting the entire repo, inherited and defined at the scopes within the repo.</p>
<p>Internally, the service takes all the policies in a project and only keeps the ones with scope starting with <code>*</code> (project-level policies affecting all the repos) or with <code>2c938d1f6e6f458d816484fc51e7cf74</code>. This includes both push and branch policies.</p>
<p>After switching the policy management client to using <code>refName=~all</code> instead of doing client-side filtering, the overall server-side CPU consumption dropped by half, across all the endpoints for this client.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260413004123.webp"><img alt="Time chart showing overall CPU utilization over time with clear stabilized drop." class="alignnone size-medium wp-image-72699" height="167" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260413004123-300x167.webp" width="300" /></a></p>
<p>The drop in total wall-clock time across all requests is even more impressive &#8211; from 1-3 thousand hours per day down to only ~100-150, more than 10x-15x improvement!</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260413011224.webp"><img alt="Time chart showing overall wall-clock execution time with clear stabilized drop." class="alignnone size-medium wp-image-72700" height="153" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/Pasted-image-20260413011224-300x153.webp" width="300" /></a></p>
<h2>Conclusion</h2>
<p>For enterprises with large engineering teams, automated policy governance becomes a necessity. The new <code>refName=~all</code> feature further simplifies this process and can significantly improve the performance of your automation. Make use of our REST API to achieve that:</p>
<ul>
<li><a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/policy/?view=azure-devops-rest-7.2"><code>/_apis/policy/</code> set of endpoints</a></li>
<li><a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/git/policy-configurations?view=azure-devops-rest-7.2"><code>/_apis/git/policy/configurations</code> endpoint</a></li>
</ul>
<p>For more information about Git policies, see these docs on Microsoft Learn:</p>
<ul>
<li><a href="https://learn.microsoft.com/en-us/azure/devops/repos/git/repository-settings?view=azure-devops&amp;tabs=browser">Set Git Repository Settings</a></li>
<li><a href="https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=azure-devops&amp;tabs=browser">Git branch policies and settings</a></li>
</ul>
<p>Let me know in the comments below if your team faces similar challenges. I&#8217;ll be happy to chat and answer your questions.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/optimizing-git-policy-management-at-scale/">Optimizing Git policy management at scale</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
