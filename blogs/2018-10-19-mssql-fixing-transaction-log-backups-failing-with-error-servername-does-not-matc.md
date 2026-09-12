---
title: MSSQL - Fixing Transaction Log backups failing with error "@@SERVERNAME does
  not match SERVERPROPERTY('ServerName')"
url: http://www.azuredevops.com/2018/10/mssql-fixing-transaction-log-backups.html
date: '2018-10-19'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
To Fix Transaction Log backups failing with error "@@SERVERNAME does not match SERVERPROPERTY('ServerName')" you can simply run the following to verify that the machine name and server name property are the same: SELECT SERVERPROPERTY('MachineName') SELECT @@SERVERNAME If not, you must fix it manually and RESTART SQL - replace oldname and newname with the new name of the SQL server. sp_dropserver 'OLDNAME; GO sp_addserver 'NEWNAME', local; GO
