---
title: Cisco Voice Gateway SRST Re-Convergence Time
tags:
  - Cisco
  - IPT
  - VoIP
categories:
  - Cisco
  - IPT
  - VoIP
date: 2009-06-22 14:35:17
---

When the phone register to SRST, after the connection to CUCM re-established, it takes 120 seconds by default to re-register to CUCM. This timer is configured in CUCM, by global `enterprise parameters` configuration, or by `Device Pool` configuration, as `Connection Monitor Duration`
