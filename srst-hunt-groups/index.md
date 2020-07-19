# SRST Hunt groups


When fallback to SRST, the Hunting feature from CUCM will not available. Add below commands to SRST configuration to had the hunting working when SRST.

```
call-manager-fallback
 no huntstop
 alias 1 20000 to 20001 cfw 20011
 alias 2 20011 to 20002 cfw 20012
 alias 3 20012 to 20003 cfw 20000
```

or

```
call-manager-fallback
 no huntstop
 alias 1 20000 to 20001
 alias 2 20000 to 20002
 alias 3 20000 to 20003
```

