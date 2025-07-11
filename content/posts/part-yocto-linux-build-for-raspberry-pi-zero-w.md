---
author: amrithmmh
category:
  - uncategorized
date: "2022-07-23T03:41:45+00:00"
guid: https://armphibian.wordpress.com/?p=246
title: '[Part-1] Yocto Linux Build for Raspberry pi zero w'
url: /2022/07/23/part-1-yocto-linux-build-for-raspberry-pi-zero-w/

---
![](/wp-content/uploads/2019/10/img_20191001_154014.jpg?w=750)

The [Yocto Project](http://yoctoproject.org) is an open-source project that delivers a set of tools that create operating system images for embedded Linux systems. The Yocto Project tools are based on the [OpenEmbedded](http://www.openembedded.org/wiki/Main_Page) (OE) project, which uses the BitBake build tool, to construct complete Linux images. BitBake and OE are combined to form a reference build host known as Poky which includes the following [core components](https://wiki.yoctoproject.org/wiki/Core_Components). This [video](https://www.youtube.com/watch?v=utZpKM7i5Z4) will help explain what it's all about.

Using Ubuntu Linux base (use any as per your preference as long as you can satisfy the prerequisite packages)

installing prerequisite packages:

```
$ sudo apt update
$ sudo apt install git git-lfs tar python3 python3-pip gcc
$ sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool
$ sudo pip3 install sphinx sphinx_rtd_theme pyyaml
```

Create a working directory

```
$ mkdir yocto
$ cd yocto
```

Download Poky and select the desired release

```
$ git clone git://git.yoctoproject.org/poky
$ cd poky
$ git checkout -b kirkstone origin/kirkstone

```

source the directory

```
$ source oe-init-build-env build
```

Adding recipes and layers to add rpi specific drivers and files(make sure you are in build directory that was newly created when you run source command).

```
$ git clone git://git.openembedded.org/meta-openembedded -b kirkstone
$ git clone git://git.yoctoproject.org/meta-raspberrypi -b kirkstone
```

add these layers to the build:

```
$ bitbake-layers add-layer ./meta-openembedded
$ bitbake-layers add-layer ./meta-raspberrypi
$ bitbake-layers add-layer ./meta-openembedded/meta-oe
$ bitbake-layers add-layer ./meta-openembedded/meta-python
$ bitbake-layers add-layer ./meta-openembedded/meta-networking
$ bitbake-layers add-layer ./meta-openembedded/meta-multimedia
```

make sure the layers are added using bitbake-layers command

```
$ bitbake-layers show-layers
NOTE: Starting bitbake server...
layer                 path                                      priority
==========================================================================
meta                  /home/amrith/yocto/poky/meta              5
meta-poky             /home/amrith/yocto/poky/meta-poky         5
meta-yocto-bsp        /home/amrith/yocto/poky/meta-yocto-bsp    5
meta-oe               /home/amrith/yocto/poky/build/meta-openembedded/meta-oe  5
meta-multimedia       /home/amrith/yocto/poky/build/meta-openembedded/meta-multimedia  5
meta-networking       /home/amrith/yocto/poky/build/meta-openembedded/meta-networking  5
meta-python           /home/amrith/yocto/poky/build/meta-openembedded/meta-python  5
meta-raspberrypi      /home/amrith/yocto/poky/build/meta-raspberrypi  9

```

select whatever raspberry pi board image you want to build using the following

```
$ cd  meta-raspberrypi/conf/machine
$ ls
include                  raspberrypi0.conf       raspberrypi3-64.conf  raspberrypi4.conf     raspberrypi.conf
raspberrypi0-2w-64.conf  raspberrypi0-wifi.conf  raspberrypi3.conf     raspberrypi-cm3.conf
raspberrypi0-2w.conf     raspberrypi2.conf       raspberrypi4-64.conf  raspberrypi-cm.conf

```

Note down the above file name for the board without the .conf extension and add it to config file under poky/build/conf/local.conf

Set machine name in local.conf , also enable uart logging

```
MACHINE ??= "raspberrypi0-wifi"

```

inside meta-raspberrypi/recipes-core/images you should be able to see some recipes for rpi, we are building **rpi-test-image**

```
$ bitbake rpi-test-image --runall=fetch
$ bitbake rpi-test-image
```

Build takes few hours depending on the machine and network , so chill for few hours !

**If the build is successful , you can find the image inside /yocto/poky/build/tmp/deploy/images/raspberrypi0-wifi/rpi-test-image-raspberrypi0-wifi.rpi-sdimg**

Burn the image to sd card using dd . follow below link for the tutorial on how to use dd

[https://osxdaily.com/2018/04/18/write-image-file-sd-card-dd-command-line/](https://osxdaily.com/2018/04/18/write-image-file-sd-card-dd-command-line/)

connect usart pins to serial to USB converter like cp2102 and open your favourite serial monitor program .

```
$picocom -b 115200 /dev/ttyUSB0

[   18.354558] Bluetooth: BNEP socket layer initialized
[   18.379023] random: bluetoothd: uninitialized urandom read (4 bytes read)
[   18.554833] NET: Registered PF_ALG protocol family
   ...done.
Starting Telephony daemon
Starting Linux NFC daemon
[   19.284711] nfc: nfc_init: NFC Core ver 0.1
[   19.294654] NET: Registered PF_NFC protocol family
[   19.464256] Bluetooth: RFCOMM TTY layer initialized
[   19.469284] Bluetooth: RFCOMM socket layer initialized
[   19.484303] Bluetooth: RFCOMM ver 1.11

Poky (Yocto Project Reference Distro) 4.0.2 raspberrypi0-wifi /dev/ttyS0

```

**Configuring** **pi image and adding your own layer will be explained in part 2...**

Credits:

1.https://kickstartembedded.com/2021/12/19/yocto-part-1-a-definitive-introduction/

2.https://tutorialadda.com/yocto/create-your-own-linux-image-for-the-raspberry-pi-board-using-yocto-project
