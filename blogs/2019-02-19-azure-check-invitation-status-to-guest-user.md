---
title: Azure - Check Invitation Status to guest user
url: http://www.azuredevops.com/2019/02/azure-check-invitation-status-to-guest.html
date: '2019-02-19'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
Often times you will need to invite a 3rd party to your Azure AD tenant to support your environment. When you add them to a resource, they will automatically be invited as a guest user in your Azure AD tenant, however they won't be able to access this until they accept the invitation email. If you send an invite to a guest user you can see if they have accepted the invitation or not. You also have the option to resend the invitation. From Azure AD you can search for guest users and drill down into an individual one. Here is what the email looks like - the key here is the email comes from " invites@microsoft.com " because it can be sent on behalf of this may end up in the junk or spam email folder, so be sure to have them check there.
