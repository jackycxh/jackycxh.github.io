# ArchLinux On Dell D610 - 2. 网络设置


## 驱动无线网卡

我的D610用的是broadcom4311的无线网卡。在linux下面要去下载一个firmware的文文件用`b43-fwcutter`把firmware解出来(参见[linuxwireless](http://linuxwireless.org/en/users/Drivers/b43)）或在[我的网盘](http://www.adrive.com/public/61aea0517f655a74b0ec94507f9031e2f411ec34b65c26fb3fdf7c89cc118554.html)下载

``` bash
$ tar jxvf broadcom-wl-4.150.10.5.tar.bz2
$ cd broadcom-wl-4.150.10.5/driver
$ b43-fwcutter -w /lib/firmware wl_apsta_mimo.o
```

重启电脑。 无线网卡就可以用了。

## wpa_supplicant

我家里的无线路由器是用WPA认证的。 需要用`wpa_supplicant`。安装系统里已经装上了。

新建一个文件（`wpa.conf`）

```bash
ctrl_interface=/var/run/wpa_supplicant
eapol_version=1
ap_scan=1
fast_reauth=1

network={
   ssid="myssid"
   scan_ssid=1
   psk="mypassphrase"
   priority=2
}
```

新建一个文件：（mywlan)

```bash
#!/bin/bash
ifconfig wlan0 up
wpa_supplicant -Bw -cwpa.conf -iwlan0
iwconfig wlan0 essid "myssid"
dhcpcd wlan0
```

每次启动时执行这个文件就可以连上无线网络了。

## 漫游

即然是本本， 就肯定会提到其它地方去的。而各个地方的网络环境都可能不尽一样。
进入`/etc/network.d`, 为每个地方建一个network profile。
我是只有两个，一个在家，用无线网络；一个在办公室， 有线网络， DHCP。

### Wireless-Home
```
CONNECTION＝"wireless"
DESCRIPTION="Wireless Network at Home"
INTERFACE=wlan0
SCAN="yes"
SECURITY="wpa"
ESSID="myssid"
KEY="mypassphrase"
IP="dhcp"
```
### Wired-Office
```
CONNECTION="ethernet"
DESCRIPTION="Wired Network at Office"
INTERFACE=eth0
IP="dhcp"
```
修改`/etc/rc.conf`

```
NETWORKS=(menu)
DAEMONS=(......net-profiles ......)      # networks可以删掉了。
```

重启电脑时就会有一个菜单可以选择用哪个网络了。


