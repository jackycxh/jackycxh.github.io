---
title: mGRE - multipoint GRE tunnel
date: 2014-06-12 13:55:56
tags:
 - Cisco
 - Network
categories:
 - Cisco
 - Network
featuredImage: "/images/mgre-multipoint-gre-tunnel.jpg"
---

## Features

1. tunnels between each two sites, traffics between spoke sites don't need to pass the hub site.
    * permanent tunnel between Hub site and each Spoke site
    * dynamic tunnel between Spoke sites on demand
2. simplify the configuration
3. In Spoke site, static IP address is not required. static IP address is required only in Hub site.

## How is it working

1. Hub site works as a NHRP server and ready to accept the register from the spoke sites
2. Spoke sites register their IP to the Hub site(NHRP server).
3. permanent tunnels created between Hub and each Spoke.
4. the data traffics between Hub and each spoke go thru the permanent tunnels.
5. if there is any data required traffics from spokeX to spokeY, spokeX query from hub for the spokeY's IP address.
6. a dynamic tunnel created between spokeX and spokeY.
7. the data traffics between spokeX and spokeY go thru the dynamic tunnel but no need to pass thru the Hub.
8. while there is no data traffic between spokeX and spokeY, a timer counted down, the tunnel destroyed when expired.

## How to configure

### Hub:

```
interface tunnel 1000
 ip address xxx.xxx.xxx.xxx xxx.xxx.xxx.xxx
 ip nhrp authentication ?????
 ip nhrp map multicast dynamic
 ip nhtp network-id 1000
 tunnel source ???
 tunnel mode gre multipoint
```

### Spoke:

```
interface tunnel 1000
 ip address xxx.xxx.xxx.xxx xxx.xxx.xxx.xxx
 ip nhrp authentication ?????
 ip nhrp map (hub tunnel ip address) (hub physical interface ip address)
 ip nhrp map multicast (hub physical interface ip address)
 ip nhtp network-id 1000
 ip nhs (hub physical interface ip address)
 tunnel source ???
 tunnel mode gre multipoint
```
