---
title: Batch change extension number of IP phones.
tags:
  - Cisco
  - IPT
  - VoIP
categories:
  - Cisco
  - IPT
  - VoIP
date: 2009-07-14 15:20:34
---

In some case you may want to change the extension number for all IP phones, or a lot of phones. For example, now the IP phones have 3 digits extension number and you want to extend it to 4 digits.

1. create a phone file format including all uncommon fields(each phone has its own value in these fields).
2. export the phones with the phone file format just created. separate by phone model.
3. modify the exported files.
4. create a phone template for each phone model, including all common fields(all phones have same value in these fields)
5. delete all phones you want to change.
6. insert the phones with the just created phone template from the modified files of each phone each model.
