---
layout: post
title:	"VirtualBox"
date:	 222-09-280 00:00:00 +0800
categories: linux
---

Vue.js is a great and trending framework.
If you followed my first article [Vue.js Getting Started](../../2021/vue-getting-started) you have touched already how it is simple to scaffold and start developping a front end application with rich HTML and styling. 

In this second chapter on Vue.js we will discuss the **routing capability**.

## Install CentOS on VirtualBox
Install Oracle VirtualBox from https://www.virtualbox.org/ 
Download CentOS image from http://isoredirect.centos.org/centos/7/isos/
Create a new VM basd on this image

## Install VirtualBox Guest Addition (optional)
https://askubuntu.com/a/1119250
```
sudo su
cd /media
mkdir cdrom
mount /dev/cdrom /media/cdrom
cd cdrom
sh VBoxLinuxAdditions.run
```
share disk https://www.linuxshelltips.com/install-virtualbox-guest-additions/

## Configure CentOS

### Add a sudoer
adding user as sudoer as depicted on https://linuxize.com/post/how-to-add-user-to-sudoers-in-centos/
add username  ALL=(ALL) NOPASSWD:ALL at the end of /etc/sudoers that you can edit via visudo
You can scaffold a new app to test **Vue router** or use one you already have:

### Install handy packages
install netstat, ifconfig
`sudo yum install net-tools -y`
install lsof
`sudo yum install lsof -y`
"Kernel headers not found" error, cf. https://unix.stackexchange.com/a/568358/538394
`yum install "kernel-devel-uname-r == $(uname -r)" `
install bcc-tools and configure .bashrc
`sudo yum install bcc-tools`
```bash
.bashrc
# .bashrc

# Source global definitions
export BCC_BIN=/usr/share/bcc/tools
export PATH=$BCC_BIN:$PATH
alias psudo="sudo env \"PATH=$PATH\""
```

### Change  Hostname
https://www.hostinger.com/tutorials/change-hostname-on-centos-7/
hostnamectl set-hostname centos7.local

### ssh
`systemctl start sshd` https://www.golinuxcloud.com/ssh-into-virtualbox-vm
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-network-adapter.png">
    </div>
</div>

### Autoboot of Network Interface
/etc/sysconfig/network-scripts/ifcfg-eth0 > ONBOOT=yes
`nmcli conn up` or `systemctl restart network.service`
https://emcorrales.com/blog/how-to-enable-internet-access-on-centos7-virtualbox
This will ensure you can ssh in your box. 