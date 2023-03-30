# Overview
This is the base Vagrant image for local development work.
It uses Ubuntu Jammy64, provisions with ansible and
provides with VirtualBox

## Requirements
- Make sure you have Vagrant installed on the host machine
- Enough space on your default drive (see below if you want to change the location where
  Vagrant stores its boxes and VirtualBox the built VMs)

## How-To

### Shared folders
Make sure you shared the additionals folders you would like to have
in sync with the VM

Add those to .gitignore to avoid conflicting repo issues

### Tags
Before running make sure you select the adequate tags
you wish to run.

On the *Vagrantfile*, look for:

`ansible.tags = ["tag_of_the_play"]`

Change that according to the tags you wish to run. List of
tags in the Vagrantfile. Each play on ansible maps to a single tag.

#### AWS
If using the **aws** tag.

Create:

`ansible\playbooks\secrets\main.yml`

With values:
```
aws:
  access_key_id: aws_test
  secret_access_key: aws_test
  region: us-west-2
  output: json
```

This file is ignored by git

### .gitignore
Vagrant syncs the root folder with the VM. Make sure you add
your source project folder name on `.gitignore` to avoid any conflicts
with this repo.

By default it is called **src/**

### Run
Once you've made your changes.

Make sure you are at the root of the project where the
Vagrantfile is located.

On your command line:
`vagrant up`

Once done:
`vagrant ssh`

You should be able to ssh into the machine.

## Troubleshooting

### Firewall issues while downloading the box
Please refer to this [thread](https://stackoverflow.com/questions/72290594/unknown-error-0x80092012-trying-to-configure-vagrant-with-git-bash/75837342#75837342) where it is explained in detail.

If you want a quick and dirty fix, disable your firewall or antivirus.

### Windows
If you want to change the default location where Vagrant stores
the boxes . Run the following on CMD or Powershell. By default is stores them
on *C:\Users\USERNAME\.vagrant.d\boxes*

`Set-Item Env:VAGRANT_HOME 'D:\Example\Location'`

This will only change the environment variable per terminal session.

If you wish to change it system wide then go to:

*Home > Edit the System Environment Variables (admin password prompt required) > Environment Variables > System Variables > New*

Then set the values accordingly

```
Variable name: VAGRANT_HOME
Variable value: D:\Example\Location
```

Reboot your host machine

This is important if you have limited space on your default hard drive.

### Virtual Box
If you're using VirtualBox as a provider make sure you change
the default location where it stores the VMs.

**File > Preferences**

Select the dropdown **Default Machine Folder** and choose your
custom location

This is important if you have limited space on your default hard drive.
