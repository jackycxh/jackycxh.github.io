---
title: Caller ID receive and call disconnect problem for FXO ports
categories:
  - Cisco
  - IPT
  - VoIP
date: 2011-03-31 07:09:26
tags:
  - Cisco
  - IPT
  - VoIP
---

## 关于FXO上的挂断

大家应该都已经了解FXO上的挂断需要检测外线上的挂断音，在思科的FXO上，一般我们通过配置下面的命令：

```
voice-port x/x/x
  supervisory disconnect dualtone mid-call
  cptone CN
  timeouts call-disconnect 1
```

这样多数情况下，可以解决在外线挂机后，系统可以侦测到挂断的音频，并释放端口，这里的`cptone CN`已经为路由器制定了检测的标准。

但在某些情况下，不能用 `cptone CN`这条命令时，就需要手动制定disconnect的侦测标准并应用到端口，如下：



```
voice class custom-cptone CHN
  dualtone disconnect
  frequency 440
  cadence 350 350
！
voice-port 0/0/0
  supervisory disconnect dualtone mid-call
  supervisory custom-cptone CHN
  timeouts call-disconnect 1
```

中国的忙音/挂断音的标准是440HZ，通350ms，断350ms。如果用外置的挂断器，也是检测这个挂断音后，强制释放这个语音端口。在今后的项目中，如果需要额外用挂断器时，可以先试用`CPTONE CN`，不行的话，试试看自定义挂断检测，应该可以处理掉大部分的故障。

[思科的参考](http://www.cisco.com/en/US/docs/ios/12_1/12_1xm/feature/guide/dt_dscon.html)

## 来电显示

来电显示开通后，由于局端的设备不同，来电显示的制式有可能为DTMF或者FSK。FSK的来电显示，通常是在第1、2声振铃之间，局端同时会伴随有对时信息，如果终端设备有此功能，可以自动调整时间，但FSK也会有3种不同的制式应用在不同的国家。DTMF的来电显示多数会在振铃前。

多数电话机设备不太会有制式不兼容的问题，一般都可以自动检测到不同的制式并显示来电号码。（强烈要求思科自适应来电！！！）

思科的路由器的默认的CPTONE是美国，在修改CPTONE时，来电显示的制式也会一并调整，我没有从命令中找到单独修改来电显示制式的方式。

所以，在确认电信的来电显示已开通（比如用话机可以显示号码）但路由器通过`debug vpm all`看到`caller id receive failed`时，就很有可能是制式的不兼容造成的，这时可以试试看改变`cptone`来进行调整并测试。不能太指望电信的接口人员会告诉我们详细的技术规范，至少我从来没有碰到过。

这种方式解决来电显示时，可能带来的问题是FXO在外线上的挂断，这时参考方法1里面的内容来解决吧。
