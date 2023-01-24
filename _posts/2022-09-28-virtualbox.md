---
layout: post
title:	"VirtualBox"
date:	 2022-09-28 00:00:00 +0800
categories: linux
---

This post will go through the main elements to install a Linux image on VirtualBox to get a real Linux system on your host.

## Install Linux on VirtualBox
- Install Oracle VirtualBox from [https://www.virtualbox.org/](https://www.virtualbox.org/)
- Download a Linux image: [Fedora](https://getfedora.org/en/workstation/download/), [Rocky Linux](https://rockylinux.org/), [CentOS](http://isoredirect.centos.org/centos/7/isos/)
- Create a new VM based on this image: in VitualBox select New and pick the .iso of your choice

## Configure Linux

### Add a sudoer
Set user as sudoer as depicted on https://linuxize.com/post/how-to-add-user-to-sudoers-in-centos/.  
Add `username  ALL=(ALL) NOPASSWD:ALL` at the end of `/etc/sudoers`    
You can edit via `visudo` as root (`sudo -i` to get a root prompt).  

### Change Hostname
cf. https://www.hostinger.com/tutorials/change-hostname-on-centos-7/.
`hostnamectl set-hostname fedora.local`

## Install VirtualBox Guest Addition
It enables features like file sharing and copy/paste from/to your host.

Start the VM.  
In Menu select _Devices>Insert Guest Additions CD Image_.  
Configure a **Shared folder** with _Auto-mount_ and _Make Permanent_ ticked.  
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-ga-shared-folder.png">
    </div>
</div>

Grant rights for you user on the vboxsf group to access the shared folder `sudo usermod -aG vboxsf $USER`  
Configure **Shared clipboard** as _Bidirectional_.  

## (Optional) Configure Linux Further

### Install handy packages
install _netstat_, _ifconfig_ `sudo yum install net-tools -y`  
install _lsof_ `sudo yum install lsof -y`  
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

### ssh
Let's check first ssh is up and running or start it now
```bash
systemctl status sshd
systemctl enable sshd --now #start and register as auto-start ssh for next VM boot
```

**ssh** can be configured using Bridged Adapter or NAT.
Note service called ssh or sshd according your Linuc distribution.

Most credit from [here](https://www.golinuxcloud.com/ssh-into-virtualbox-vm/).  

#### Using Bridged Adapter
`systemctl start sshd` https://www.golinuxcloud.com/ssh-into-virtualbox-vm

<div class="alert alert-danger" role="alert"><i class="fa fa-exclamation-circle">
Unfortunately this configuration has been proven sometimes instable and the brideg adapter network interface stops starting when rebooting the VM.</i></div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-bridged-adapter.png">
    </div>
</div>
<br/>

#### Using NAT
To ssh in the guest machine on 127.0.0.1:2222
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/2022-09-28-vbox-nat.png">
    </div>
</div>
<br/>

## FAQ

### Ubuntu tips
https://askubuntu.com/a/1119250
sudo su
cd /media
mkdir cdrom
mount /dev/cdrom /media/cdrom
cd cdrom
sh VBoxLinuxAdditions.run

"Kernel headers not found"
yum install "kernel-devel-uname-r == $(uname -r)" 
https://unix.stackexchange.com/a/568358/538394

### Autoboot of Network Interface
/etc/sysconfig/network-scripts/ifcfg-eth0 > ONBOOT=yes
`nmcli conn up` or `systemctl restart network.service`
[https://emcorrales.com/blog/how-to-enable-internet-access-on-centos7-virtualbox](https://emcorrales.com/blog/how-to-enable-internet-access-on-centos7-virtualbox)
This will ensure you can ssh in your box. 