# packer-windows10
Creating Windows 10 with VirtualBox, Vagrant, Packer

Refer: https://toannm.me/2017/07/23/creating-windows-10-visual-studio-2017-with-virtualbox-vagrant-packer/

## Summary of the toannm.me blog post


## Setup

### To find your windows 10 product key, run this command in an elevated powershell:

`wmic path SoftwareLicensingService get OA3xOriginalProductKey`

### After creating the windows ISO with the Media Creation Tool 

https://support.microsoft.com/en-us/help/4028192/windows-create-an-iso-file-for-windows-10

1. get the sha1 hash for the iso
  * `bash> sha1sum <isofile>`


## Usage

### Create a new dev environment based on a local dir/git repo

1. Add the VS 2017 box to vagrant
  * ps> vagrant box add win10_vs2017 .\win10_vs2017.box
2. Create a unique dev box for the app in question
  * ps> cd <development dir>
  * ps> vagrant init win10_vs2017
3. Copy the content of vagrantfile-vs2017.template into Vagrantfile
