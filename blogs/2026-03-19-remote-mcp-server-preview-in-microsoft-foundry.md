---
title: "Remote MCP Server preview in Microsoft Foundry"
url: "https://devblogs.microsoft.com/devops/remote-mcp-server-preview-in-microsoft-foundry/"
date: "Thu, 19 Mar 2026 17:48:10 +0000"
author: "Dan Hellem"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>Earlier this week we release the <a href="https://devblogs.microsoft.com/devops/azure-devops-remote-mcp-server-public-preview">public preview</a> for our Azure DevOps MCP Server. Today we are excited to let you know that the Azure DevOps MCP Server is now available to use in Microsoft Foundry.</p>
<p>For those who are new to Foundry, Microsoft Foundry is a unified platform for building and managing AI powered applications and agents at scale. It brings together model access, orchestration, evaluation, and deployment into a single environment.</p>
<p>It is used to develop intelligent solutions such as copilots and automated workflows, connect AI to real world tools and services, and move projects from experimentation to secure, production ready systems on Azure.</p>
<p>Learn more at <a href="https://ai.azure.com">https://ai.azure.com</a></p>
<p>Adding the Azure DevOps MCP Server to your agent is straightforward. You can either add the tool first or create the agent and add it afterward. In either case, go to <strong>Add Tools</strong> > <strong>Catalog</strong> and search for “Azure DevOps.” You should see Azure DevOps MCP Server (preview) listed in the results.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-catalog.webp"><img alt="foundry catalog image" class="aligncenter size-full wp-image-72576" height="1191" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-catalog.webp" width="2380" /></a></p>
<p>Select the tool and click <strong>Create</strong>.</p>
<p>From there, enter your organization name and click <strong>Connect</strong>.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-connect.webp"><img alt="foundry connect image" class="aligncenter size-full wp-image-72577" height="1153" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-connect.webp" width="1764" /></a></p>
<p>Once you have connected the tool to your agent, you can begin using the Azure DevOps MCP Server tools. However, you may want to limit which tools are available. You can specify a subset of tools to control exactly what your agent can access.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-config-tools.webp"><img alt="foundry config tools image" class="aligncenter size-full wp-image-72584" height="822" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-config-tools.webp" width="665" /></a></p>
<p>To test your agent, simply create a new chat, write a prompt, and run it.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-test.webp"><img alt="foundry test image" class="aligncenter size-full wp-image-72579" height="884" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/03/foundry-test.webp" width="2178" /></a></p>
<p>We are excited to bring the Azure DevOps Remote MCP Server (preview) into Microsoft Foundry. This integration gives builders a simple way to connect their agents to Azure DevOps and create useful workflows that help developers and product managers be more productive.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/remote-mcp-server-preview-in-microsoft-foundry/">Remote MCP Server preview in Microsoft Foundry</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
