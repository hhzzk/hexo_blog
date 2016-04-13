title: "Install OpenStack on ubuntu server 14.04 using devstack"
date: 2015-10-1 10:50:50
tags:
  - ubuntu
  - Openstack
categories:
  - 原创
  - 软件安装
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-12/5621776.jpg
---

Install OpenStack on ubuntu server 14.04 using devstack
<!-- excerpt -->

DevStack is a set of scripts and utilities to quickly deploy an OpenStack cloud on single machine.

In order to access the external network，we should set proxy in our system by putting the following in the .bashrc:
```
http_proxy= <-- your http proxy
https_proxy= <-- your https proxy
no_proxy="local,localhost,127.0.0.1,YOUR-IP-ADDRESS"
exporthttp_proxy
exporthttps_proxy
exportno_proxy
exportGIT_SSL_NO_VERIFY=1
```
Don't forget replace YOUR-IP-ADDRESS, and then reboot your system or execute the following command:
```
cd
source .bashrc
```
Upgrade the packages on your system and install git:
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install git
```
Download devstack(use master branch on 2016/03/18):
```
git clone https://git.openstack.org/openstack-dev/devstack
```
The devstack repo contains a script that installs OpenStack and templates for configuration files.
Create local.conf and add configuration:
```
cd devstack/
vim local.conf
[[local|localrc]]
 
# Set related password
SERVICE_TOKEN=your-password
ADMIN_PASSWORD=your-password
MYSQL_PASSWORD=your-password
RABBIT_PASSWORD=your-password
SERVICE_PASSWORD=$ADMIN_PASSWORD
 
# Replace your ip address
HOSP_IP=YOUR-IP-ADDRESS
 
# Enable neutron
disable_service n-net
enable_service q-svc
enable_service q-agt
enable_service q-dhcp
enable_service q-l3
enable_service q-meta
enable_service q-metering
enable_service neutron
```
Change git base url
The solution is to modify the sourcerc file in the devstack installation folder to use https instead of git.
Default setting in stackrc is :
```
GIT_BASE=${GIT_BASE:-git://git.openstack.org}
```
Modified setting that should bypass git restrictions:
```
GIT_BASE=${GIT_BASE:-https://git.openstack.org}
```
The following commands will install OpenStack in about 1 hour:
```
cd devstack/
./stack.sh
```
If the script completes execution successfully, you will find the output ending with the following lines:
```
Horizon is now available at http://your-ip-address/
Keystone is serving at http://your-ip-address:5000/v2.0/
Examples on using novaclient commandline is inexercise.sh
The default usersare: admin and demo
The password: your password
This is your host ip: your-ip-address
stack.sh completed inxxx seconds.
```
Then you can open a browser and type your ip address, you will get the login page of openstack dashboard.

Add external bridge br-ex:
```
# Restart OVS service
sudo service openvswitch-switch restart
# Add external bridge
ovs-vsctl add-br br-ex
sudo ovs-vsctl add-port br-ex INTERFACE_NAME
# Note: do not use management NIC (which allocted HOST_IP)
 
sudo apt-get installethtool
# Stop generic receive offload(GRO)
sudo ethtool -K INTERFACE_NAME gro off
```
With external bridge, instance can access external network.
Due to use EIGRP in csr, we should disable firewall use following command:
```
sudo ufw disable
```
or
```
sudo iptables -F
```

***
(EOF)
