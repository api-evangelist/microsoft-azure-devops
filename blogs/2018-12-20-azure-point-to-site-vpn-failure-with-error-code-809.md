---
title: Azure Point to Site VPN failure with error code 809
url: http://www.azuredevops.com/2018/12/azure-point-to-site-vpn-failure-with.html
date: '2018-12-20'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
I ran into some trouble today troubleshooting a developer's workstation. The issue was when using a Point to Site IKEV2 VPN some clients could not connect - they received an error: The network
connection between your computer and the VPN server could not be established
because the remote server is not responding. This could be because one of the
network devices (e.g, firewalls, NAT, routers, etc) between your computer and
the remote server is not configured to allow VPN connections. Please contact
your Administrator or your service provider to determine which device may be
causing the problem. (Error 809) The strange thing was that some other workstations seemed fine. After bashing our collective heads against the wall, checking checking the P2SChild, doing all sorts of network troubleshooting to the endpoint DNS name - azuregateway-8fc2c9e3-26cd-432a-ae47-92b7f6422a5d-e1a7e8cec41d.vpn.azure.com , editing registry, checking firewalls, etc, we finally determined the cause of the issue. The issue was only apparent on Windows 10 desktops with OS version 1703. After manually updating these Workstations to Windows 10 1803 we no longer received the 809 error. Moral of the story - Remember to update your Windows 10 versions!
