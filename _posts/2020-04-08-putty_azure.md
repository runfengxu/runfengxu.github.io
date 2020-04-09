---
layout:     post
title:      Putty connect Azure
subtitle:   
date:       2020-04-08
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:
		- centos
		- linux
---

## Use PuTTY to access EC2 Linux Instances via SSH from Windows

*original post here(https://linuxacademy.com/guide/17385-use-putty-to-access-ec2-linux-instances-via-ssh-from-windows/)**
####  If you don’t have the PuTTY software installed on your system, you will need to download it from www.putty.org. 
Be sure to select the entire package as shown below, as it will include all the needed utilities such as puttygen and pageant.
<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f13d65604.png" alt="drawing" style="width:800px;"/>

If you have not already downloaded (or cannot locate) your key pair (i.e my_key_pair.pem) you will need to create a new 
EC2 instance and download a new one. A key pair consists of a public key that AWS stores and a private key file that you store (downloaded as PEM file). PEM stands for Privacy Enhanced Mail and is a widely used X.509 encoding format used for security certificates.
Together, the two keys enable you to securely connect to your EC2 instance using SSH.

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f154c4918.png" >

#### convert .pem file to .pkk

PuTTY does not natively support the PEM format that AWS uses, so you need to first convert your PEM file to a PPK file (PPK = PuTTY Private Key). To do this, you use the PuTTYgen utility. To start the utility you can type puttygen in the Windows start dialog box:

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f17c37655.png" class="community-post-image" alt="user_30505_5915f17c37655.png">

On the PuTTYgen dialog box, click the Load Button and then select the .pem file that you downloaded from AWS. Note: when browsing for your pem file be sure to select All Files in the dropdown list that is located to the right of the File name field. PuTTYgen will then load and convert your file.

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f2a25961d.png" class="community-post-image" alt="user_30505_5915f2a25961d.png">

As the message indicates, you then need to click on “Save private key”. You will receive a warning message asking if you want to save this key without a passphrase. Be sure to select Yes.

Provide a name for your ppk file and click save.

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f2f8f3d08.png" class="community-post-image" alt="user_30505_5915f2f8f3d08.png">

#### LAUNCH PuTTY

Now that you have converted the pem file to a ppk file, you are ready to use the PuTTY utility. In the Windows start dialog box, type in putty to start the utility.

ENTER HOST NAME
Enter your Host Name into the appropriate field. This will be in the format of: user_name@public_dns_name. Be sure to specify the appropriate user name for your AMI type. For example:

•For an Amazon Linux AMI, the user name is ec2-user.

•For a RHEL AMI, the user name is ec2-user or root.

•For an Ubuntu AMI, the user name is ubuntu or root.

•For a Centos AMI, the user name is centos.

•For a Fedora AMI, the user name is ec2-user.

•For SUSE, the user name is ec2-user or root.

•Otherwise, if ec2-user and root don’t work, check with the AMI provider.

Here is an example for connecting to an Amazon Linux AMI:

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f325dfb0b.png" class="community-post-image" alt="user_30505_5915f325dfb0b.png">

Next, click on the + button next to the SSH field to expand this section. Then click on Auth (which stands for authenticate) and
enter the name of your private key file (i.e. the ppk file) where it says Private key file for authentication
(if you click on browse you can easily search for the directory where you have stored it).

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f3468460b.png" class="community-post-image" alt="user_30505_5915f3468460b.png">

Lastly, click on Open to start your SSH session.

Note: if this is the first time that you are logging into the instance, you will receive the following alert.

Click on Yes to continue.

<img src="https://la-community-cdn.linuxacademy.com/img/user_30505_5915f369d3e07.png" class="community-post-image" alt="user_30505_5915f369d3e07.png">
