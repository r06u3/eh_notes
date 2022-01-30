[[TryHackMe]] [[OWASP]]
##################################
			Cosmin Tibuleac | 11.04.2021
##################################

https://tryhackme.com/room/owaspjuiceshop

# Task 2:
**Question #1: What's the Administrator's email address?**

If we click on the "Apple Juice" product, we can see a review from the admin: 
![[Pasted image 20211104043757.png]]
Looks like leaving a review on the website, will post your email address.


**Question #2: What parameter is used for searching?**

If we search for letter 'k' for example, the search will look like this: http://10.10.90.45/#/search?q=k

Therefore, the letter 'q' is the parameter used for searching.


**Question #3: What show does Jim reference in his review?**

Jim's review: 

 ` jim@juice-sh.op

Fresh out of a replicator. - reference to star trek.`

# Task 3 - Inject the juice

This task will be focusing on injection vulnerabilities. Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

**SQL Injection**  

SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.  

**Command Injection**  

Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.   

**Email Injection**  

Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.

In our case, we will be using an **SQL Injection**

**Question #1: Log into the administrator account!**


We can use Burp Suite to intercept and see the request we are sending to the website while trying to log in: 


![[Pasted image 20211104051121.png]]




{
"email":"a",
"password":"a" }

What we can do here instead, is this: 

{
"email:'or 1=1--"
"password":"a" }

This will log us in as the administrator.

**Why does this work?**

This works because: the character **'** is closing the SQL statement. We then put **or 1=1** which is a conditional logical statement. The server will basically verify if either the email address (the gibberish we inserted) OR 1=1 is true. Since 1 equals 1 is indeed true, this will pass.
The server will "understand" that the email address we used is valid and will log us in as the user 0, which is the administrator account.

 The **--** character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the # and // comment in python and javascript respectively.


**Question #2: Log into the Bender account!**

We will do an injection similar to the one we did previously, but this time we know the email address: bender@juice-sh.op

The injection looks like this:

bender@juice-sh.op'--

Why are we not using the logical statement 1=1 anymore? 

Since this email address is valid (and will return **true**), we don't need to use that OR statement anymore.

Also, if, however we try it like this: bender@juice-sh.op' or 1=1-- we will be logged in as admin once again.

**Note the **1=1** can be used when the email or username is not known or invalid.**


# Task 4 - Who broke my lock?!

In this task, we will look at exploiting authentication through different flaws. When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation. These mechanisms, listed below, are what we will be exploiting. 

Weak passwords in high privileged accounts

Forgotten password pages

 More information: [Broken Authentication](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication)
 
 
 **Question #1: Bruteforce the Administrator account's password!**

We have used SQL Injection to log into the Administrator account but we still don't know the password. Let's try a brute-force attack! We will once again capture a login request, but instead of sending it through the proxy, we will send it to Intruder.

First, we need to go to the login page, input the admin's email address and the password does not really matter as we are going to leave it blank in Burp Suite anyways. For now, just use any random character(s) you wish.

![[Pasted image 20211105062436.png]]

Make sure burp is running and listening then press the login button.

Once you do press the button, burp should intercept the request to the website: 

**POST /rest/user/login HTTP/1.1
Host: 10.10.255.19
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/json
Content-Length: 44
Origin: http://10.10.255.19
Connection: close
Referer: http://10.10.255.19/
Cookie: io=r2ic2pG2ofPAflZxAAAR; language=en; cookieconsent_status=dismiss

{"email":"admin@juice-sh.op","password":"."} **

Right click on the request, in the proxy tab and send it to the intruder: 

![[Pasted image 20211105063027.png]]

In the intruder tab, go to "Positions" and make sure to click "Clear". This is really important, as that special character is telling burp where to try and brute force the password:
![[Pasted image 20211105063134.png]]

Now, after "email", go to the password field, make sure to clear any characters you may have inputted and then click
![[Pasted image 20211105063234.png]] 2 times.

Afterwards, your password field should look like this: 

![[Pasted image 20211105063309.png]]

Now, go to Payloads and click load to load a list. As TryHackMe indicates, we should use *best1050.txt* as the password list.
If you are on Kali Linux, this should be in: /usr/share/seclists.

You can look for it by issuing the following command: 

**find / -type d -name seclists**
That command basically looks through your whole machine, for a directory (hence the -type d) with the name seclists.

If you don't have seclists installed, you can install it directly from apt: *apt install seclists*.

Now, you can load it into Burp Suite.

Click "Start Attack".

If Burp will find a matching password, the status code will be **200** and the length might be different as well.
That's easy for us, since every failed attempt will result in **401** which means *Not authorized*.
![[Pasted image 20211105064235.png]]

Note: You could try to filter the results by the status codes like this: 
![[Pasted image 20211105064335.png]]
However, for me it doesn't seem to work for some reason.


After some time, we can see that on the request #117, the request code and length are different: 

![[Pasted image 20211105071019.png]]

That's because this password matches with the admin's email address!

Now, let's login with the password. Once we do, we will get a flag for tryhackme.

**Question #2: Reset Jim's password!**

"Believe it or not, the reset password mechanism can also be exploited! When inputted into the email field in the Forgot Password page, Jim's security question is set to _"Your eldest siblings middle name?"_. In Task 2, we found that Jim might have something to do with Star Trek. Googling "Jim Star Trek" gives us a wiki page for Jame T. Kirk from Star Trek." 

Alright, by googling "James Kirk star trek family" we get a list of the  character's family: 
**George Kirk (father)** **Winona Kirk (mother)** **George Samuel Kirk (brother)** **Tiberius Kirk (grandfather)** **James** (maternal grandfather) Aurelan Kirk (sister-in-law) Peter Kirk (nephew) 2 other nephews

Looks like he has a brother, with the middle name Samuel. Let's try that.

![[Pasted image 20211105091008.png]]

Success!


# Task 5 - AH! Don't look!

"A web application should store and transmit sensitive data safely and securely. But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack. 

More information: [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A3-Sensitive_Data_Exposure)"


**Question #1: Access the Confidential Document!**

If we check on the **About us** page, we can see a link:

![[Pasted image 20211105091725.png]]

If we hover over it, there seems to be a file legal.md that will be downloaded when clicked. But more importantly, it looks like it's getting it from a FTP  (File transfer Protocol) server:

10.10.75.214/ftp/legal.md
If we try to access it through our browser, we can see that we have acces: 

![[Pasted image 20211105091953.png]]

You will get the flag once you download the file.

**Question #2: Log into MC SafeSearch's account!**

https://www.youtube.com/watch?time_continue=16&v=v59CX2DiX0Y&feature=emb_logo

"After watching the video there are certain parts of the song that stand out.

He notes that his password is "Mr. Noodles" but he has replaced some "vowels into zeros", meaning that he just replaced the o's into 0's.

We now know the password to the _mc.safesearch@juice-sh.op_ account is "**Mr. N00dles**""

