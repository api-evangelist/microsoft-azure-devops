---
title: "Axios npm Supply Chain Compromise – Guidance for Azure Pipelines Customers"
url: "https://devblogs.microsoft.com/devops/axios-npm-supply-chain-compromise-guidance-for-azure-pipelines-customers/"
date: "Fri, 24 Apr 2026 10:40:43 +0000"
author: "Josef Sin"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>On <strong>March 31, 2026</strong>, malicious versions of the widely used JavaScript HTTP client library <strong>Axios</strong> were briefly published to the npm registry as part of a supply chain attack.</p>
<p>The affected versions — <strong>1&#46;14.1</strong> and <strong>0&#46;30.4</strong> — included a hidden malicious dependency that executed during installation and connected to attacker-controlled command-and-control (C2) infrastructure to retrieve a second-stage payload.</p>
<p>Because modern development workflows frequently rely on automated dependency resolution during CI/CD builds, environments such as developer workstations and build agents—including those used in Azure Pipelines—may have been exposed if they resolved the compromised versions during installation or update.</p>
<p>For a detailed technical analysis of the attack and recommended mitigations, please refer to the Microsoft Security Blog:</p>
<p><a href="https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/">Mitigating the Axios npm Supply Chain Compromise</a> on the Microsoft Security Blog.</p>
<h2>Impact on Azure Pipelines</h2>
<p>This incident <strong>does not represent a compromise of Azure Pipelines itself</strong>.</p>
<p>Customers who:</p>
<ul>
<li>Use <strong>Microsoft-hosted agents</strong>, and</li>
<li>Run only <strong>Microsoft-authored built-in tasks</strong></li>
</ul>
<p>are <strong>not affected by any compromise of the Azure Pipelines platform or hosted agent infrastructure</strong> as a result of this npm ecosystem attack.</p>
<p>Azure Pipelines Microsoft-hosted agents execute jobs on Microsoft-managed virtual machines. Each pipeline job runs on a newly provisioned VM that is discarded after the job completes. Any changes made during a job are not persisted to subsequent jobs. See <a href="https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops">Microsoft-hosted agents for Azure Pipelines</a> and <a href="https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops">Azure Pipelines agents</a> on Microsoft Learn.</p>
<p>However, CI/CD pipelines execute customer-defined workflows, including installing third-party dependencies during build time. If a pipeline run installed one of the malicious Axios versions, code executed during package installation, and any credentials or secrets available to that affected job should be treated as potentially exposed.</p>
<h2>If Your Pipelines Include Custom Scripts, Extensions, Self-Hosted Agents, or Containers, We Recommend the Following Actions</h2>
<p>You may be at risk if your Azure Pipelines workflows include:</p>
<ul>
<li>Custom pipeline scripts</li>
<li>Third-party extensions installed from the Marketplace</li>
<li>Self-hosted agents</li>
<li>Containerized build environments</li>
</ul>
<h3>Review Self-Hosted Agents</h3>
<p>Self-hosted agents are customer-managed compute infrastructure used to run pipeline jobs. See <a href="https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops">Azure Pipelines agents</a> on Microsoft Learn.</p>
<p>Self-hosted agents that executed pipeline builds during the compromise window may have:</p>
<ul>
<li>Installed malicious dependencies</li>
<li>Persisted compromised packages in local caches</li>
<li>Exposed credentials accessible during pipeline execution</li>
</ul>
<p>We recommend:</p>
<ul>
<li>Reimaging or rebuilding affected agents</li>
<li>Reviewing agent activity logs during the relevant timeframe</li>
<li>Rotating credentials used by affected agents</li>
</ul>
<h3>Audit Third-Party or Custom Pipeline Tasks</h3>
<p>Review whether any:</p>
<ul>
<li>Marketplace extensions</li>
<li>Custom tasks</li>
<li>Inline scripts</li>
</ul>
<p>used in your pipelines depend directly or transitively on Axios and executed npm install or update operations during pipeline execution.</p>
<p>Pipeline steps that resolve compromised dependencies may have access to:</p>
<ul>
<li>Secure pipeline variables</li>
<li>Service connection tokens</li>
<li>Deployment credentials</li>
</ul>
<h3>Review Service Connections</h3>
<p>Azure Pipelines uses <strong>service connections</strong> to authenticate pipelines to external or remote services such as:</p>
<ul>
<li>Azure subscriptions</li>
<li>Container registries</li>
<li>Kubernetes clusters</li>
<li>External build or artifact systems (see <a href="https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops">Service connections</a> on Microsoft Learn)</li>
</ul>
<p>If compromised dependencies executed within a pipeline job, identities or credentials associated with service connections used during that run may have been exposed.</p>
<p>We recommend:</p>
<ul>
<li>Rotating credentials associated with affected service connections</li>
<li>Reviewing actions taken by pipelines using those connections</li>
</ul>
<h3>Clear Pipeline Dependency Caches</h3>
<p>Compromised dependencies may persist in:</p>
<ul>
<li>Pipeline workspace caches</li>
<li><code>npm</code>/<code>yarn</code>/<code>pnpm</code> cache directories</li>
<li>Container build layers</li>
<li>Generated artifacts</li>
<li>Package-manager caches</li>
</ul>
<p>Clear dependency caches associated with affected repositories or agents to prevent reuse of compromised packages in future builds.</p>
<p>Artifacts generated from runs that installed the malicious package versions should be treated as <strong>untrusted</strong> and replaced with clean builds.</p>
<h2>What to do now</h2>
<p>Review any pipeline runs that may have installed the affected Axios versions, especially in workflows that use self-hosted agents, custom tasks, third-party extensions, or containerized build environments.</p>
<p>For detailed attack analysis, indicators of compromise, and mitigation guidance, see <a href="https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/">Mitigating the Axios npm Supply Chain Compromise</a> on the Microsoft Security Blog.</p>
<h2>What to review in your pipelines</h2>
<ul>
<li>Custom pipeline scripts</li>
<li>Third-party extensions installed from the Marketplace</li>
<li>Self-hosted agents</li>
<li>Containerized build environments</li>
</ul>
<h2>Best Practices to Reduce Future Supply Chain Risk in Azure Pipelines</h2>
<h3>Pin Dependency Versions</h3>
<p>Avoid loose semantic version ranges such as:</p>
<pre><code class="json">"axios": "^1.13.0"
</code></pre>
<p>Loose constraints may automatically resolve to newly published versions during routine installs — including compromised ones.</p>
<h3>Use Lockfiles and Deterministic Installs</h3>
<p>Ensure your pipelines:</p>
<ul>
<li>Commit <code>package-lock.json</code> / <code>yarn.lock</code> / <code>pnpm-lock.yaml</code></li>
<li>Use deterministic install commands (e.g. <code>npm ci</code>)</li>
</ul>
<p>This helps prevent unexpected dependency resolution during CI/CD runs.</p>
<h3>Limit Secret Scope in Pipelines</h3>
<p>Minimize exposure by:</p>
<ul>
<li>Using least-privilege service connections</li>
<li>Injecting secrets only into steps that require them</li>
<li>Avoiding global environment variable exposure across jobs</li>
</ul>
<h3>Rebuild Build Outputs After Remediation</h3>
<p>Do not assume that:</p>
<ul>
<li>Container images</li>
<li>Deployment bundles</li>
<li>Published packages</li>
</ul>
<p>produced during a compromised pipeline run are safe.</p>
<p>Rebuild affected outputs after remediating dependencies.</p>
<h2>How to reduce future supply chain risk</h2>
<ul>
<li>Pin dependency versions</li>
<li>Use lockfiles and deterministic installs</li>
<li>Limit secret scope in pipelines</li>
<li>Rebuild build outputs after remediation</li>
</ul>
<h2>Learn More</h2>
<p>To understand the attack mechanics, indicators of compromise, and Microsoft’s mitigation guidance, please review: <a href="https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/">Mitigating the Axios npm Supply Chain Compromise</a> on the Microsoft Security Blog.</p>
<p>Self-hosted agents that executed pipeline builds during the relevant timeframe should be reviewed for signs that they installed the malicious package versions or the injected dependency <code>plain-crypto-js@4.2.1</code>.</p>
<p>We recommend:</p>
<ul>
<li>Reviewing pipeline and agent logs for <code>npm install</code> or <code>npm ci</code> runs that resolved <code>axios@1.14.1</code>, <code>axios@0.30.4</code>, or <code>plain-crypto-js@4.2.1</code></li>
<li>Reviewing network activity for connections to <code>sfrclak[.]com</code> or <code>142.11.206.73</code> on port <code>8000</code></li>
<li>Reimaging or rebuilding affected agents where practical</li>
<li>Rotating credentials that were available to affected runs</li>
</ul>
<p>If an affected pipeline run had access to service connections or deployment credentials, those credentials should be treated as potentially exposed.</p>
<p>We recommend:</p>
<ul>
<li>Rotating credentials associated with service connections used by affected runs</li>
<li>Reviewing service connection usage history and actions taken by those identities during the relevant timeframe</li>
</ul>
<h2>How to verify whether you were affected</h2>
<p>Review pipeline logs for <code>npm install</code> or <code>npm ci</code> executions that resolved:</p>
<ul>
<li><code>axios@1.14.1</code></li>
<li><code>axios@0.30.4</code></li>
<li><code>plain-crypto-js@4.2.1</code></li>
</ul>
<p>Also review network and endpoint telemetry for the following indicators:</p>
<ul>
<li><code>sfrclak[.]com</code></li>
<li><code>142.11.206.73</code></li>
<li><code>hxxp://sfrclak[.]com:8000/6202033</code></li>
</ul>
<p>The post <a href="https://devblogs.microsoft.com/devops/axios-npm-supply-chain-compromise-guidance-for-azure-pipelines-customers/">Axios npm Supply Chain Compromise – Guidance for Azure Pipelines Customers</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
