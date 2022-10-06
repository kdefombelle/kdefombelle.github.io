---
layout: post
title:	"VirtualBox"
date:	 2022-09-28 00:00:00 +0800
categories: linux
---

This post will go through tyhe main elements to install a CentOS image on VirtualBox to get a real Linux system on your (other) OS.  

## Install CentOS on VirtualBox
- Install Oracle VirtualBox from [https://www.virtualbox.org/](https://www.virtualbox.org/)
- Download CentOS image from [http://isoredirect.centos.org/centos/7/isos/](http://isoredirect.centos.org/centos/7/isos/)
- Create a new VM basd on this image

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
add username  `ALL=(ALL) NOPASSWD:ALL` at the end of `/etc/sudoers` 
you can edit via `visudo`

### Install handy packages
install _netstat_, _ifconfig_ `sudo yum install net-tools -y`<br/> 
install _lsof_ `sudo yum install lsof -y`<br/>
install _bcc-tools_ 
- "Kernel headers not found" error, cf. https://unix.stackexchange.com/a/568358/538394
`yum install "kernel-devel-uname-r == $(uname -r)" `
- configure .bashrc `sudo yum install bcc-tools`

install _bzip2_ `sudo yum install bzip2 -y`

### Configure .bashrc
```bash
.bashrc
# Source global definitions
export BCC_BIN=/usr/share/bcc/tools
export PATH=$BCC_BIN:$PATH
alias psudo="sudo env \"PATH=$PATH\""
```

### Change Hostname
https://www.hostinger.com/tutorials/change-hostname-on-centos-7/
hostnamectl set-hostname centos7.local

### ssh
#### Using Bridged Adapter
`systemctl start sshd` https://www.golinuxcloud.com/ssh-into-virtualbox-vm

<div class="alert alert-danger" role="alert"><i class="fa fa-exclamation-circle">
Unfortunately this configuration has been proven sometimes instable and the brideg adapter network interface stops starting when rebooting the VM.
</i></div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-bridged-adapter.png">
    </div>
</div>
<br/>

#### Using Bridged Adapter with Port Forwarding
To ssh in the guest machine on 127.0.0.1:2222
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-nat.png">
    </div>
</div>
<br/>

### Autoboot of Network Interface
/etc/sysconfig/network-scripts/ifcfg-eth0 > ONBOOT=yes
`nmcli conn up` or `systemctl restart network.service`
[https://emcorrales.com/blog/how-to-enable-internet-access-on-centos7-virtualbox](https://emcorrales.com/blog/how-to-enable-internet-access-on-centos7-virtualbox)
This will ensure you can ssh in your box. 