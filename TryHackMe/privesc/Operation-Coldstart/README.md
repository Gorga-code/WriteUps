# UNFINISHED THM Operation Coldstart Writeup

WebEx, reverse shell + privilege escalation
![image](./images/rk-Qx2QqMx.png)

scan ip with nmap
![image](./images/B1M5Z3XqMe.png)
port that was open : 21(ftp), 22(ssh), and 80(http)

i tried anon login in ftp and i succeded
![image](./images/ByKNzn7qMl.png)

now that were in the ftp session, easiest way to find a user flag is : 
![image](./images/rJHlNhX5zx.png)
download all the directories and files in ftp

search using find
```bash!
find TARGET_IP -type f \( -name "user.txt" -o -name "flag.txt" \) 2>/dev/null
```
but doing this didnt gave me anything

so i went back to the ftp session and did
```bash!
ls -la
```
![image](./images/HyvspamcGg.png)

there is a directory called pub. lets move there and see what s inside
![image](./images/S1UkRp79fx.png)
found an interesting file

use 
```bash!
get backup.tar.gz
```
to get then extract the file
![image](./images/HJXdJ0X9Gx.png)

read the inside of the zip
![image](./images/S1V3JC7qGe.png)

sending the output of each file to ai gives us a direct attack path

typed in http://kestrel.thm/admin/notes to the web given shows us
![image](./images/B1Bx7R79fg.png)

let's use this ssh
![image](./images/H1HwXR79fx.png)

and use 
```bash!
find / -type f \( -name "user.txt" -o -name "flag.txt" \) 2>/dev/null
```
got the dir, then the flag
![image](./images/BJKjQAm5Ml.png)
![image](./images/HkshmC7cfe.png)

for the last flag, since it didnt show up in our find, i assume we need to do a privilege escalation

checked privilege with sudo -l and found that i need a password.

next thing i did was checking scheduled task
![image](./images/By9WBCQ5zx.png)

giving the output to ai, tells me to continue searching
![image](./images/B18PLAQ9Mg.png)

the first dir i checked gave me something interesting which is voltlabs-backup
![image](./images/r1CK8R79fl.png)

i read it then gave the output to ai

it told me to check if i can write in /opt/backups
![image](./images/rJybvCm9Mx.png)
the output confirms that our webdev user owns and can write to /opt/backups, while the cron job executes tar there as root every minute.

A classic tar option-injection technique will create specially named files such as --checkpoint and --checkpoint-action=exec=.... When root's cron runs tar ... *


wait for one min and run shell.sh
