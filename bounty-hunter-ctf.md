Hello we are doing the bounty hunter ctf on tryhackme easy level

The first question was: Find open ports on this machine

we will use nmap to scan on our target to see what ports are opened

nmap -sV -A 10.10.179.29

Ports:

21 ftp

22 ssh

80 http

as shown in the image below those are the open ports

![](/images/bh1.png)


We have completed the first question now the second question is: Who wrote the task list?

It seems like we have to find a text file somewhere. We know FTP is File Transfer Protocol and its used to store files or transfer files.
Based on our port scan it shows that ftp can be access without correct credentials it allows anonymous access so we can access it.
In order to access ftp i used this command:

(make sure to replace the ip address to your target machine address)

ftp 10.10.59.1

once you press enter it will ask for username and for username always type anonymous and u can use any password. you will know you are in if it says login successful and it looks like this
on the image below

![](/images/bh2.png)

Once we are in lets explore it works the same as in the terminal if you need help for the commands type help and it will list the commands you can use
lets see whats in our current directory


Type ls 

![](/images/bh3.png)

The photo above we can see two text files called locks.txt and task.txt, now we have to transfer those files to our computer in order to do that we use the mget command

mget (file name) 

![](/images/bh4.png)

to exit out of ftp we can type exit. lets view our text files that we have got from ftp

im going to see task.txt first.

![](/images/bh5.png)

we see that in the task.txt file there is a guy names lin

lets see what locks.txt has to show

![](/images/bh6.png)

this shows us a bunch of random stuff this could be a list of passwords

third question: What service can you bruteforce with the text file found?

we can brute force ssh because there is a guy named lin so we can use lin as a username and bruteforce ssh with a password wordlist such as the file we found locks.txt


Fourth question: What is the users password?

since there is a guy named lin i can use that as a user to brute force my way in into ssh with the password list called locks.txt that we got from ftp.
In order for us to brute force i used a tool called hydra to burte force ssh. The commands to brute force with hydra is below

![](/images/bh7.png)

hydra -l lin -P lock.txt ssh://10.10.179.29

it found the password! now we can ssh into it

In order to ssh into it we used the command shown below 

ssh lin@10.10.179.29

Fifth question: user.txt

we have to find the user.txt file so lets explore around lets list out what we have in our current directory

ls

we have found the user.txt in our current directory! cat it out and paste what u found inside user.txt to solve the fifth question

Sixth question: root.txt

now we need to find the root.txt file, in order to get the root.txt file we have to priv escelate to root because we dont have root priveleges. 

i tried su command with the password RedDr4gonSynd1cat3

but it was wrong so i tried to use this other command to see if we have any root privelege to run any commands

su -l 

and then the password RedDr4gonSynd1cat3

and i got 

[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar

![](/images/bh8.png)

i can see i can run root on tar and i can see if i can exploit tar within using sudo command and to do that we use this website called gtfobins.com

so i went to a website to see if there is a privlage escalation on sudo with tar so 

i typed in tar in the search bar and found out there is a way to get root so i clicked on sudo and copied the command and pasted it in the terminal

once we press enter we should be root

![](/images/bh9.png)

we can type whoami to see if we are root and yes we are now we have to go to root directory and you will find the root.txt

you have completed the ctf i hope this has helped you out in any way! thank you
