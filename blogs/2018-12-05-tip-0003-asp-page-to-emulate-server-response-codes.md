---
title: 'Tip 0003: ASP page to emulate server response codes'
url: http://www.azuredevops.com/2018/12/tip-0003-asp-page-to-emulate-server.html
date: '2018-12-05'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
This is a handy page to emulate a response code on the server. This is useful for testing your custom error pages, either from IIS or Cloudflare. https://gist.github.com/blarsonrp/fa373cd9a562d91d9a42419a34a24a7a < html > <% @ Page Language = " C# " AutoEventWireup = " true " %> < script runat = " server " > protected override void OnLoad (EventArgs e) { base . OnLoad (e); this . Response . StatusCode = 418 ; Response . TrySkipIisCustomErrors = true ; } </ script > </ html >
