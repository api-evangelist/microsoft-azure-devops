---
title: 'Tip 0002: httpstat.us for HTTP response codes'
url: http://www.azuredevops.com/2018/10/tip-0002-httpstatus-for-http-response.html
date: '2018-10-26'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
Need to trigger a specific response code for some testing? Possibly need to test how your monitoring tool handles a 301 response code? There is a wonderful free service called httpstat.us which can be used to trigger HTTP response codes and payload. This is helpful in testing apps, or web monitoring tools. Simply append the response code that you want to receive with the tool you're testing, for instance, httpstat.us/500 Site: https://httpstat.us/ GitHub project: https://github.com/Readify/httpstatus
