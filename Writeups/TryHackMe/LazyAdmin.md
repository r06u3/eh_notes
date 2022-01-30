
https://tryhackme.com/room/lazyadmin



##################################
			Cosmin Tibuleac | 25.11.2021
##################################

export IP=10.10.237.239

# Task 1: Lazy Admin

## Nmap:
Host is up (0.078s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)



## Gobuster:
*gobuster dir -u http://$IP --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | tee gobuster.log*

![[Pasted image 20211211091904.png]]

So, gobuster found the following directories on the website:

/content 
/server-status


While accessing server-status requires privilegies, we can definitely access /content and we see the following:

![[Pasted image 20211211091844.png]]


If we click on the tips link, we get redirected to an article with 5 tips to do after installing SweetRice, which is a content management system (CMS).


The second tip is pretty interesting: 

2,**Protect your data**.

*SweetRice save all important file in the **inc** directory,there are two kinds of format ?:.txt (link.txt , htaccess.txt, lastest.txt) and .db (if track feature enabled).If you are using apache server,the file .htaccess which in inc directory will work for protect your data,if your server is nginx,you may see [Security setting for Nginx](https://www.sweetrice.xyz/docs/Security-setting-for-Nginx/).For other web server ,you may try it yourself.*

The tip tells us about a directory, called "inc".


This is interesting, as we may find some important files here.


 ![[Pasted image 20211211092542.png]]
 
 One sub-directory that stands out, seems to be a mysql backup:
 
 ![[Pasted image 20211211092744.png]]
 
 Inside, we can see a .sql file that we can download.
 
 The file actually is a PHP script: 
 
 ![[Pasted image 20211211092841.png]]
 
 Weird.
 
 We can open the file with any text editor (or reading it directly from the CLI)
 
 If we take a look at line 79, we can see some credentials: 
 
 ![[Pasted image 20211211093345.png]]
 
 
`admin:42f749ade7f9e195bf475f37a44cafcb`



Using the hash-identifier, it looks like the hash is an MD5: 


![[Pasted image 20211211094416.png]]


Now we can put the hash into a txt file and then give it to **JohnTheRipper**:


![[Pasted image 20211211094503.png]]

And in no time, John found it. It looks like the password is "Password123".

Now, we need to find a login page so we can login using the credentials we have found.

After doing some fuzzing with gobuster on both **/content** and  **/inc** we have found different pages. The one that seems to be a login page, is the **/as**
![[Pasted image 20211217030404.png]]

To log in, use the credentials we have found: 

manager:Password123

![[Pasted image 20211217030624.png]]

Now, after we have logged in, we can see there is a Ads section where we can run code. 

![[Pasted image 20211217033258.png]]

We can try to get a reverse php shell from github:

https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

Make sure to change the ip address to your machine's one, and choose a port that the shell will connect to. Once that is done, click "Done".

![[Pasted image 20211217033451.png]]


Now, it's time to start a listener on our chosen port (I have chosen quad 6):

![[Pasted image 20211217033755.png]]

Once that is done, go to the website to /content/inc/ads.

![[Pasted image 20211217034050.png]]

Access the php script.

And we're in! 

![[Pasted image 20211217034119.png]]

Inside the /home/itguy, we can find our **user.txt** flag.

Use the following for stabilizing your shell: 

/usr/bin/script -qc /bin/bash /dev/null

ps: Thank you, **John Hammond!**

![[Pasted image 20211217034241.png]]

If we run "sudo -l" to see what commands we can run as sudo, we can see the following: 

![[Pasted image 20211220042807.png]]

So, we can use perl and the backup.pl script:
(ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl

Let's cat to see the content of the backup script:

*cat /home/itguy/backup.pl
#!/usr/bin/perl

system("sh", "/etc/copy.sh");*

Looks like the script calls for a bash script. Let's check it out.

While there's nothing interesting inside the script for us by default, since we can run that perl script and it calls to this one, we can put /bin/bash -i inside, which will elevate our privilegies to root:

![[Pasted image 20211220044627.png]]

And that's it! Now just to go the root directory and enjoy your flag! 

