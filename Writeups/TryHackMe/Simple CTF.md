############################
 		Cosmin Tibuleac | 12/11/2021
############################



https://tryhackme.com/room/easyctf


export IP=10.10.243.244

nmap -A -T4 -p- $IP -o nmap.txt


# Task 1: Simple CTF

Deploy the machine and attempt the questions!

## Question 1: How many services are running under port 1000? 

For this, we can run the following nmap command:

nmap -A -T4 -p 1-999 -o nmap.txt 

Answer: 

``
2
``

## Question 2: What is running on the higher port?
We can use this, to scan for all the ports on the target machine:

nmap -A -T4 -p- $IP -o nmap.txt 


After the complete nmap scan has finished, we can see that the port 2222 is open and it's running ssh: 

*2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)*


So, the answer for this question is: `ssh`

## Question 3: What's the CVE you're using against the application? 


FTP is open on port 21 and it looks like **anonymous** login is allowed. 

After we have logged in on the machine, we can see there is a folder "pub" with a txt file inside: ForMitch.txt

We can use: get ForMitch.txt

Now, we can read it on our system:

"Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!"

So, now we have a potential username: Mitch/mitch, 
and we know the password is weak.

Now, I initially tried to crack the ssh password with hydra, but that didn't seem to lead anywhere, so I started **gobuster** to enumerate the directory listing on the website and that found something juicy for us!

We can run gobuster like this: 

**gobuster dir --url http://$IP --wordlist=/usr/share/wordlists/rockyou.txt | tee gobuster.log**

After running for a few seconds, we can already see that it has found a directory:

![[Pasted image 20211112073206.png]]

Once we go on this directory, on the website, we can see that it is a software called "CMS".

If we scroll to the bottom of the page, in the left bottom corner we can see the version:

![[Pasted image 20211112073338.png]]


The version seems to be 2.2.8

If we google "CMS 2.2.8" we find the following exploit-db link: 

https://www.exploit-db.com/exploits/46635

If we take a look at it, we can see the CVE is: 2019-9053

So, the answer for this question is: `CVE-2019-9053`

Now, we can download the exploit from the page mentioned above.

Once we have it on our system, it's time to mark it as executable: 

chmod +x 46635.py

You will then need to run it like this:

python 46635.py -u http://10.10.243.244/simple/ 

If you get an error that a certain module, like termcolor is missing, you can install the respective module with pip:

pip install modulename

Now, once everything is set, let the script run. 
After some time, we will see the following:

![[Pasted image 20211112082524.png]]

## Question 4: To what kind of vulnerability is the application vulnerable?

Answer `sqli` - SQL Injection. The python script is doing a SQL Injection.

## Question 5: What's the password?

Now that we know the username and that the password is for ssh, we can try to have **hydra** attempt brute-force the password:

hydra -l mitch -P /usr/share/wordlists/rockyou.txt 10.10.168.32 ssh  -s 2222 -t 4 -vv | tee hydra.log

Pretty quickly, we can see that Hydra managed to find the password: 

![[Pasted image 20211112101350.png]]

Answer for this question: `secret`

## Question 6: What's the user flag?

Now that we know both the user and the password, it's time to ssh into the machine.

Note: don't forget to use the flag -p to specify the port 2222 since ssh is not running on the default 22 port:

*ssh mitch@10.10.168.32 -p 2222*

And we are in!

And here is the user flag:

![[Pasted image 20211112110847.png]]

## Question 7: Is there any other user in the home directory? What's its name?

If we cd .. , we can there is another directory that we cannot access:

![[Pasted image 20211112111037.png]]

Answer: `sunbath`

## Question 8: What can you leverage to spawn a privileged shell?

Alright, now we should try to maybe upload a privilege escalation vectors check, like linPEAS.
We can do this by using **scp**:

First, if you don't have it, you can download it with git from here: https://github.com/carlospolop/PEASS-ng.git

Type: git clone https://github.com/carlospolop/PEASS-ng.git

in your terminal.

Now, go to the location of your linpeas.sh and type the following in your terminal:

**scp -P 2222 linpeas.sh mitch@10.10.50.106:/home/mitch**

Type the password *secret* when prompt.

Once it's uploaded into the machine, mark it as executable by doing *chmod +x linpeas.sh* and now it's time to run it:

*./linpeas.sh | tee linpeas.log*

The tee part is optional, that's only if you want linpeas to create a log file for you to have and look at.

Now give it some time to finish, and once it's done, if we scroll through, the following pops:

![[Pasted image 20211113003311.png]]

Of course, alternatively we could have just run "sudo -l" to see what we can run with sudo, and that's good practice, and would have saved us some time.

If we google "vim root shell", we can GTFOBins as the first entry: https://gtfobins.github.io/gtfobins/vim/

![[Pasted image 20211113003549.png]]

So, let's try the first variant:

![[Pasted image 20211113003635.png]]


Afterwards, type "clear" to clear your terminal from all the mess.
And we are now root!

So, the answer for this question is: `vim`

![[Pasted image 20211113003724.png]]


## Question 9: What's the root flag?

Let's cd to /root, and there we can find the root.txt file. 
Cat it and get the flag!


![[Pasted image 20211113003817.png]]





