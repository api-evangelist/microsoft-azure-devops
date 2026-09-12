---
title: How to update Expired VSTS Service Principal Keys in AzureRM portal
url: http://www.azuredevops.com/2017/12/how-to-update-expired-vsts-service.html
date: '2017-12-28'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
If the service principal expires you may need to update the expiration by creating a new key. In Visual Studio Team Services, the following error may be logged if the Service Principal key is expired: Failed to check the resource group status. Error: Could not fetch access token for azure. Status code: 401, status message: Unauthorized. To fix, we can create a new key: Login to the Azure portal: https://portal.azure.com Select Azure Active Directory Select App Registrations from the sidebar Search for the Service Principal Client ID – that has expired . Select the Application after searching. Select “Keys” Create a new password you can enter anything as the
description – set the duration to never expire. Click save and copy the value shown under “value”. In VSTS select Services under project Select "Update Service Configuration" and enter the new key.
