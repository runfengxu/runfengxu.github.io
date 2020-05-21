---
layout:     post
title:      Annoying things about Linux enviroment setting
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Linux
 - Ubuntu
---

WHen I wanted to export a enviromnet variable. ANd I wanted it to be set permanently.

`$:  export GOOGLE_APPLICATION_CREDENTIALS="..."`
And in the same terminal when checked with
 
 `printenv GOOGLE_APPLICATION_CREDENTIALS` 
 
 it print the correct value which I just set.

But when open a new terminal, this variable is gone. not exist anymore.

And I tried add the following content to ~/.profile 
```
GOOGLE_APPLICATION_CREDENTIALS="..."
export GOOGLE_APPLICATION_CREDENTIALS
```

Same issue happens, when open a new terminal, this environ variable does not exist,

unless I execute `source ~/.profile`, but this is not less work than simply export the variable once again.

How can I export once and in the future it will remain existing?

Solved:
add it to ~/.bashrc. because ./bashrc will be source every time you open a shell. but ./profile will not.