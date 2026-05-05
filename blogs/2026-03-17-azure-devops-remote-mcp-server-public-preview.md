---
title: "Azure DevOps Remote MCP Server (public preview)"
url: "https://devblogs.microsoft.com/devops/azure-devops-remote-mcp-server-public-preview/"
date: "Tue, 17 Mar 2026 13:52:29 +0000"
author: "Dan Hellem"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>When we released the <a href="https://github.com/microsoft/azure-devops-mcp">local Azure DevOps MCP Server</a>, it gave customers a way to connect Azure DevOps data with tools like Visual Studio and Visual Studio Code through GitHub Copilot Chat. The next step was to make this experience easier to get started with and to enable it for services that support only remote MCP servers.</p>
<p>The Remote MCP Server is a hosted version of the Azure DevOps MCP Server that uses streamable HTTP transport. It supports the same core scenarios as the local server but removes the need for additional setup and installation.</p>
<p>The <strong>Remote Azure DevOps MCP Server</strong> preview is available now. We are excited to see how teams use it and will continue investing in the experience as we expand support and improve the MCP Server tools.</p>
<h1><img alt="👟" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f45f.png" style="height: 1em;" /> Getting Started</h1>
<p>Getting started in simple. Depending on the tools that you are using, you only need to add the following server information to your <code>mcp.json</code>.</p>
<pre><code class="json">{
  "servers": {
    "ado-remote-mcp": {
      "url": "https://mcp.dev.azure.com/{organization}",
      "type": "http"
    }
  },
  "inputs": []
}
</code></pre>
<p>There are additional configuration options available, and you can read more in our <a href="https://learn.microsoft.com/en-us/azure/devops/mcp-server/remote-mcp-server">official documentation</a>.</p>
<p><video controls="controls" height="480" width="640"><source src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/remote-mcp-video.mp4" type="video/mp4" />Your browser does not support the video tag.</video></p>
<h2><img alt="⚙" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/2699.png" style="height: 1em;" /> Supported Clients and Services</h2>
<p>The Remote MCP Server is hosted on the Azure DevOps service and uses Microsoft Entra for authentication. As a result, it follows the authentication rules and constraints that come with Entra. This also means your Azure DevOps organization must be backed by Entra. Standalone organizations, sometimes referred to as MSAs, are not supported by the Remote MCP Server.</p>
<p>The following clients are supported today with no additional onboarding required:</p>
<ul>
<li>Visual Studio with GitHub Copilot </li>
<li>Visual Studio Code with GitHub Copilot </li>
</ul>
<h2><img alt="⌛" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/231b.png" style="height: 1em;" /> Coming Soon</h2>
<p>Additional client tools such as GitHub Copilot CLI, Claude Desktop, Claude Code, and ChatGPT require dynamic registration of an OAuth Client ID in Entra before they can be used with the MCP server. We are working closely with the Entra team to enable this capability. For now, only Visual Studio and Visual Studio Code are supported.</p>
<p>Support for other services, including Azure AI Foundry, Microsoft 365 Copilot, and Copilot Studio, is not yet available. We will share a separate blog post when these services become available with the Azure DevOps MCP Server.</p>
<h2><img alt="📌" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f4cc.png" style="height: 1em;" /> Local MCP Server</h2>
<p>You can continue using the <a href="https://github.com/microsoft/azure-devops-mcp">local MCP Server</a> for the time being. However, we plan to eventually archive that repository and focus our investments on the Remote MCP Server. We do not have a specific archive date yet, but it will align with the Remote MCP Server reaching general availability.</p>
<p>Now is a great time to try the Remote MCP Server and begin moving your workloads over to it instead of relying on the local MCP Server.</p>
<h2><img alt="🆘" class="wp-smiley" src="https://s.w.org/images/core/emoji/17.0.2/72x72/1f198.png" style="height: 1em;" /> Support</h2>
<p>During the public preview, you can submit any issues or questions through the <a href="https://github.com/microsoft/azure-devops-mcp/issues/new?template=remote-mcp-server-issue.md">local Azure DevOps MCP Server repository</a>. This allows our team to quickly review and respond to any problems or feedback you may have.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/azure-devops-remote-mcp-server-public-preview/">Azure DevOps Remote MCP Server (public preview)</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
