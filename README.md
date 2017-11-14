# DabianDockerMac
Virtualbox appliance running Debian with Docker for Mac.
The virtual machine contains docker, docker-compose and a NFS server to locally mount the docker user's home folder.

## Pre-requisites
Installation of [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Installation and configuration
1. Add entry on /etc/hosts - need to use sudo to edit the file: `10.10.10.10 docker.local`
2. Access VirtualBox's network configuration for the Host-only Networks adapter vboxnet0 by accessing the menu VirtualBox -> Preferences -> Network -> Host-only Networks
    1. On Adapter tab:
        1. IPv4 Address: 10.10.10.1
        2. IPv4 Network Mask: 255.255.255.0
    2. On DHPC Server tab:
        1. Uncheck "Enable Server"
3. Download the [latest release](https://github.com/esimonetti/DabianDockerMac/releases/latest) of this git repository
4. Import the downloaded VirtualBox appliance by accessing the menu File -> Import Appliance and follow the prompts
5. Access the network configuration of the VirtualBox imported appliance by right clicking on the virtual machine -> Settings -> Network
    1. On the Adapter 1 tab:
        1. Make sure the Host-only Adapter Name is `vboxnet0` (or the same that has been setup with the settings on step 2)
        2. Click OK

To boot the virtual machine double click on the virtual machine and wait few seconds for it to complete.
To shutdown the running virtual machine click the X on the running virtual machine, select Send the shutdown signal, click OK and wait few seconds for it to complete.

## Virtual machine details
* Hostname: `docker.local`
* Ip address: `10.10.10.10`
* Credentials
    * Username: `docker`
    * Password: `docker`

## Virtual machine access

### SSH
To ssh into the virtual machine run `ssh docker@docker.local`

### NFS mount
To mount and access the virtual machine folder `/home/docker` from your MAC complete the following steps after every boot:
1. Open Finder
2. Access the menu Go -> Connect to Server
3. On Server Address enter: `nfs://docker.local:/home/docker`
4. Click Connect
