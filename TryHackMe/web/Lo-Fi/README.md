# THM Lo-Fi Writeup

Webx, LFI
![image](./images/rkt15Sb5zg.png)
Since it already mentioned LFI, i tried doing 

![image](./images/Hka13HW5Ge.png)
just to confirm it was LFI, and after searching for root for confirmation, i got it
![image](./images/ry5b3BW5Gg.png)

next i used curl while varying the page names to find something interesting
![image](./images/H1jbCB-qMe.png)
![image](./images/SJywCHZqMl.png)
![image](./images/ByC20SWqGl.png)

thought simpler and went to 
![image](./images/B159b8-cMx.png)

and i got the flag
![image](./images/BJRs-UZ9Gl.png)

Lesson : 
- inspect source code to find that /?page is an important perimeter
- confirm LFI using 'http://IP/?page=../../../../etc/passwd'
- next time, brute force using gobuster or fnff
