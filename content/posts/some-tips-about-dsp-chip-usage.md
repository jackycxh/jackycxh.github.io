---
title: Some Tips about DSP Chip Usage
date: 2009-02-13 02:12:00
tags:
  - Cisco
  - VoIP
  - IPT
categories:
  - Cisco
  - VoIP
  - IPT

---

One DSP Chip formed in hardware as PVDM2-16.

Each DSP can provide only one function.

Each DSP chip can support 8 PSTN line(analog), Or four DSP chips suppor a E1 PRI PSTN circuit.

Conference based on each DSP allows 8 conferences with a maximum of 8 g.711 participants. When a conf begin, all 8 position are reserved at that time. When Conf configured to accept both g.711 and g.729, one DSP provices 2 conference.

NM-HDV2 limited to 400 streams.

Each DSP can provide max of 8 sessions of transcoder.

Each DSP can provide 16 g.711 mulaw or a-law MTP sessions or 6 G.729 or GSM MTP sessions.
