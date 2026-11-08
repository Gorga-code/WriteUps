# THM Checkmate Writeup

opened http://IP:5000 to firefox

level 1 told me to go to firewall.thm:5001
![image](./images/HJpSGNFtze.png)

put IP and firewall.thm to /etc/hosts so i can open firewalll.thm:5001 in browser
![image](./images/ryp9GEFtMe.png)

clicked inspect + network to see the fields then put admin as user and 123 as password

used hydra command : 
hydra -l admin -P rockyou.txt firewall.thm -s 5001 http-post-form "/login:username=^USER^&password=^PASS^:Invalid credentials"

got the pass (12345)

level 2 told me this :
![image](./images/BylJQ4ttMx.png)

as usual, put web name and ip in /etc/hosts then went to http://job.thm:5002 and got this home page
![image](./images/H1Zu7VYYzl.png)

the login button caught my attention and clicking it brought me to a familiar login page
![image](./images/BkkhX4Ktze.png)

repeat the same thing as in level1
![image](./images/rydXSVKFzg.png)

couldnt find the pass using this, looking at the hint which is 'Focus on commonly used words.' i decided to use wget to extract meaningful keywords from the application
![image](./images/SJoYdA15Me.png)

ran hydra with our new keyword and got the pass
![image](./images/SJTnuR1cGx.png)

level 3:
![image](./images/S13eYCJ5Mg.png)

web:
![image](./images/BkJStC15fx.png)

reading the hint, i just used hydra with the exact same command as level2, but changing marco to Marco

however that yielded no result as i realised i have to input new information about marco acquired after unlocking level2
using cupp, i created a new wordlist called marco.txt, then ran hydra again
![image](./images/B1icjA19zg.png)
got the pass

level4:
![image](./images/SyCECC19zx.png)
i just opened marco's profile pic in another tab and got the hash as the file name.
after cracking the hash with an online tool, i got the pass *family*

level5:
![image](./images/BJPtR0Jqfg.png)
we will be using the following info to create a pass list using crunch
![image](./images/Sy9RRCy5zg.png)

crunch command:
![image](./images/S1hYJyx5Gx.png)

explanation:
crunch [min] [max] [charset]: Generates passwords with a fixed length using the specified character set.

-t [pattern]: Defines a custom structure for password generation:

[pattern]→Security20%%!

Security → fixed keyword
20 → ensures a year-like prefix
%% → two variable digits (00–99)
! → common special character at the end

then i use hydra to get the ssh password
![image](./images/BJlNgke5fx.png)
![image](./images/rkvdgke5fe.png)
