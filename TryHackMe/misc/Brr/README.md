# THM Brr Writeup

![image](./images/BkNMPnUcfx.png)

as usual, do an nmap scan
![image](./images/Hk4WdRxjfl.png)

opening port 80 gave us an erro response
port 5020 loaded too long
port 5901 gave us this ![image](./images/B1Th_Alszx.png)
and port 8080 redirected us to ScadaBR/login.htm
![image](./images/SkM1t0ljMx.png)

tried hydra with username admin
```bash
hydra -l admin -P rockyou.txt 10.49.166.229 -s 8080 http-post-form "/ScadaBR/login.htm:username=^USER^&password=^PASS^:<failure-string>"
```
got this : 
![image](./images/Sy8Y0AxiMx.png)

using admin:admin, i got into the web
![image](./images/ryEhR0gjGg.png)

took me awhile to find this path : 
went to data sources and edit
![image](./images/B1jvmkWjGl.png)

changed the register range to holding register and the number of register to only 30 and got some interesting hex
![image](./images/H1Oi7JbsMl.png)

decoding them using gpt gives me the flag : 
THM{modbus_hid}

Lessons : 
- search what scadabr is and click on anything to find information
