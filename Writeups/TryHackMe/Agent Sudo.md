

[[BURP-SUITE]] [[TryHackMe]] 
[[John The Ripper]]  [[binwalk]] [[strings]] [[Steghide]]

##############################
        Cosmin Tibuleac | 06.11.2021
##############################

https://tryhackme.com/room/agentsudoctf

export IP=10.10.189.162

# Task 1 - Author note

Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!  
  
If you are stuck inside the black hole, post on the forum or ask in the TryHackMe discord.

# Task 2 - Enumerate
 
 ## Question 1: How many open ports?
 
 Time to begin our reconnaissance:
 
 nmap -A -T4 -p- $IP -o nmap.txt
 
 `Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-06 12:43 EDT
Stats: 0:00:37 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 97.65% done; ETC: 12:44 (0:00:01 remaining)
Stats: 0:00:37 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 98.15% done; ETC: 12:44 (0:00:01 remaining)
Nmap scan report for 10.10.189.162
Host is up (0.085s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Annoucement
|_http-server-header: Apache/2.4.29 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=11/6%OT=21%CT=1%CU=36907%PV=Y%DS=2%DC=T%G=Y%TM=6186B0F
OS:6%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=107%TI=Z%CI=I%II=I%TS=A)OPS
OS:(O1=M505ST11NW7%O2=M505ST11NW7%O3=M505NNT11NW7%O4=M505ST11NW7%O5=M505ST1
OS:1NW7%O6=M505ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M505NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 23/tcp)
HOP RTT      ADDRESS
1   62.79 ms 10.11.0.1
2   63.48 ms 10.10.189.162

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 61.86 seconds`

Looks like the following ports are open on the machine: 80, 21, 22.
So the answer to this question is 3.

## Question 2: How you redirect yourself to a secret page?

Let's try and visit the website

![[Pasted image 20211106124734.png]]

By seeing "user-agent", this indicates that somehow, we should alter our user-agent when visiting the webpage, and that might redirect us to a different/secret webpage.

What is the user-agent?

The **User-Agent** [request header](https://developer.mozilla.org/en-US/docs/Glossary/Request_header) is a characteristic string that lets servers and network peers identify the application, operating system, vendor, and/or version of the requesting [user agent](https://developer.mozilla.org/en-US/docs/Glossary/User_agent).

Source: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent


Alright, since the author of the "note" on the website is called "Agent R", this might indicate that the agents are using single alphabet letters for their codename. So, we should fire up Burp suite and alter our **User-agent** and then send it to the website to see if that works.

Set up Burp Suite, and then refresh the page.

Now, if everything is set up correctly, burp should intercept your request to the webserver and it should look something like this: 

![[Pasted image 20211106125349.png]]

Send it to the repeater within Burp-Suite

Note: The repeater allows us to send a request to the website and then shows us the **Response** of the website, which is super helpful. 

Erase the user-agent string and replace it with a letter, like this: 

![[Pasted image 20211106125512.png]]

*Note: I restarted the machine and that's the reason you will be seeing a different IP address from now on.*

A doesn't seem to work, let's try R since we saw that the note was left by "Agent R"

![[Pasted image 20211107103258.png]]

R is not it, but this indicates we're on the good path.

Let's try B:

B doesn't seem to work.

Finally, C seems to redirect us: 

![[Pasted image 20211107103445.png]]

Upon going on the redirection page: agent_C_attention.php, we see the following text: 

"Attention chris,  
  
Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!  
  
From,  
Agent R"

So for this question: 
**How you redirect yourself to a secret page?**
The **answer**  is: user-agent.

## Question 3: What is the agent name?

**Answer**: As we've seen, the text above is for a user named "chris".

# Task 3: Hash cracking and brute-force

"Done enumerate the machine? Time to brute your way out."
 ## Question 1: FTP password
Alright, so we have seen that on the machine, FTP port is open, and now that we know a username (chris, potentially), we could try to fire up **Hydra** and try to brute-force the password for it. 

Let's give it a go:

We can issue the following command:

hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.23.98 -vv 

where:
-l stands for the user (since we already know a user and don't need to use a user list)

-P is the flag we use for a password list, and we are going to use the password list rockyou.txt, which by default is stored in /usr/share/wordlists on Kali.

Then we give **Hydra** the service (ftp) and IP address of our target.

-vv is just for a verbose output, so we can see each attempt. This is optional.

Now, when you are ready, hit the enter button and give it some time to finish.

Awesome, Hydra managed to find **chris's** password: 

![[Pasted image 20211107104905.png]]

So now we know a user and password for FTP:

user: chris
password: crystal

# Question 2: Zip file password

Let's try and login into the FTP
ftp 10.10.23.98

We're in! Sweet!

So, the answer for the Question 1  FTP password is: crystal.

If we use "ls" to list the files inside the FTP server, we can see there are 2 pictures and one .txt: 

![[Pasted image 20211107121223.png]]

we can use the mget * command to get everything from the server.

Alright, now that we have all the files, let's first examine the .txt

"Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C"

So, this tells us that, probably, the password is somehow stored in one of the pictures.

I have already checked the pictures for exif data, but there's nothing there.

So, let's try to use **strings**:

We can use it like this: 

strings *.png

While there's nothing in the .jpg file, it seems that there's a txt file inside the png one: 

![[Pasted image 20211107125044.png]]

If we use binwalk on the picture, we can see that it actually contains a zip archive. We can use binwalk with the -e flag to extract the contents from the picture:
We will get the following folder: 

_cutie.png.extracted

Once we cd into it, we can see the following items: 

![[Pasted image 20211107125312.png]]

Trying to read the the .txt To_agentR doesn't seem to output anything.

If we try to unzip the archive, it skips it and lets us know that it needs a password.

Time for John The Ripper!

John has a module called zip2john, which basically will crack the password for us.
Let's try it. We can use the following syntax: 

zip2john 8702.zip > john.txt

It will generate a hash that john understands and is able to crack it.

It should look like this: 

![[Pasted image 20211107130836.png]]

Now, we can give john a password list and the john.txt and let him try and crack the password:


**john --format=zip --  	    wordlist=/usr/share/wordlists/rockyou.txt john.txt**

The password is: alien

Alright, so now that we know the password, we can unzip the archive using the following command:

**7z e 8702.zip**

Press Y and then enter the password John has cracked for us, to unzip the files.

Sweet, now we can read that txt file: 

![[Pasted image 20211108112514.png]]


## Question 3: steg password

If we try to decode that strange string using base64, we get the following: 

(root💀kali)-[~/…/thm/agentSudo/ftp/_cutie.png.extracted]
└─# echo QXJlYTUx | base64 -d                        
Area51                     

"Area51" is the answer for this question.

## Question 4: Who is the other agent (in full name)?

Now, what we can do, is try to use a tool called **steghide** to check in any of those images for any information.

What is Steghide? 

Steghide is a tool that allows us to either embed information inside a audio or image file, or to **extract** it.

Let's check to see if any of those pictures have any hidden information inside.

If you don't already have steghide installed, you can use apt: 

apt install steghide

Once it is installed, the syntax is: 

steghide extract -sf file

![[Pasted image 20211111102400.png]]

When asked for the passphrase, input the password we had just found and decoded using base64: "Area51" without the quotes.

![[Pasted image 20211111102701.png]]

Sweet! So now we have a username, "james" and a password "hackerrules", which will most likely allow us to ssh into the machine!

And damn, agent R, cheesy indeed!

So, for this question the answer is: james

## Question 5: SSH password

We have just found it: hackerrules! (the exclamation symbol is included)

# Task 4: Capture the user flag

"You know the drill."

## Question 1: What is the user flag?

Once we're logged in, we will be in /home/james.
Upon doing an ls, we can see a jpg file and a user_flag.txt
For the user flag, just cat user_flag.txt


## Question 2: What is the incident of the photo called?

Once we have ssh'ed into the machine, we can open another terminal and use the scp command to get the image from the server like this: 

scp james@IP:/home/james/Alien_autospy.jpg .

Now ,to answer this question, we need to do a reverse image search on google.

You may not get what the question is asking you specifically, but the hint tells you to use reverse image search and foxnews.

Normally, if you only do the reverse image search, you will get the 'normal' results like me with news and articles about the picture, but if you add "foxnews", you will get a foxnews article that reveals the incident was a hoax and it is called: "Roswell Alien Autopsy".















