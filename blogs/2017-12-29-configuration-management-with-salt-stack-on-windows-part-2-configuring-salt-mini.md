---
title: Configuration Management with Salt Stack on Windows - Part 2 - Configuring
  Salt Minion with Vagrant
url: http://www.azuredevops.com/2017/12/configuration-management-with-salt_73.html
date: '2017-12-29'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
This is part two of a series of basic configuration management for 
Saltstack for use in an Windows environment. In this guide we'll be 
covering the basics of setting up Salt Stack Windows minion on a test machine 
on Vagrant. Part 3 we will be diving into the meat and pushing some buttons automatically with the Salt Master. In order to create a test minion, we'll be using a Windows Server 2012 R2 vagrant image. https://app.vagrantup.com/mwrock/boxes/Windows2012R2 vagrant init mwrock/Windows2012R2
vagrant up Once the machine is online you can login with vagrant / vagrant. The download for the salt minion can be found on https://repo.saltstack.com/#windows Or the direct link: https://repo.saltstack.com/windows/Salt-Minion-2017.7.2-Py2-AMD64-Setup.exe The Windows installer is straightforward - just enter the Master IP or Hostname for the Salt server. We're not going over the internet, so I entered the Private IP. If you use the default "hostname" option, it will use the windows Hostname as the minion name. The preferred method of installation would be to use the silent install parameters, which are listed below. Salt-Minion-2017.7.2-Py2-AMD64-Setup.exe /S /master = yoursaltmaster /minion-name=hostname Once the minion is installed, you can see the salt-minion displayed in services: The Salt installation is default under C:\ The configuration under C:\salt\conf you can edit the minion_id file if you'd like to change the name. In the next post, "Part 3 - Configuration Management of Windows with Vagrant." we'll be diving into Salt basics and example configuration management tasks.
