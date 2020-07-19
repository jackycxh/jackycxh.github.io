# Translation-profile in Cisco Voice Gateway SRST mode.


In some cases, you may want to populate the calling/called number when remote site fallback to SRST mode. You can add `translation-profile` in `call-manager-fallback` as below:
```
call-manager-fallback
 translation-profile incoming aaa
 !!! or
 translation-profile outgoing bbb
```

The `translation-profile` under the `call-manager-fallback` affect the incoming and outgoing calls to the router from the IP phone. This is a different behavior than when you apply the `translation-profile` under a `dial-peer`. The **incoming** and **outgoing** commands are related to the IP phone. The **incoming** command changes the parameters of calls that come from the IP phone. The **outgoing** command changes the values of calls that go out of the router to the IP phone.

