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

lets look around to see if there is anything useful. it seems like there is a guy named alex and loves music, lets explore in the admins tab. woah theres alot of information it seems like alex has been messing around with a proxy and the backup is called music_archive which can be useful, lets explore the archives tab and lets click download to see if we get anything. we do! we get a tar file named archive.tar lets extract the file. To do that we go into the directory were it was downloaded and type this comamnd:

tar -xf archive.tar

once thats done lets see what we got, type ls to see whats in the directory and looks like we got a new directory called home

![](/images/cy7.png)

lets see whats inside of home directory and there is another directory and it keeps going, at final_archive directory there is a README text file as shown below

![](/images/cy8.png)

once we read the README file it says that it is a borg Backup repository. doing more reasurch on borg it is a backup repository that is safe and secure, we need to extract everything in this back up so we can see what it has inside but first we have to download the tool for borg in order to extract everything to do that type

sudo apt install borgbackup -y

once installed we can type this command to extract the whole backup 

borg extract home/field/dev/final_archive/::music_archive

the reason we put ::music_archive is because when we read at the target website alex was messing around with borg and he named the backup music_archive and the :: is used to specify which backup name we want to extract

once we type that command we have to type in a password to extract it but sadly we dont have the password

![](/images/cy9.png)

now we are stuck and hit a dead end, lets explore the other web directory we have found within our gobuster scan. the /etc so lets go back to our target website and type at the end of the url /etc 

http://10.10.46.212/etc/

![](/images/cy10.png)

we see this page and lets click on squid directory, we are seeing one text file and one configuration file, lets take a look at the passwd file. within the passwd file we get this long weird text to me it seems like some type of hash i havent seen before

music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.

lets explore the squid.config file and see if there is anything useful, within that file it doesnt seem to be useful at the moment. Lets try to find out what kind of hash it is that we have found in passwd file, i am going to use a website called https://hashes.com/en/tools/hash_identifier all you have to do is type the hash in and let it find what type of hash it is, make sure you remove the music_archive: from the hash so it should be $apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn. because the music_archive is the username and after : is the hash format

![](/images/cy11.png)

it says it found a possible match which is Apache $apr1$ MD5, md5apr1, MD5 (APR)

lets use johntheripper to try to crack the hash

first we have to put the hash in a text file im going to name mine hashh.txt

nano hashh.txt file

once thats done we can use johntheripper to crack the hash

john hashh.txt wordlist=/usr/share/wordlist/rockyou.txt

![](/images/cy12.png)

it cracked the hash! the password is squidward now we can try to use squidward to extract the backup

![](/images/c9.png)

once we type the password in we didnt get any error or anything lets go inside the home directory

![](/images/cy13.png)

we see a new directory called alex lets go explore in that directory

in the desktop directory we found a secrete.txt file but nothing useful, in the documents directory we find a note.txt file and it contains a username and password

![](/images/cy14.png)

i think that can be used to ssh into the machine so lets give it a try

ssh alex@10.10.46.212 

type in the password we just found and we are in!

![](/images/cy15.png)

lets see whats in our directory

ls

we see user.txt file and cat it out and you have answered the question

The fifth question: What is the root.txt flag?

lets explore around to see if there is any way to get root

i couldnt find anything so lets see what we can use sudo without a password to check for that we have to type

sudo -l

![](/images/cy16.png)

it shows that there is a bash script we can execute on /etc/mp3backups/backup.sh lets go to the /etc/mp3backups directory to see more

once we are there type ls -al

![](/images/cy17.png)

we can see we can run backup.sh script with root so we can write to that file if we add /bin/bash to it and once it executes we will become root

chmod +w backup.sh

that will give us writing priveleges to backup.sh and lets write inside backup.sh /bin/bash

nano backup.sh

![](/images/cy18.png)

as you can see /bin/bash this will execute bin bash which will give us root because we are running this bash script as root

we are finally root, go into root directory and you will find your root flag and you have completed the ctfs congractsss!!! thank you for reading

