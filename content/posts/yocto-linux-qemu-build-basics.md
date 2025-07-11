---
author: amrithmmh
category:
  - linux
date: "2022-06-16T03:38:04+00:00"
guid: https://armphibian.wordpress.com/?p=86
tag:
  - custom-linux
  - embedded-linux
  - embedded-systems
  - yocto-linux
title: Yocto Linux Qemu build basics
url: /2022/06/16/yocto-linux-qemu-build-basics/

---
![](/wp-content/uploads/2022/06/yocto_project_logo.svg_.png?w=1024)Yocto project logo . Source: Wikipedia

_Using Ubuntu /Popos as a development_ machine

Install required packages for Ubuntu/Debian-based Linux

```
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm
sudo apt install -y zstd liblz4-tool
```

Download poky build system source from git

```
git clone git://git.yoctoproject.org/poky
```

cd into poky directory and run the init command

```
source oe-init-build-env build-qemux86
```

the directory is switched to build-qemux86 (else cd into build-qemux86). Check the configuration file and set the machine to qemux86 (or qemux86-64 for 64bit)

```
cd build-qemux86
vim conf/local.conf

MACHINE ??= "qemux86"
```

Start qemu image build using below command

```
bitbake core-image-minimal
```

The above build process will download 1.5Gb-2Gb source code and build tools in parallel. This might take time depending on your PC

To build a GUI X11 based with very minimal controls for your Yocto image do the following:

```
bitbake core-image-sato
```

more to come do follow to keep updated... :-)
