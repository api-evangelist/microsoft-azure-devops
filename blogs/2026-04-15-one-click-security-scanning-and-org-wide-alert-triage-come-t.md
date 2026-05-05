---
title: "One-click security scanning and org-wide alert triage come to Advanced Security"
url: "https://devblogs.microsoft.com/devops/one-click-security-scanning-and-org-wide-alert-triage-come-to-advanced-security/"
date: "Wed, 15 Apr 2026 15:06:53 +0000"
author: "Laura Jiang"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>We&#8217;re shipping two major capabilities that change how security teams enable and act on application security in Azure DevOps: <strong>CodeQL default setup</strong> makes it possible to enable code scanning across your organization without configuring a single pipeline, and a <strong>new combined alerts experience in Security Overview</strong> gives security administrators a single place to search, filter, and coordinate remediation across every repository.</p>
<p>In tandem with dependency scanning default setup and automatic secret scanning, scanning is now the default, and delegating work is built-in to the product with security campaigns powered by the combined alerts experience.</p>
<h2>CodeQL default setup (public preview)</h2>
<p>Until now, enabling CodeQL code scanning on Azure DevOps required manually configuring a pipeline for each repository, installing the CodeQL task, setting up the build steps, and maintaining the pipeline over time. For organizations with hundreds of repositories, this could be a significant barrier to adoption.</p>
<p>CodeQL default setup eliminates that friction. With one click, you can enable code scanning for a repository, or across your entire project or organization. Advanced Security automatically runs CodeQL scans using Azure Pipelines by default on your behalf, with no additional configuration required.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/advanced-security-codeql-default-setup-enablement-repo-unbundled.webp"><img alt="advanced security codeql default setup enablement repo unbundled image" class="aligncenter size-full wp-image-72664" height="566" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/advanced-security-codeql-default-setup-enablement-repo-unbundled.webp" width="979" /></a></p>
<h3>Key capabilities</h3>
<ul>
<li><strong>One-click enablement</strong> at the repository, project, or organization level</li>
<li><strong>Automatic scanning</strong> — no YAML editing, no task installation</li>
<li><strong>Runs on Azure Pipelines</strong> for a seamless out-of-the-box experience </li>
<li><strong>Configurable agent pool</strong> via organization-level repository settings, so you control where scans run</li>
</ul>
<h3>Getting started</h3>
<ol>
<li>Navigate to your repository, project, or organization settings</li>
<li>Enable the Code Security plan for your repository</li>
<li>Enable CodeQL default setup</li>
<li>Scans run on specified schedule, which can be changed at the organization level </li>
</ol>
<p>For more information on default setup, see <a href="https://aka.ms/ghazdo/codeql-default-setup" target="_blank">https://aka.ms/ghazdo/codeql-default-setup</a>.</p>
<hr />
<h2>Combined alerts view and security campaigns</h2>
<p>Security administrators have told us consistently: &#8220;I need to see what&#8217;s happening across my entire organization, not repo by repo.&#8221; The new <strong>combined alerts experience in Security Overview</strong> delivers exactly that.</p>
<h3>See everything in one place</h3>
<p>The Security Overview alerts tab surfaces individual alerts from the default branch of <strong>all repositories</strong> in your organization in a single, unified view. Instead of clicking into each repository to understand your security posture, you can now search, sort, and filter across your entire estate from one screen.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/security-overview-alerts-page.gif"><img alt="Filter with different criteria in the alerts view" class="aligncenter size-full wp-image-72621" height="704" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/security-overview-alerts-page.gif" width="1185" /></a></p>
<h3>Security campaigns: coordinate remediation at scale</h3>
<p><strong>Security campaigns</strong> let you create filtered views of alerts and share them with your team. Focus on a specific vulnerability type, severity level, or set of repositories, then share the campaign as a coordination tool for remediation. Filters are live, so if any new vulnerabilities appear that match your criteria, you&#8217;ll see them appear in your campaigns.</p>
<p>Use campaigns to:</p>
<ul>
<li>Track remediation of a specific CVE across all affected repositories</li>
<li>Create a &#8220;critical secrets&#8221; campaign for your security team&#8217;s weekly triage</li>
<li>Share a filtered view with a development team so they see only what&#8217;s relevant to them</li>
</ul>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/security-overview-alerts-campaigns.gif"><img alt="security overview alerts campaigns image" class="aligncenter size-full wp-image-72620" height="769" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/security-overview-alerts-campaigns.gif" width="1117" /></a></p>
<hr />
<h2>What&#8217;s next</h2>
<p>We&#8217;re continuing to invest in making Advanced Security the most seamless way to secure your Azure DevOps workflows. Both CodeQL default setup and the combined alerts dashboard will roll out to organizations over the next two to three weeks.</p>
<p>If you&#8217;re interested in trying CodeQL default setup, enable it from your repository or organization settings and start scanning.</p>
<p>For the combined alerts dashboard, navigate to your <strong>Organization Settings</strong> > Security overview to view.</p>
<hr />
<p><em>Have feedback? We&#8217;d love to hear from you. Reach out via the Azure DevOps Developer Community or contact your Microsoft account team.</em></p>
<p>The post <a href="https://devblogs.microsoft.com/devops/one-click-security-scanning-and-org-wide-alert-triage-come-to-advanced-security/">One-click security scanning and org-wide alert triage come to Advanced Security</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
