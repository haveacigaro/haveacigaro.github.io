---
layout: post
title:  "PHP Deployment with Phing"
date:   2014-03-03 10:44:15
categories: phing development
---

Deploying applications to remote servers can be problematic. Traditionally, files would be transferred via FTP to a remote host, using a tool like FileZilla.

This isn’t however a complete solution. Depending on your project, once the files have been transferred to the server, other operations may need to be carried out (such as changing owners, or file permissions).

Another scenario is that you are using a dependency manager such as Composer. Uploading via FTP would mean that it is necessary to either download the dependancies locally then upload them via FTP or upload the files, then SSH into the remote server and download the dependancies remotely.

You may then need to clear your cache.

So in this scenario several operations need to be completed before the site is live, and these tasks need to be repeated every time a modification has been made to the local copy. So it makes sense to automate these tasks.

#Phing#

Phing [Phing Is Not GNU Make] is a tool which allows us to script the deployment process using XML.

Phing comes with a host of tools to allow us to build an entire application, but for the purpose of this example, we’ll focus on deployment.

#The scenario#

Lets assume you have a project that you’ve currently got a Composer project on GitHub which you intend to be updated on a remote server via SSH. This project has a caching directory which needs to be cleared every time the server is updated.

So the order of operations will be something like this:

The developer will run the deployment Phing script
The Phing script will deploy a custom Phing server script to the remote server via ssh
A git pull request will be made to the git repository
Composer will be instructed to update and dump the autoloaded files
The cache will be cleared
The Files

Initially we need to create two XML files, build.xml and buildremote.xml. The job of build.xml will be to transfer buildremote.xml and to execute it once it is on the server.

#Execution#

As these two files are now configured, the user now only needs to move to execute the command ” “phing” in their terminal, phing will then take over and handle the deployment.