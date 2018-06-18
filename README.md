# packer-windows10
Creating Windows 10 with VirtualBox, Vagrant, Packer

Refer: https://toannm.me/2017/07/23/creating-windows-10-visual-studio-2017-with-virtualbox-vagrant-packer/

---
## Summary of the toannm.me blog post
If anything is not working, please check the original post as I have summarized what needed to be done in an evironment which already had all the component prerequisites.

### Prerequisites
- Windows 10 ISO file
- Vagrant
- Packer
- VirtualBox
- OpenSSH
- templates, json and autounattend.xml included in this repo

### Installation
- Install vagrant, packer and virtualbox
- I have git already installed `git version 2.10.1.windows.1`
-- I run all commands from the git bash shell 
-- ssh is included with my installation of git
- Update Autounattend.xml with your product key
- Update the "disk_size" value in the windows_10.json file to something that works for you
- Update the "variables" section of the windows_10.json file

### Creat the initial windows box
```
bash$ packer build -only=virtualbox-iso windows-10.json
```
After a successfully create box, there will be a win10_virtualbox.box file

### Import the current box into vagrant
```
vagrant box add win10 ./win10_virtualbox.box
```
### You can now use the box in vagrant
- reference win10 as the name of the box to use
- after initializing vagrant directory, copy the vagrantfile-windows_10.template into Vagrantfile 

---
## Usage example

### Create a new dev environment based on a local dir/git repo

1. Add the VS 2017 box to vagrant
```
$ vagrant box add win10_vs2017 .\win10_vs2017.box
```
2. Create a unique dev box for the app in question
```
$ cd <development dir>
$ vagrant init win10_vs2017
```
3. Copy the content of vagrantfile-vs2017.template into Vagrantfile

---
## Additional info

### To find your windows 10 product key, run this command in an elevated powershell:
```
ps> wmic path SoftwareLicensingService get OA3xOriginalProductKey`
```

### Create the windows ISO with the Media Creation Tool 

https://support.microsoft.com/en-us/help/4028192/windows-create-an-iso-file-for-windows-10

### Get the sha1 hash for the iso
```
bash> sha1sum <isofile>
```

---
## Generating another box from the first
### We may want to do this when software has been updated or added to the original box but we'd like to redistribute a version with the software preinstalled. Note that this is different than the use case in which we want to install software while the box is being built, using ansible for example.

1. find the name of the virtualbox you want to "box" up
    * In step 2. I use "win10_default_1524848325109_18374". Your identifier will be different.
```
$ VBoxManage.exe list vms
"apprwin" {6b7f467c-7b3a-4c67-bd0a-2e483b1ff17d}
"linux-kubectl" {e729346b-69b2-4459-ac76-42be5e943493}
"win10_default_1524848325109_18374" {65009b2e-908c-4690-972e-f735965922c5}
"vpn_default_1526063533829_5360" {a53cbab6-af3a-44f5-b3d7-72e79a01fff0}
"vpn2_default_1526310768222_38704" {3facd587-279d-4bb3-a0e5-2547b484c46a}
```
2. package the box for use with vagrant
Make sure to package it with your working currently working vagrantfile as in this command:
```
$ vagrant package --base win10_default_1524848325109_18374 --vagrantfile ./vagrantfile-vs2017.template --output ./win10_vs2017.virtualbox.box
```
3. Add the box to vagrant
```
$ vagrant box add win10_vs2017 ./win10_vs2017.virtualbox.box
```
