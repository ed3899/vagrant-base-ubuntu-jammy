# Vagrant Base Image
This is the base image for local development work. The Dockerfile is
only meant to be run in a production environment.

## Overview
Using Ubuntu Jammy, provisioning with ansible and providing with VirtualBox

## Setup

### Windows
If you want to change the default location where Vagrant stores
the boxes . Run the following on CMD or Powershell. By default is stores them
on *C:/Users/USERNAME/.vagrant.d/boxes*

`Set-Item Env:VAGRANT_HOME 'D:/Example/Location'`

This will only change the environment variable per terminal session.

If you wish to change it system wide then go to:

Home > Edit the System Environment Variables (admin password prompmt required) > Environment Variables > System Variables > New

Then set the values accordingly

Variable name: VAGRANT_HOME
Variable value: D:/Example/Location'

Reboot your host machine

This is important if you have limited space on your default hard drive.

### Firewall issues while downloading the box
Please refer to this [thread](https://stackoverflow.com/questions/72290594/unknown-error-0x80092012-trying-to-configure-vagrant-with-git-bash/75837342#75837342) where it is explained in detail.

If you want a quick and dirty fix, disable your firewall or antivirus.

### Virtual Box
If you're using VirtualBox as a provider make sure you change
the default location where it stores the VMs.

*File > Preferences*

Select the dropdown **Default Machine Folder** and choose your
custom location

This is important if you have limited space on your default hard drive.

## Environments
On the *Vagrantfile*, look for:

`ansible.tags = ["tag_of_the_environment"]`

Change that according to the environment you wish to run.

## Rust
`ansible.tags = ["rust"]`