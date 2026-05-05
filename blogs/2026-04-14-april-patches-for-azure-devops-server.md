---
title: "April Patches for Azure DevOps Server"
url: "https://devblogs.microsoft.com/devops/april-patches-for-azure-devops-server/"
date: "Tue, 14 Apr 2026 19:18:22 +0000"
author: "Gloridel Morales"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>We are releasing patches for our self‑hosted product, <a href="https://azure.microsoft.com/services/devops/server/" rel="noopener" target="_blank">Azure DevOps Server</a>. We strongly recommend that all customers remain on the latest, most secure version to ensure optimal protection and reliability. The latest release of Azure DevOps Server is available from the <a href="https://learn.microsoft.com/azure/devops/server/download/azuredevopsserver?view=azure-devops" target="_blank">download page</a>.</p>
<p>This patch applies to the most recent version, Azure DevOps Server, and includes the following updates:</p>
<ul>
<li>Fixed an issue where completing a pull request could fail due to a null reference exception during work item auto-completion.</li>
<li>Improved validation during sign out to prevent potential malicious redirects.</li>
<li>Fixed creating PAT connection to GitHub Enterprise Server. </li>
</ul>
<p><img alt="⬇" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2b07.png" style="height: 1em;" /><strong>Azure DevOps Server Patch Download</strong> </p>
<table>
<thead>
<tr>
<th>Version</th>
<th>Patch Download</th>
<th>Release Notes</th>
</tr>
</thead>
<tbody>
<tr>
<td>Azure DevOps Server</td>
<td><a href="https://aka.ms/devopsserverpatch3">Download Patch 3</a></td>
<td><a href="https://learn.microsoft.com/azure/devops/server/release-notes/azuredevopsserver?view=azure-devops#azure-devops-server-patch-3-release-date-april-14-2026">Release notes</a></td>
</tr>
</tbody>
</table>
<p><img alt="✅" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2705.png" style="height: 1em;" /><strong>Verifying Installation</strong></p>
<p>To verify that the patch is installed, run the following command on the Azure DevOps Server machine using the patch installer you downloaded:</p>
<pre><code>&lt;patch-installer&gt;.exe CheckInstall
</code></pre>
<p>Replace <code>&lt;patch-installer&gt;</code> with the name of the patch file you downloaded. The command output will indicate whether the patch is installed.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/april-patches-for-azure-devops-server/">April Patches for Azure DevOps Server</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
