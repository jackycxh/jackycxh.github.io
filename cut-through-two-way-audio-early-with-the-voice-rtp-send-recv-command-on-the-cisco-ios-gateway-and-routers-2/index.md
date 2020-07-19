# Cut Through Two-Way Audio Early with the voice rtp send-recv Command on the Cisco IOS Gateway and Routers



The voice path is established in the backward direction at the start of the RTP stream. The forward audio path is not cut through until the Cisco IOS gateway receives a Connect message from the remote end.

In some cases, it is necessary to establish a two-way audio path as soon as the RTP channel is opened, which is before the Connect message is received. In order to achieve this, issue the `voice rtp send-recv` global configuration command.

