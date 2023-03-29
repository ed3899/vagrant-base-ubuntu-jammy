# Rust Environment
Make sure you clone the parent repo as well, as it contains the
base ansible playbook.

## Windows
If you want to change the default location where Vagrant stores
the boxes . Run the following on CMD or Powershell. By default is stores them
on *C:/Users/USERNAME/.vagrant.d/boxes*

`Set-Item Env:VAGRANT_HOME 'D:/Example/Location'`

### Virtual Box
If you're using VirtualBox as a provider make sure you change
the default location where it stores the VMs.

*File > Preferences*

Select the dropdown **Default Machine Folder** and choose your
custom location