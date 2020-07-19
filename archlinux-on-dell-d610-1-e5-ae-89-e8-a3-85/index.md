# ArchLinux On Dell D610 -1. 安装


一直用ubuntu在我的老D610上，一直想把他给换了，在这个老机器上ubuntu还是太慢了。老早看中了ArchLinux， 更新快，而且完全自己定制。刚刚发现ArchLinux出2009.02, 安装时可以选ext4了。马上下了一个USB的文件。开工......

参照[ArchWiki](http://wiki.archlinux.org/index.php/Install_from_USB_stick%20)里面的把镜像解到一个U盘里。

```bash
sudo dd if=archlinux-2009.02-core-i686.img of=/dev/sdb(这是我的U盘设备名)
```
重启，开机时选择从USB启动。顺利进入Shell. (安装盘用户名`root`，无密码)
```bash
#/arch/setup
```
启动安装程序. 我原来`home`是单独的分区, 不需要备份了, 只要安装时不要覆盖`home`分区就行了.(Linux这点非常好,用户所有文件都在`home`里). 

一个菜单, 从上到下一个个做下去就可以了.相对来说Arch的安装过程比Ubuntu要难一点, 但只要有一点linux基础,应该都没什么问题.
在分区那一步时,把我原来的`root`分区重新format, 可以选`ext4`了. :smile:.  `home`分区保留原来的, 我原来的文件都还在里面呢, 只能装完后在把它从`ext3`转换到`ext4`了.
选package时, 我把`base-dev`一起选上了, 另外还有`b43-fwcutter`, `netcfg`, 和wireless的一些东西, broadcom的无线网卡, `b43-fwcutter`是必需的. 
改配置文件时, 大部分都是保留默认的. 只是在`rc.conf`里设了一个`hostname`. 还是`locales`里选上了那些个关于中文的(zh_...)
很快就装好了. 重启, 非常快, 但现在只有console模式, 什么X之类的都还没有. 这就是我想要的, 从头开始, 全部自选. 

未完待续


