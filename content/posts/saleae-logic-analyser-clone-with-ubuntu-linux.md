---
author: amrithmmh
category:
  - uncategorized
cover:
  alt: index
  image: /wp-content/uploads/2022/06/index.png
date: "2022-06-26T08:23:30+00:00"
guid: https://armphibian.wordpress.com/?p=167
tag:
  - linux
  - logic-analyser
  - salae-logic
  - ubuntu
title: Saleae Logic Analyser Clone with Ubuntu Linux
url: /2022/06/26/saleae-logic-analyser-clone-with-ubuntu-linux/

---
![](/wp-content/uploads/2022/06/screenshot-from-2022-06-26-13-51-46.png?w=1024)

The [Saleae Logic](https://www.saleae.com/logic) is an 8 channel 24MHz logic analyser.  Soon after its launch people in China opened them up to find that they are pretty simple inside and, as sure as night follows day, little workshops in Shenzen started producing clones impossibly cheaply and to be sold through eBay, AliExpress, etc

Follow this [link](https://sigrok.org/wiki/Linux#Distribution_packages) for excellent instruction on other distros and requirements

Type the below command to check if the USB was connected successfully!

```
sudo dmesg | tail
```

To install Sigrok in ubuntu just do: (in case of any error use appimage , make sure your USB device have proper permission to be used else you cannot find it !)

```
sudo apt install sigrok pulseview
sudo pulseview
or
pulseview
```

### AppImage

Sigrok provide AppImages (see [appimage.org](https://appimage.org) for details) for [sigrok-cli](https://sigrok.org/wiki/Sigrok-cli) and [PulseView](https://sigrok.org/wiki/PulseView) which make it very easy and convenient to use sigrok on somewhat recent Linux distributions (most distros newer than Ubuntu 16.04 LTS (Xenial Xerus) from around 2016 should work fine).

After downloading the AppImage (see [Downloads](https://sigrok.org/wiki/Downloads#Binaries_and_distribution_packages)) you can run it by simply making it executable and executing it, for example:

```
$ chmod u+x PulseView-NIGHTLY-x86_64.AppImage
$ ./PulseView-NIGHTLY-x86_64.AppImage

```

You might need to install the libsigrok **udev rules files** to be able to access some devices. See _[Cannot access USB / serial / other device](https://sigrok.org/wiki/Building#Cannot_access_USB_.2F_serial_.2F_other_device)_ for details.
