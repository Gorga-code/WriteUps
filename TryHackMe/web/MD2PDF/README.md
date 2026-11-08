# THM MD2PDF Writeup

CTF : 
![image](./images/rkJTfylqzx.png)

it is unclear if MD2PDF is a tool or a website, but after changing /etc/hosts and going to http://MD2PDF.com, i found this
![image](./images/HyMlQ1x5fl.png)

used dirbuster to find other directories
![image](./images/S1HZPyx5zl.png)

result : 
![image](./images/B1vAwkxcGg.png)

going to /admin shows me this : 
![image](./images/ryBOw1x5fg.png)

this shows me what to do. I create a payload that allows me to make a server-side request to the internal service with a Markdown image
```html 
<iframe src="http://localhost:5000/admin"></iframe>
```
got the flag : 
![image](./images/S1XbFylqzx.png)

Lesson : 
- use nmap to confirm it was a web instead of randomly adding .com (i got lucky)
