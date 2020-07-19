# Dial-peer Timeout


When the dial-peer with lower preference could not succeed, how long time would be wait until fallback to the dial-peer with higher preference. default value is 15 seconds. useful when h323 gateway working with multiple Call Managers.

```
voice class h323 1
 h225 timeout tcp establish 3
 ! set it to 3 seconds.

dial-peer voice 1 voip
 preference 1
 destination-pattern (pattern)
 session target (target)
 voice-class h323 1

dial-peer voice 2 voip
 preference 2
 destination-pattern (pattern)
 session target (target)
 voice-class h323 1
```

