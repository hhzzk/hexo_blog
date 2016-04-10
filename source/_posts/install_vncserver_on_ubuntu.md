title: "How to install vncserver on ubuntu server 14.04"
date: 2015-07-26 0:50:50
tags:
  - ubuntu
  - Vncserver
categories:
  - 原创
  - 软件安装
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-10/62106305.jpg
---

Install vncserver on ubuntu server 14.04
<!-- excerpt -->

<!-- toc -->

## Install VNC server

- In order to access the external network，we should set proxy in our system by putting the following in the `.bashrc` :
```shell
http_proxy= <-- your http proxy address
https_proxy=  <-- you https proxy address
no_proxy="local,localhost,127.0.0.1,YOUR-IP-ADDRESS"
export http_proxy
export https_proxy
export no_proxy
export GIT_SSL_NO_VERIFY=1
```
- Don't forget replace YOUR-IP-ADDRESS, and then reboot your system or execute the following command:
```shell
source .bashrc
```
- Upgrade the packages on your system :
```shell
sudo apt-get update
```
- Then you'll need to install the Gnome components using terminal :
```shell
sudo apt-get install gnome-core
```
- Install VNC server :
```shell
sudo apt-get install vnc4server
```
## Configure VNC Server

* Set VNC server password:
```shell
testbed@ubuntu:~$ vncserver
 
You will require a password to access your desktops.
 
Password:  <--- put your password
Verify:    <--- put your password again
xauth:  file /home/testbed/.Xauthority does not exist
 
New 'ubuntu:1 (testbed)' desktop is ubuntu:1
 
Creating default startup script /home/testbed/.vnc/xstartup
Starting applications specified in /home/testbed/.vnc/xstartup
Log file is /home/testbed/.vnc/ubuntu:1.log
 
testbed@ubuntu:~$
```

* Configure xstartup file
Before we begin configuring new xstartup file, back up the original first:
```shell
testbed@ubuntu:~$ cd .vnc/
testbed@ubuntu:~/.vnc$ cp xstartup xstartup.bak
testbed@ubuntu:~/.vnc$ vim xstartup
# copy the following configuration and save it into the file:

#!/bin/sh
 
# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &
```
* Restart vncserver
Now, we should kill the VNC server session that is running, and recreate it using the screen size you want:
```shell
testbed@ubuntu:~/.vnc$ vncserver -kill :1
Killing Xvnc4 process ID 20709
testbed@ubuntu:~/.vnc$ vncserver -geometry 1300x1000
 
New 'ubuntu:1 (testbed)' desktop is ubuntu:1
 
Starting applications specified in /home/testbed/.vnc/xstartup
Log file is /home/testbed/.vnc/ubuntu:1.log
 
testbed@ubuntu:~/.vnc$
```
Tips: you can get multiple VNC sessions by multiple commands `vncserver [option]`, you will got diffrent display ports, and these ports will used by VNC client as following.

## Add vncserver as init service 
we also need to automatically start vncserver if system reboot

*  Create init script
```shell
#!/bin/bash
PATH="$PATH:/usr/bin/"
export USER="testbed"
DISPLAY="1"
GEOMETRY="1680x1050"
OPTIONS="-geometry ${GEOMETRY} :${DISPLAY}"
. /lib/lsb/init-functions
 
case "$1" in
start)
log_action_begin_msg "Starting vncserver for user '${USER}' on localhost:${DISPLAY}"
su ${USER} -c "/usr/bin/vncserver ${OPTIONS}"
;;
 
stop)
log_action_begin_msg "Stopping vncserver for user '${USER}' on localhost:${DISPLAY}"
su ${USER} -c "/usr/bin/vncserver -kill :${DISPLAY}"
;;
 
restart)
$0 stop
$0 start
;;
esac
exit 0
```
- Add it to default service
```shell
sudo chmod +x /etc/init.d/vncserver
sudo update-rc.d vncserver defaults
```
## Connect to Your VNC Desktop with VNC Client

You can use your [VNC viewer](https://www.realvnc.com/download/viewer/) to connect to the VNC server at IP_ADRESS:display_port.

***
(EOF)

