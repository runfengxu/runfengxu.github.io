---
layout:     post
title:      Use screen in linux
subtitle:   
date:       2020-04-08
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - centos
 - linux
 - screen
---

## Use Screen to detach a session

When you are using ssh or other software to connect to a server, and you are running some program, when the shell is closed, the session terminates automatically which also causes the program to terminate as well.
But if we want to keep the program running as we can safely close the shell. We can use screen.

### install screen

Install Linux Screen on Ubuntu and Debian

`$ sudo apt install screen`

Install Linux Screen on CentOS and Fedora

`$ sudo yum install screen`

### use screen

 - start a screen session `$ screen`
 - start a named session `$ screen -S session_name`
 - create a screen session with a new window `ctrl+a c`
 - detach a screen session `ctrl+a d`
 - view all screen session running in background `$ screen -ls`
 - resume a screen session `$ screen -r session_id`



Below are some most common commands for managing Linux Screen Windows:

 - Ctrl+a c Create a new window (with shell)
 - Ctrl+a " List all window
 - Ctrl+a 0 Switch to window 0 (by number )
 - Ctrl+a A Rename the current window
 - Ctrl+a S Split current region horizontally into two regions
 - Ctrl+a | Split current region vertically into two regions
 - Ctrl+a tab Switch the input focus to the next region
 - Ctrl+a Ctrl+a Toggle between the current and previous region
 - Ctrl+a Q Close all regions but the current one
 - Ctrl+a X Close the current region
