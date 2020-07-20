---
title: Cisco IOS configuration file auto backup and rollback
date: 2013-05-03 10:17:24
tags:
  - Cisco
categories:
  - Cisco
featuredImage: "/images/cisco-ios-configuration-file-auto-backup-and-rollback.png"
---

Since IOS ver 12.3(4), Cisco introduced the Archive mode into the IOS which could be used for backup the configuration file automatically and roll back the configuration to an earlier version. Below are some IOS commands for this.
```
Router> enable                                             !Enter privilige mode
Router# configure terminal                       !Enter configuration mode
Router(config)# archive                            !Enter archive mode
Router(config-archive)# write-memory    !Enable automatic backup during write memory
Router(config-archive)# path URL             !Configure the path for backup
```
for example:
```
path flash:/xxx/$h-confg
path tftp:/$h-confg
```
`$h` will be replaced by the hostname. The final backup name will be `hostname-confg-1`, `hostname-confg-2`, etc.

```
Router(config-archive)# time-period xxx     !Configure the period of time in minutes to automatic archive the running-config
Router(config-archive)# maximum xxx       !Maximum number of the backup copies
Router(config-archive)# end               !Exit the configure mode
```
Manually archive with following command will generate the file name as `hostname-config-0`,
```
Router# archive config
```
Rollback to the earlier version of the config
```
Router# configure replace PATH        !Replace the current running-config with the configure file at PATH
```
