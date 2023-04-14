Nmap scan:



![image](https://user-images.githubusercontent.com/73919277/232127513-0b8338ca-add3-44c3-b3d3-f404f31e9bf9.png)


Now I see smb is there and mysql server is there, lets try enumerating smb first


![image](https://user-images.githubusercontent.com/73919277/232127630-328d57e9-9de6-487c-a384-090e9cce594e.png)



I see backups is unusual to be there so lets see if we can access backups with anonymous login


![image](https://user-images.githubusercontent.com/73919277/232127703-8a5790d3-5d47-4747-a338-1b0026edfb3b.png)


I see there is a file in there so I went ahead and downloaded the file with get command

Lets view the file and we can see we have the username and password to connect to microsoft sql 


![image](https://user-images.githubusercontent.com/73919277/232127749-4b3ca1bf-1c7d-4067-9616-7489c7308237.png)


We can connect to microsoft sql server with a script called mssqlclient.py
We can download all the scripts we need In github called impacket, once installed navagate to impacket directory and examples directory and then type the command to log into the sql server


![image](https://user-images.githubusercontent.com/73919277/232127805-3357815e-ad1a-40a7-9292-23096bba6f42.png)


Once we are in we type In help to see what we can do


![image](https://user-images.githubusercontent.com/73919277/232127956-10f5b85a-d2fb-4926-b4c8-c73189a6b8f7.png)


I see that we can use xp_cmdshell to use a cmd command but when we run that command it says it is disabled by default


![image](https://user-images.githubusercontent.com/73919277/232128275-50555b8e-c564-4dcf-ad38-765de9cd559e.png)


Time to google how to enable xp_cmdshell and I found this article that shows you how to enable it and its straight forward and easy. https://www.mssqltips.com/sqlservertip/1020/enabling-xpcmdshell-in-sql-server/

These is the commands you want to run in order to enable xp_cmdshell


![image](https://user-images.githubusercontent.com/73919277/232128314-dd620fe1-99b5-4718-8ab3-ce3a81503d71.png)


Now we can use xp_cmdshell 


![image](https://user-images.githubusercontent.com/73919277/232128370-484274e7-dc38-49f2-a150-ec6424fbe7d5.png)


We see it works so now lets get a reverse shell on this, so we need to download this exe file to gain a reverse shell
Lets first download the reverse shell https://github.com/int0x33/nc.exe/blob/master/nc64.exe

After its downloaded go to the directory it downloaded and lets start a http server 

Python3 -m http.server 80 

Once that is up and running open another terminal and set up netcat


![image](https://user-images.githubusercontent.com/73919277/232128469-4add221e-69e9-412b-8cfb-f12eb4c50444.png)


Once that is done, go back to the sql server and lets download the reverse shell, 


![image](https://user-images.githubusercontent.com/73919277/232128497-1e9a5fbe-a6e4-4c37-b6e9-3d4df0bb5790.png)



We are saying use powershell and go to the downloads folder of sql_svc and then we use wget tool to download our reverse shell and in order for it to save we have to output the file so we use -outfile command and name the file whatever you want in this case I am going to name it the same.


![image](https://user-images.githubusercontent.com/73919277/232128559-1f2fa26e-bfeb-4280-a817-51c8dcace9ca.png)


This might be a little bit confusing, let me explain:
We are using powershell to do our commands and we are saying go to the downloads directory of sql_svc and then we execute nc64.exe and we bind it with cmd.exe and connect back to our ip and the port we chose. After that we have a shell so check your netcat and you should be in.

Now lets enumerate more with winpeas, it’s a great tool to enumerate windows machines and a time saver too! First you have to download winpeas https://github.com/carlospolop/PEASS-ng/releases/tag/20230413-7f846812

After that go to the directory you have downloaded it and start a http server

Now lets download winpeas to the windows machine 


![image](https://user-images.githubusercontent.com/73919277/232128597-5b0c7c9c-bb34-4d86-ac7f-a624732a4588.png)


As we can see winpeas is there so lets start winpeas

./winpeas.exe


![image](https://user-images.githubusercontent.com/73919277/232128621-3b87b0f4-82d6-477b-a3e9-90ed19f9fcd6.png)


Lets wait until It finishes and once it finished I know it can be a lot if it is your first tine using this tool but we are looking for red words which usually means its helpful information.

We can see it found a history file so maybe we can find someone logging in


![image](https://user-images.githubusercontent.com/73919277/232128697-893a8576-c306-4d94-8db9-7d8343cfda10.png)


Lets check and see

![image](https://user-images.githubusercontent.com/73919277/232128789-5ddd9409-7203-402f-8a0f-a4d346ed46e3.png)


We see someone logging in as administrator and their password that’s amazing! Now we can log in as administrator with a tool called psexec.exe and its in the impacket directory of what we downloaded earlier 

Now in the new directory lets go to impacket and then examples and type this command to log in


![image](https://user-images.githubusercontent.com/73919277/232128970-a0d3e302-76a9-4a10-90d6-bbb87bc0a268.png)


After you type that, It will ask you for a password. Once you type in the password we are Administrator!
The root file is in /Users/Administrator/Desktop
The user file is in /Users/sql_svc/Desktop

Happy Hacking!!
