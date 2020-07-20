---
title: How to block the incoming phone call from a particular calling number in H323 voice gateway

date: 2011-09-10 01:47:00
tags:
  - Cisco
  - IPT
  - VoIP
categories:
  - Cisco
  - IPT
  - VoIP
featuredImage: "/images/how-to-block-the-incoming-phone-call-from-a-particular-calling-number-in-h323-voice-gateway.jpg"
---

1. Create a translation rule that reject the calling number.

```
voice translation-rule 99
 rule 1 reject /1234567890/
```

2. Create a translation-profile match the calling number to the created translation-rule.

```
voice translation-profile block-incoming-call-from
 translate calling 99
```

3. Add call block configuration to dial peer.

```
dial-peer voice 10
 call block translation-profile incoming block-incoming-call-from
```
