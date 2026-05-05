---
title: "Authentication Tokens Are Not a Data Contract"
url: "https://devblogs.microsoft.com/devops/authentication-tokens-are-not-a-data-contract/"
date: "Wed, 18 Mar 2026 18:46:49 +0000"
author: "Angel Wong"
feed_url: "https://devblogs.microsoft.com/devops/feed/"
---
<p>Authentication tokens exist to answer one question: <em>is this caller authorized to do this?</em></p>
<p>They are not intended to be a stable data interface, a schema you can depend on, or an input into application logic.</p>
<p>If your application decodes tokens and reads claims from them, this is an important heads-up.</p>
<h2>Token Claims Were Never Guaranteed</h2>
<p>Although tokens may appear readable today, that was never a promise. We have never publicly documented token contents, and as a result, we have always reserved the right to change token claims at any point, for any reason.</p>
<p>Claims may change, become optional, be renamed, be removed, or stop being readable altogether. Relying on decoded token contents may work today, but it has always been an unsupported and fragile pattern across the industry.</p>
<h2>What’s Changing</h2>
<p><strong>Coming this summer, we will be further encrypting authentication tokens.</strong> In some scenarios, these changes may take effect even earlier, as we continue to evolve and change token formats. As this happens, token payloads will no longer be readable by clients. Any application that depends on decoding tokens to extract claims will break.</p>
<p>Applications that already treat tokens as opaque will not be impacted.</p>
<h2>What to Do Instead</h2>
<p>Tokens should be used only for authorization. After acquiring a token, your application should rely on <a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/?view=azure-devops-rest-7.2" target="_blank">supported Azure DevOps REST APIs</a> to retrieve user or organization data. Those APIs provide stable contracts, documentation, and clear expectations around change. Token claims do not.</p>
<p>As a rule of thumb:</p>
<ul>
<li>Use tokens to prove who the caller is and what they’re allowed to do </li>
<li>Use supported APIs when you need actual data </li>
<li>Assume any token claim may change or disappear without notice </li>
</ul>
<p>If you find yourself decoding tokens to read values, that logic belongs elsewhere.</p>
<h2>Final Reminder</h2>
<p>If your application depends on decoded token claims, consider this your warning to move off that pattern now—especially before encryption is enforced this summer.</p>
<p>Authentication tokens are for authentication and authorization, not data access. Treat them as opaque, and use <a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/?view=azure-devops-rest-7.2" target="_blank">supported APIs</a> instead.</p>
<p>The post <a href="https://devblogs.microsoft.com/devops/authentication-tokens-are-not-a-data-contract/">Authentication Tokens Are Not a Data Contract</a> appeared first on <a href="https://devblogs.microsoft.com/devops">Azure DevOps Blog</a>.</p>
