The first question was: Find open ports on this machine

we will use nmap to scan on our target to see what ports are opened
nmap -sV -A 10.10.179.29

ports:
21 ftp  
22 ssh
80 http

as shown in the image below those are the open ports

![](/images/bh1.png)


We have completed the first question now the second question is: Who wrote the task list? 
ftp was open for anonymous access so i needed to get in ftp is file transfer proticol

i used 

ftp 10.10.179.29 

and used anonymous as the username and password

im in so i used 

ls 

and i see two text files so i used a a command to transfer them to my system using 

get (file name) 

in the locks file i got a bunch of usernames or passwords 

in the task file i got a weird writing saying
 1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin

there is a guy named lin 

since there is a guy named lin i can use that as a user to brute force my way in into ssh with the password called locks.txt that i got from ftp

so i used hydra 

hydra -l lin -P lock.txt ssh://10.10.179.29

found the password RedDr4gonSynd1cat3

so i ssh into it 

ssh lin@10.10.179.29

once i was in i found the user file already 

now i needed the root file but im not root 

i tried 

su and the password RedDr4gonSynd1cat3

but it was wrong so i tried to use this other command 

su -l 

and then the password RedDr4gonSynd1cat3

and i got 

[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar

i can see i can run root on tar and i used that command to see if i ca n run root on anything

so i went to a website to see if there is a privlage escalation and it was caled 

gtfobins.com 

i typed in tar and found out there is a way to get root so i clicked on sudo and copied the command and pasted it in the ssh terminal

and then i typed su again and i was root and i found the root.txt file 