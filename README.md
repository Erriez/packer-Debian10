# packer-Debian10

## What is packer-Debian10 ?

packer-Debian10 is a set of configuration files used to build an automated Debian 10 virtual machine images using [Packer](https://www.packer.io/).
This Packer configuration file allows you to build images for VMware Workstation and Oracle VM VirtualBox.

## Prerequisites

* [Packer](https://www.packer.io/downloads.html)
  * <https://www.packer.io/intro/getting-started/install.html>
* A Hypervisor
  * [VMware Workstation](https://www.vmware.com/products/workstation-pro.html)
  * [Oracle VM VirtualBox](https://www.virtualbox.org/)

## Create Debian 10 VM for VMWare Workstation

To create a Debian 10 VM image using VMware Workstation use the following commands:

```cmd
cd c:\packer-Debian10
packer build -only=vmware-iso debian10.json
```

Import the generated `output-vmware-iso/*.ovf` in VMWare and start the VM. 


## Create Debian 10 VM for VirtualBox

To create a Debian 10 VM image using Oracle VM VirtualBox use the following commands:

```cmd
cd c:\packer-Debian10
packer build -only=virtualbox-iso debian10.json
```

Import the generated `output-virtualbox-iso/*.ovf` in VirtualBox and start the VM.

*Note: Both VMWare and Virtualbox VMs will be created by omitting the keyword "-only=".*

## Configuration

The Debian 10 ISO is pulled from <https://cdimage.debian.org/mirror/cdimage/archive/10.11.0/amd64/iso-cd/debian-10.11.0-amd64-netinst.iso> by default.

You can change the URL to one closer to your build server. To do so change the `iso_url` parameter in the `variables` section of the `debian10.json` file.

A `http/preseed-xxx.cfg` file can be selected with variable `preseed_file` in `debian10.json`.

```json
{
  "variables": {
      "iso_url": "https://cdimage.debian.org/mirror/cdimage/archive/10.11.0/amd64/iso-cd/debian-10.11.0-amd64-netinst.iso",
      "iso_checksum": "133430141272d8bf96cfb10b6bfd1c945f5a59ea0efc2bcb56d1033c7f2866ea",
      "preseed_file": "preseed-cli.cfg",
  }
}
```

### Default credentials

The default credentials for this VM image are:

|Username|Password|
|--------|--------|
|packer|packer|
|root|packer|

This can be changed in `debian10.json` variables `ssh_username` and `ssh_password`.

### Add desktop

Edit `http/preseed-cli.cfg` and add a desktop such as `xfce-desktop` at the end of the line `tasksel tasksel/first`:

```
Available tasks as of this writing include:
#    standard (standard tools)
#    desktop (graphical desktop)
#    gnome-desktop (Gnome desktop)
#    xfce-desktop (XFCE desktop)
#    kde-desktop (KDE Plasma desktop)
#    cinnamon-desktop (Cinnamon desktop)
#    mate-desktop (MATE desktop)
#    lxde-desktop (LXDE desktop)
#    web-server (web server)
#    ssh-server (SSH server)
tasksel tasksel/first multiselect standard, ssh-server xfce-desktop
```