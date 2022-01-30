https://tryhackme.com/room/picklerick


#################################
			Cosmin Tibuleac | 22.12.2021
#################################



export IP=10.10.57.231
  
  # Nmap: 
  
  *nmap -sC -sV -Pn -T4 -p- $IP | tee nmap.txt*
  
  
   **Note to self, remember username!
    Username: R1ckRul3s**
	
	
	Nmap scan report for 10.10.57.231
Host is up (0.067s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 42:a1:bb:37:44:86:a7:a0:fb:18:aa:88:0f:3e:75:e5 (RSA)
|   256 dd:38:91:c6:c6:42:b5:98:3a:ae:5d:fd:65:85:18:4e (ECDSA)
|_  256 bf:78:3d:83:be:ed:7d:c9:3b:42:7e:9b:1f:59:27:59 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 935.75 seconds