---
title: "Up and Running With NX on Rhel"
date: "2014-11-24T18:51:56+11:00"
draft: false
categories:
- "how-to"
tags:
- linux
#thumbnailImagePosition: left
#thumbnailImage: ../../media/NX.png
---

Over a period of time I’ve seen several folks run into issues trying to setup NX to be able to remotely access their Linux machines. So, I thought it might be a good idea to list down the steps here.

<!--more-->

Following steps explain how you can setup free-nx server on Linux – for this post we use RedHat Enterprise Linux 6 – and then the NoMachine client on your Laptop/Desktop.

First, lets start by setting up the free-nx server on your Linux machine.

### Install Dependencies

Login to your Linux machine. Start by installing expect, if you don’t have it installed already.

```
   # yum install expect
```

### Download and install free-nx

```
   # wget http://dl.atrpms.net/all/freenx-server-0.7.3-18.el6.x86_64.rpm
   # wget http://dl.atrpms.net/all/nx-3.3.0-38.el6.x86_64.rpm

   # yum -y localinstall nx-3.3.0-38.el6.x86_64.rpm
   # yum -y localinstall freenx-server-0.7.3-18.el6.x86_64.rpm
```

### Setting up NX

```
   # export PATH=$PATH:/usr/sbin
   # /usr/libexec/nx/nxsetup --install
```

As mentioned here, you may have to add /usr/sbin to your PATH as “usermod” command (internally used by nxsetup) is located in /usr/sbin. For easier/quicker setup, use the defaults as you walk thru the prompts while running nxsetup.

### Start the server (or confirm that it is running) using the following command

```
   # /usr/libexec/nx/nxserver --start
```

Now, Install the NoMachine client on your Laptop/Desktop.

Login to your client machine from where you need to remotely access the Linux host and Download the No Machine client for your platform from [here](https://www.nomachine.com/download). Clients are available for OS X, Windows, Linux, Android & iOS. Install, Launch the client and enter valid user credentials.

If you have trouble logging into your Linux machine, try the following:

- Change your client settings so that it uses _NoMachine Login_ instead of the default System Login.
- NX writes to the user’s home directory. So if the user you are logging-in as has a HOME dir that is mounted upon login and that mounting fails, your nx login will not work.
