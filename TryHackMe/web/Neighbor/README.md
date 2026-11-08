# THM Neighbor Writeup

Webex, IDOR

![image](./images/HkcE5i8cfx.png)

went to the page and viewed the source code
![image](./images/rJGGijI5zl.png)
found this interesting command which indicates that i can try doing hydra on it since we know the username 

went to network to see what the variable is
![image](./images/B1XOsoL9Ge.png)
it's just regural username and password

used this command
```bash!
hydra -l admin -P rockyou.txt 10.48.183.1 http-post-form "/login.php:username=^USER^&password=^PASS^:F=Invalid"
```
after running it for awhile, i realised i was wrong since we're working on an IDOR

i used burpsuite to interupt the guest login process 
![image](./images/SkjwLh89Mg.png)

i changed user=guest to user=admin then forwarded it
and i got the flag in response
