---
title: 'Tip 0001: Invoke-Webrequest for a specific hostname without a host entry'
url: http://www.azuredevops.com/2018/10/tip-0001-invoke-webrequest-for-specific.html
date: '2018-10-09'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
My imposter "Ah-ha" moment of the day is finally realizing that you can invoke-webrequest for a specific hostname without setting a host entry by using the invoke-webrequest cmdlet with the host header. Ie: invoke-webrequest 172.16.43.5 -Headers @{Host = "mydomain.com"}
