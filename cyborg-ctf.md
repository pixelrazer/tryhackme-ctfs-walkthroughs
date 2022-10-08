this is the cyborg ctf walkthrough in tryhackme

The first question: Scan the machine, how many ports are open?

we will use nmap to scan for open ports

![](/images/cy1.png)

nmap -A -sC 10.10.239.240

open ports:

22 ssh

80 http

The second question: What service is running on port 22?

if you know your networking you would know port 22 is ssh but if you dont know it its ok within our nmap scan it shows port 22 and under service it would say ssh 

The third question: What service is running on port 80?

again if you know your networking you would know port 80 is http and port 443 is https but its ok if you dont know it, you can see from our nmap scan that port under service on port 80 it says http


The fourth question: What is the user.txt flag?

Now we need to get the user.txt flag but theres only 2 ports open. we cant do anything to ssh beshides brute force but that will take too much time and port 80 is open that means its hosting a website so enter your target ip address to google and see the website

![](/images/cy2.png)

it seems like it is a default webpage of apache. lets take a look of the page source to see if there is any notes in there to give us a clue. you can do inspect element and look for clues that way its the same thing

![](/images/cy3.png)

there is nothing useful in there so we have to do some directory enumeration on the website. i use a tool called gobuster but there is another tool called dirbuster it will do the same thing the only difference is that gobuster doesnt have a gui and dirbuster does have a gui. i will be showing how to use gobuster.

you have to download gobuster to do that in your terminal type sudo apt get install gobuster and press y then let it install. once installed lets get started. i will show you the command to use below along with a screenshot as always. 

![](/images/cy4.png)

gobuster dir -u http://10.10.239.240/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 

gobuster dir is the type of scan we will do which is directory scan

-u: the website we want to scan to

-w: the wordlist we are going to use to scan the wesbite (in ctfs always use that wordlist to scan for directories in websites)

once your are all set press enter and let it run for 2 minutes or so. i let it run for 2 min or so and this is the results i got

![](/images/cy5.png)

it showed me two results: /admin and /etc

go back to the website of the target and at the end of the ip put the directory we found, i will put admin because it seems more important than etc http://10.10.239.240/admin/

we get this webpage

![](/images/cy6.png)

lets look around to see if there is anything useful. it seems like there is a guy named alex and loves music, lets explore in the admins tab. woah theres alot of information it seems like alex has been messing around with a proxy and the backup is called music_archive which can be useful, lets explore the archives tab and lets click download to see if we get anything. we do! we get a tar file named archive.tar lets extract the file to do that we go into the directory were it downloaded and 


The fifth question: What is the root.txt flag?
