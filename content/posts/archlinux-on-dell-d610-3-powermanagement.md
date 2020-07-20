---
title: ArchLinux On Dell D610 -3. Power Management
categories:
  - Linux
date: 2009-02-22 14:55:00
tags:
  - Linux
---

## CPU Frequency

CPU频率自动调整。节能，减噪。

```bash
pacman -S cpufrequtils    # 安装调频工具
modprobe acpi_cpufreq   # 加载内核模块。
modprobe cpufreq_ondemand    # 加载模版
modprobe cpufreq_powersave
```

执行 `cpufreq-info`取得CPU频率的上下限。

```bash
cpufrequtils 005: cpufreq-info (C) Dominik Brodowski 2004-2006
Report errors and bugs to cpufreq@vger.kernel.org, please.
analyzing CPU 0:
driver: acpi-cpufreq
CPUs which need to switch frequency at the same time: 0
   hardware limits: 800 MHz - 1.87 GHz</span>
available frequency steps: 1.87 GHz, 1.60 GHz, 1.33 GHz, 1.07 GHz, 800 MHz
available cpufreq governors: powersave, ondemand, performance
current policy: frequency should be within 800 MHz and 1.87 GHz.
              The governor "ondemand" may decide which speed to use
              within this range.
current CPU frequency is 800 MHz.```
```

编辑`/etc/conf.d/cpufreq`, 选择想要的模版以，设定上下限频率。

执行程序`/etc/rc.d/cpufreq start`

编辑`/etc/rc.conf`

```bash
MODULES=(...acpi_cpufreq cpufreq_ondemand cpufreq_powersave...)
DAEMONS=(cpufreq ......)
```

这样以后每次重启电脑后就会自动加载模块，程序

## Fan Control

根据温度自动调节风扇

常用的`lm_sensors`不支持D610的芯片。Google一下找到一个好东西 －[Dellfand](http://dellfand.dinglisch.net/)。

到这个网站一去下载[dellfand-0.9.tar.bz2](http://dellfand.dinglisch.net/dellfand-0.9.tar.bz2) （或到[我的网盘](http://www.adrive.com/public/0c017d983564397a53e3360550b017a315adf738472cce13c67c0b89254d40c6.html)下载）

```bash
＄tar jxvf dellfand-0.9.tar.bz2
$ cd dellfand-0.9
$ make
$ cp dellfand /usr/sbin
$ cp etc.default.dellfand /etc/conf.d/dellfand      # 可以修改这个文件设定风扇转速在多少温度时调节到什么速度（关，低，高）
```

软件自带的启动脚本`etc.init.d.dellfand`不适合用在Archlinux上， Archlinux用的是BSD方式的启动脚本。要自己做一个，或拿原来那个来修改。可以到[我的网盘上下载一个改好的](http://www.adrive.com/public/452cb8ebbd2b2830a96e028c9668c4965a69f646292873d941b1032c92563883.html)

把启动脚本放到`/etc/rc.d`里面.

修改`/etc/rc.conf`,

```bash
DAEMONS=(... dellfand ...)
```

这样dellfand守护进程就会在每次启动时自动加载。

## Hibernate & Standby

安装`pm-utils`
```bash
pacman -S pm-utils
```

修改`/boot/grub/menu.lst`

在`kernel` 后加上`resume=/dev/sda4` (交换分区，交换分区大小要大于实际内存）

挂起到内存：`pm-suspend`

挂起到硬盘：`pm-hibernate`

## CPU空闲时音箱发出bibi的噪音。

D610上跑LINUX的通病，解决方法：

修改`/boot/grub/menu.lst`

在`kernel`的选项后加上`idle=halt`.

## ACPI

安装`acpid`.

```bash
pacman -S acpid
```

修改`/etc/rc.conf`.

```bash
DAEMONS=(...hal...) # 启动时自动载入hal和acpid.
```

修改`/etc/acpi/handler.sh`([修改好了的](http://www.adrive.com/public/29178ed6fd594c0ba91fbe4e230c85cea2aa228fbe60e7cead4e7d37077f2fc9.html)), 改一些按键的名字和执行的程序。
按键名字可用`acpi\_listen`获取。 程序用`pm\_utils`里面的。

