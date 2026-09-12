---
title: Configuration Management with Salt Stack on Windows - Part 1 - Installing Salt
  Stack Server with Vagrant
url: http://www.azuredevops.com/2017/12/configuration-management-with-salt_29.html
date: '2017-12-29'
author: AzureDevops (noreply@blogger.com)
feed_url: http://www.azuredevops.com/feeds/posts/default
---
This is part one of a series of basic configuration management for Saltstack for use in an Windows environment. In this guide we'll be covering the basics of setting up Salt Stack Server with a test machine on Vagrant. It's important to note that the "master" server of Salt must be run on a Linux sever, with their being support for a ton of different operating systems as minions. Part 2 will descibe setting up a Windows minion. We'll be grabbing the recently updated generic Ubuntu 16.04 vagrant box as a starting point https://app.vagrantup.com/generic/boxes/ubuntu1604 Run the following commands to start this box: vagrant init generic/ubuntu1604
vagrant up Once the server is setup, you can SSH into the Linux server and login with vagrant with the password of vagrant . Using the bootstrap script, you can easily provision the Salt Master server https://docs.saltstack.com/en/latest/topics/tutorials/salt_bootstrap.html#salt-bootstrap There are a ton of different opinions listed for getting Salt setup - but since wget is installed by default we'll use these commands to install the latest stable version. The -M parameter is used to install the master server, running without it will install the minion components. wget -O bootstrap-salt.sh https://bootstrap.saltstack.com
sudo sh bootstrap-salt.sh -M You should receive a notice that SALT has been installed. The default out of box configuration works well in most instances. The Master server listens on all interfaces on port 4505 for publications and port 4506 for "returns". Run the command to check that the salt master is running. service salt-master status In the next post, "Part 2 - Configuring Salt Stack Minion with Vagrant" we'll be diving into configuration of a Windows minion.
