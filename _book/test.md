# 简介

本实验手册用于实验esp32开发版的基本功能。

使用班级：电子信息工程16级。

制作者：张利民

# 目录

1. [前言](README.md)
2. [实验一 ：实验环境与实验工具操作](prepare.md)
   - [操作系统](os.md)
   - [串口驱动](serial_driver.md)
3. [实验二 ：烧录固件](flash_firmware.md)
4. 

# ESP 实验准备工作

1. [操作系统](os.md)
2. [串口驱动](seiral_driver.md)

# 操作系统

## 安装 ubuntu 

实验环境建议使用ubunutu操作系统，如果是Windows操作系统，请在虚拟机下安装Ubuntu操作系统。

1. 安装虚拟机vmware，vmware软件请自行在网上下载。
2. 下载[ubuntu18.04](https://www.ubuntu.com/download/desktop/thank-you?country=CN&version=18.04.2&architecture=amd64), 建议通过bt种子[下载](http://releases.ubuntu.com/18.04/ubuntu-18.04.2-desktop-amd64.iso.torrent?_ga=2.5522096.202950691.1556000491-1568164439.1556000491)
3. 依照提示安装

## window以及Mac osx

### windows

如果不希望进入到ubuntu进行实验，可自行处理相关内容，本实验参考给出了windows下的操作提示，但某些详细内容仍需自己查阅资料完成。

### Mac osx

本参考给出了mac osx 下的提示与用法，大部分的用法与Linux相同，需要时请查阅相关资料自行解决。

# 安装usb-ttl 驱动

在开发版上，有一片ch220芯片，用于将ttl的串行接口转换为miniUSB，因此必须在主机安装该驱动程序。

Usb-2-ttl chip ch210x driver:

[Driver download](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)

## linux 下的驱动安装

下载驱动后，解压后，进入文件夹，认真阅读安装指南CP210x_VCP_Linux_4.x_Release_Notes.txt。

Ubuntu 下的安装指南

```shell
make ( your cp210x driver )
cp cp210x.ko to /lib/modules/$(uname -r)/kernel/drivers/usb/serial
insmod /lib/modules/$(uname -r)/kernel/drivers/usb/serial/usbserial.ko
insmod cp210x.ko
```

## windows, mac osx 驱动安装

下载驱动后直接运行安装程序即可。

## 检查串口驱动是否安装成功

将开发版通过USB串口数据线与电脑连接，对于Windows，请自行网上搜索如何获取设备的串口号，即com端口号。

对于Linux

```shell
# 运行如下命令
ls /dev/ttyUSB*
# 如果运行结果中有 /dev/ttyUSB0之类的则表明成功，且该端口号就是开发版串口的端口号。
```

对于Mac osx

```shell
# 运行如下命令
ls /dev/cu.*
# 如果运行结果中有 /dev/cu.SLAB_USBtoUART之类的则表明成功，且该端口号就是开发版串口的端口号。
```

# 串口监控程序安装

## linux

linux下的串口监控程序可以选择使用minicom或者picocom

```shell
# 1 安装minicom
sudo install minicom
# 2 安装picocom
sudo install picocom
```

## windows 

windows的串口监控程序使用putty，请自行在网上下载并安装。

## Mac osx

linux下的串口监控程序可以选择使用minicom或者picocom

```shell
# 1 mac osx 下安装软件需要使用home brew 软件安装管理器，如果你的机器没有安装请按照如下指令安装，如果已经安装，掠过此步骤。
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 2 安装minicom
brew install minicom
# 3 安装picocom
brew install picocom
```

# 串口监控程序的使用

## picocom

获取picocom的使用帮助

```shell
picocom --help
```

连接到串口，进入交互解释环境，

```shell
# 对于Linux系统
picocom -b 115200 /dev/ttyUSB0
# 对于Mac OS X系统
picocom -b 115200 /dev/cu.SLAB_USBtoUART
```

## minicom

获取minicom的使用帮助

```shell
minicom --help
```

连接到串口，进入交互解释环境，

```shell
# 对于Linux系统
minicom --device /dev/ttyUSB0
# 对于Mac OS X系统
minicom --device /dev/cu.SLAB_USBtoUART
```

请注意minicom的使用，该软件在串口调试时会经常用到



# 固件烧录

## 下载固件

首先，请下载Micropython固件，本实验使用esp32芯片，请前往[下载地址](http://micropython.org/download#esp32)。下载时，选择standard firmware (latest)。

## 安装烧录工具

Esp32 的烧录工具是esptool.py，可见它由python写成，因此需要你的操作系统预先安装python，或者python3。

```shell
# 对于python2
pip install esptool
# 对于python3
pip3 install esptool
```

## 烧录固件

1. 第一次使用时，需要先擦除固件，以后则可以直接烧录。

   ```shell
   # linux
   esptool.py --chip esp32 --port /dev/ttyUSB0 erase_flash
   # mac osx
   esptool.py --chip esp32 --port /dev/cu.SLAB_USBtoUART erase_flash
   ```

   

2. 烧录固件

   ```shell
   # linux, 请调整固件文件名为你下载的文件，本例中为：esp32-bluetooth.bin
   esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 460800 write_flash -z 0x1000 esp32-bluetooth.bin
   # mac osx 请调整固件文件名为你下载的文件，本例中为：esp32-bluetooth.bin
   esptool.py --chip esp32 --port cu.SLAB_USBtoUART --baud 460800 write_flash -z 0x1000 esp32-bluetooth.bin
   ```

   

3. 

