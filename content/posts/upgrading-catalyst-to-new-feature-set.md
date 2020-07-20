---
title: Upgrading Cisco Catalyst switch to new feature set
categories:
  - Cisco
  - Network
date: 2010-12-13 03:19:00
tags:
  - Cisco
  - Network
---

> Error: The image in the archive which would be used to upgrade
> Error: system number 1 does not support the same feature set.

I just got this error message while upgrading a Cisco Catalyst 3560 from 12.2(35)SE5 to 12.2(44)SE. The older version was IP BASE and the new version is ADVANCED IP SERVICES.

It seems that Cisco has added a check into there upgrade process to prevent you from accidentally changing the feature set during an IOS upgrade. This is a really nice sanity check since changing feature sets on a production switch could really ruin your day.

To bypass this check, add the `/allow-feature-upgrade` parameter to the `archive download-sw` command. According to the documentation, this feature is new as of 12.2(35).
