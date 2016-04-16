title: "Install mongodb on ubuntu 14.04"
date: 2015-09-16 10:50:50
tags:
  - ubuntu
  - mongodb
categories:
  - 原创
  - 软件安装
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-4-12/97053353.jpg
---

Install mongodb on ubuntu 14.04
<!-- excerpt -->

## Importing the public key
```shell
hhzzk@hhzzk:~$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
[sudo] password for hhzzk:
Executing: gpg --ignore-time-conflict --no-options --no-default-keyring --homedir /tmp/tmp.6hxzMphFZU --no-auto-check-trustdb --trust-model always --keyring /etc/apt/trusted.gpg --primary-keyring /etc/apt/trusted.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
gpg: requesting key 7F0CEB10 from hkp server keyserver.ubuntu.com
gpg: key 7F0CEB10: public key "Richard Kreuter <richard@10gen.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```

## Creating a list file and update system
```shell
hhzzk@hhzzk:~$ echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse
hhzzk@hhzzk:~$ sudo apt-get update
```

## Installing mongodb
```shell
sudo apt-get install -y mongodb-org
```

## Start and stop mongodb service
```shell
hhzzk@hhzzk:~$ service mongod status
mongod start/running, process 5220
hhzzk@hhzzk:~$
hhzzk@hhzzk:~$ sudo service mongod stop
mongod stop/waiting
hhzzk@hhzzk:~$
hhzzk@hhzzk:~$ sudo service mongod start
mongod start/running, process 5328
hhzzk@hhzzk:~$
hhzzk@hhzzk:~$ sudo service mongod restart
mongod stop/waiting
mongod start/running, process 5365

```

***
(EOF)
