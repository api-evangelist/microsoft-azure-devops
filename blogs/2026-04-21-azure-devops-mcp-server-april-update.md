---
title: "Azure DevOps MCP Server April Update"
url: "https://devblogs.microsoft.com/devops/azure-devops-mcp-server-april-update/"
date: "Tue, 21 Apr 2026 14:12:23 +0000"
author: "Dan Hellem"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>This update brings a set of improvements and changes across both local and remote Azure DevOps MCP Servers.</p>
<p>Here’s a summary of what’s changed.</p>
<h2>Query work items with WIQL</h2>
<p>We’ve introduced a new <code>wit_query_by_wiql</code> tool that enables users to construct and run work item WIQL queries. For our remote MCP, to ensure reliability and performance, access to this tool is currently limited to users with the <strong>Insiders</strong> feature enabled. <a href="https://learn.microsoft.com/en-us/azure/devops/mcp-server/remote-mcp-server?view=azure-devops#insiders">Learn more</a>.</p>
<p>As we gather usage telemetry and validate query performance, we plan to make it broadly available.</p>
<h2>Remote MCP Server</h2>
<h3>Annotations</h3>
<p>MCP Annotations are metadata tags that help LLMs understand how to safely and effectively use external tools by providing a shared vocabulary for behavior, context, and risk. We’re implementing annotations for read-only, destructive, and openWorld tools to clearly signal how each tool operates and ensure safer, more reliable interactions.</p>
<h3>Missing tools</h3>
<p>There are still a few gaps between the local and remote MCP servers. We’ve recently added support for <code>repo_get_file_content</code>, <code>repo_list_directory</code>, and <code>repo_vote_pull_request</code>, and will continue closing these gaps by introducing additional tools in the coming weeks.</p>
<h3>Tool restructuring</h3>
<p>One of the key challenges in building an Azure DevOps MCP Server is the sheer surface area that Azure DevOps covers. At the same time, both clients and LLMs tend to perform better with a smaller, more focused set of tools. To address this, we are beginning to consolidate related tools. With our remote MCP Server still in public preview, now is an ideal time to make these improvements. We’re starting incrementally, beginning with the wiki tools, to evaluate performance and usability before expanding further. Here is what you can expect:</p>
<table>
<thead>
<tr>
<th>
        New Tool
      </th>
<th>
        Type
      </th>
<th>
        Actions / Scope
      </th>
<th>
        Replaces
      </th>
</tr>
</thead>
<tbody>
<tr>
<td>
        <code>wiki</code>
      </td>
<td>
        Read-only
      </td>
<td>
        <code>get_page</code>, <code>list_pages</code>, <code>list_wikis</code>, <code>get_wiki</code>
      </td>
<td>
        <code>wiki_get_page</code>, <code>wiki_get_page_content</code>, <code>wiki_list_pages</code>, <code>wiki_list_wikis</code>, <code>wiki_get_wiki</code>
      </td>
</tr>
<tr>
<td>
        <code>wiki_upsert_page</code>
      </td>
<td>
        Write
      </td>
<td>
        Single operation, no action parameter
      </td>
<td>
        <code>wiki_create_or_update_page</code>
      </td>
</tr>
<tr>
<td>
        <code>search_wiki</code>
      </td>
<td>
        Search
      </td>
<td>
      </td>
<td>
        <code>search_wiki</code>
      </td>
</tr>
</tbody>
</table>
<p>There will be more changes to come. Keep an eye on the <a href="https://learn.microsoft.com/en-us/azure/devops/mcp-server/remote-mcp-server">documentation for updates</a>.</p>
<h2>Local MCP Server</h2>
<h3>Personal Access Token Support</h3>
<p>Personal access tokens are now supported for authentication, simplifying the experience for users integrating the Azure DevOps MCP Server with external services and clients such as GitHub Copilot. <a href="https://github.com/microsoft/azure-devops-mcp/blob/main/docs/GETTINGSTARTED.md#-personal-access-token-pat">Learn more</a></p>
<h3>Elicitations</h3>
<p>Elicitations are guided prompts that help ensure the correct information is provided when performing a task. For example, since most operations require a project, we’ve added elicitation support for project selection across the core, work, and work items toolsets.</p>
<p>While elicitations can be helpful, we haven’t yet seen strong demand from the community. As a result, we are experimenting with a limited rollout to evaluate their effectiveness. We would love your feedback. Please share your thoughts in an issue or comment and let us know if you would like to see broader support across more tools and parameters.</p>
<h3>MCP Apps (Experimental)</h3>
<p>MCP Apps are an experimental feature that enables packaging and executing common workflows directly within the MCP Server environment. Rather than manually chaining multiple tools together, MCP Apps provide a more structured and repeatable way to perform tasks such as querying or updating work items.</p>
<p>This approach reduces setup time and helps maintain consistency across users and scenarios.</p>
<p>For example, you can use the <code>mcp_app_my_work_item</code> tool to access a self-contained work item experience that allows you to view work items assigned to you, filter results, and open and edit work items.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/mcp-blog-1.webp"><img alt="mcp blog 1 image" class="aligncenter size-full wp-image-72681" height="1186" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/mcp-blog-1.webp" width="1708" /></a></p>
<p>To try it out, use the <a href="https://github.com/microsoft/azure-devops-mcp/tree/mcp-apps-poc">mcp-apps-poc</a> branch.</p>
<p>Then update your <code>mcp.json</code> configuration to include the <code>mcp-apps</code> domain:</p>
<pre><code class="json">{
  "servers": {
    "ado": {
      "type": "stdio",
      "command": "mcp-server-azuredevops",
      "args": ["contsoso", "-d", "core", "work", "work-items", "mcp-apps"]
    }
  }
}
</code></pre>
<p>We’d love your feedback on MCP Apps. If you find them useful, let us know. Your input will help shape whether this capability is brought into the main local and remote MCP Servers.</p>
<h2>Feedback</h2>
<p>Stay tuned, more updates are on the way. In the meantime, we’d love your feedback. Please leave a comment on this post or <a href="https://github.com/microsoft/azure-devops-mcp/issues">create an issue</a> in the MCP Server repository.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/azure-devops-mcp-server-april-update/">Azure DevOps MCP Server April Update</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
