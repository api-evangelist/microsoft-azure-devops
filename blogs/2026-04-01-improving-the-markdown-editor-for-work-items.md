---
title: "Improving the Markdown Editor for Work Items"
url: "https://devblogs.microsoft.com/devops/improving-the-markdown-editor-for-work-items/"
date: "Wed, 01 Apr 2026 17:17:11 +0000"
author: "Dan Hellem"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>We introduced the Markdown editor in July 2025 to bring Markdown support to large text fields in work items. Since then, we’ve received <a href="https://developercommunity.visualstudio.com/t/Markdown-editor-for-work-item-multi-line/10935496">valuable customer feedback</a> highlighting challenges with the editing experience, particularly when switching in and out of edit mode.</p>
<p>Many users found the current interaction model confusing and, at times, disruptive. For example, entering edit mode through actions like double clicking could feel unexpected and interrupt the flow when simply trying to read or review content.</p>
<h2>What’s changing</h2>
<p>To address this, we’ve improved the usability of the Markdown editor by introducing a clearer distinction between preview and edit modes for large text fields.</p>
<p>By default, fields now open in preview mode, allowing you to comfortably read and interact with content without accidentally entering edit mode. When you’re ready to make changes, you can explicitly click the edit icon at the top of the field to switch into editing.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/markdown-edit.webp"><img alt="markdown edit image" class="aligncenter size-full wp-image-72609" height="873" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/markdown-edit.webp" width="1237" /></a></p>
<p>Once your updates are complete, you can easily exit edit mode and return to preview mode.</p>
<p><a href="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/markdown-done.webp"><img alt="markdown done image" class="aligncenter size-full wp-image-72610" height="655" src="https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2026/04/markdown-done.webp" width="1225" /></a></p>
<h2>Why this matters</h2>
<p>This change reflects how most users interact with Markdown editors. By reducing accidental edits and making interactions more intentional, the experience becomes more predictable, less disruptive, and overall more intuitive.</p>
<h2>Rollout plan</h2>
<p>We’ve already been rolling this improvement out to a subset of customers to gather early feedback. So far, the response has been positive. Based on that feedback, we’re now expanding the rollout to all customers over the next two to three weeks.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/improving-the-markdown-editor-for-work-items/">Improving the Markdown Editor for Work Items</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
