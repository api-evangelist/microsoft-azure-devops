---
title: AWS - Basic EC2 / Snapshot Cleanup
url: http://www.azuredevops.com/2018/07/aws-basic-cleanup.html
date: '2018-07-06'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
As you migrate from AWS to Azure ;) you may find that you need to clear up resources to save costs. For example, you may continue to incur costs for snapshots after you de-provision your EC2 instances (virtual machines, for the lamen). To clear out the snapshots you can use the AWS Powershell commands to clear out any snapshot older then 90 days. See the example below - by default Get-EC2Snapshot will show all public snapshots, so the -owner self switch is required. Get-EC2Snapshot -Owner self | ? { $_.StartTime -lt $(get-date).adddays(-90)} | Remove-EC2Snapshot This command may show an error indicating that a Snapshot is still in use. Remove-EC2Snapshot : The snapshot snap-xxxxxx is currently in use by ami-xxxxxxx This may indicate that the snapshot is still in use by the AMI. AMI stands for Amazon machine images and are required to launch a EC2 instance, or a copy of an existing EC2 instance. Luckily, once we launch the image and have EC2 instances running, we no loner need the source snapshot and AMI instance we created. From AWS docs: Next, we can unregister all of the EC2Images (AMIs). To do this we can run the command below: Get-EC2Image -owner self | Unregister-EC2Image And finally, Get-EC2Snapshot -Owner self | ? { $_.StartTime -lt $(get-date).adddays(-90)} | Remove-EC2Snapshot
