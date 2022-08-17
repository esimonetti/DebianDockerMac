# DebianDockerMac
Virtualbox appliance running Debian with Docker for Mac.
The virtual machine contains docker, docker-compose and a NFS server to locally mount the docker user's home folder.

## Pre-requisites
* A Mac
* Installation of [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Installation and configuration
1. Make sure no other VirtualBox networks adapter are on the subnet 10.10.10.x or you do not run any internal network on that subnet (the routing table be checked with `netstat -rn` and the routing for the subnet 10.10.10.x if exists can be found with `netstat -rn | grep 10.10.10`)
2. Add entry on /etc/hosts - need to use sudo to edit the file: `10.10.10.10 docker.local`
3. Access VirtualBox's network configuration for the Host Network adapter `vboxnet0`. Depending on the installed version of VirtualBox it can be accessed via the top-right icon Global Tools -> Host Network Manager or via the menu VirtualBox -> Preferences -> Network -> Host-only Networks
    1. On Adapter tab configure manually the followings:
        1. IPv4 Address: 10.10.10.1
        2. IPv4 Network Mask: 255.255.255.0
   
    ## For versions above 6.1.28 create the file etc/vbox/networks.conf and add the line below to alow the IPv4 range

           * 10.0.0.0/8
    
     2. On DHPC Server tab:
        1. Uncheck "Enable Server"
4. Download the [latest release](https://github.com/esimonetti/DebianDockerMac/releases/latest) of this git repository
5. Import the downloaded VirtualBox appliance by accessing the menu File -> Import Appliance and follow the prompts
6. Access the network configuration of the VirtualBox imported appliance by right clicking on the virtual machine -> Settings -> Network
    1. On the Adapter 1 tab:
        1. Make sure the Host-only Adapter Name is `vboxnet0` (or the matching adapter setup on step 3)
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
To mount and access the virtual machine folder `/home/docker` from your Mac complete the following steps after every boot:
1. Open Finder
2. Access the menu Go -> Connect to Server
3. On Server Address enter: `nfs://docker.local:/home/docker`
4. Click Connect

## Network issues
If it is not possible to ssh or connect via NFS to the virtual machine, check the routing by running `netstat -rn | grep 10.10.10`.
The expected output should be similar to:
```
10.10.10/24        link#15            UC              3        0 vboxnet
10.10.10.10        8:0:27:7e:51:3c    UHLWI           0       71 vboxnet   1041
10.10.10.255       ff:ff:ff:ff:ff:ff  UHLWbI          0       18 vboxnet
```
If the routing table does not look similar to the above, re-check the Host-only Networks adapter settings, and that the virtual machine correct adapter is selected. It might also be the case that VirtualBox requires an OS reboot before applying the routing correctly.
