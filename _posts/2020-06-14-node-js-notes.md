---
layout:     post
title:      Node_js_notes
subtitle:   Mobilenet network
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - javascript
---

weird things happens, when I first run a node js server script in my VM, it works good, then I let my computer to sleep mode. And seconds later I realized that might kill the node js process and I turned the computer back on found out the ssh session is gone. I reopen it and list all process, didn't find the node js process. But the webpage is still accessible from all my devices. I made sure it is not the cached stuff that enables the connection. It is really weird. I found similar issue on the stackoverflow. It turns out likely to be that because I am hosting this on the VM and build different SSH connect with it multiple times. Things are likely that all the connections are not exactly to same instance. The google cloud VM may create different instance for same VM via different SSH visit.
https://stackoverflow.com/questions/20091433/cant-find-out-where-does-a-node-js-app-running-and-cant-kill-it/29663533

And this is super interesting.


## file system
const fs = require('fs');
 - fs.rename(old_path,new_path,(err)=>{})
 - fs.appendFile(file_path,data,(err)=>{})
 - fs.unlink(file_path,(err)=>{})  //delete a file
 - fs.mkdir(folder_name,(err)=>{})
 - fs.rmdir(folder_name,(err)=>{})    <br>   // the directory has to be empty
 - fs.writeFile(file_path,data,(err)=>{})
 - fs.readdir(FOLDER_PATH,(err,files)=>{})