# Hydra
#tool 

Hydra is a brute force online password cracking program; a quick system login password 'hacking' tool.

We can use Hydra to run through a list and 'bruteforce' some 
authentication service. Imagine trying to manually guess someones 
password on a particular service (SSH, Web Application Form, FTP or 
SNMP) - we can use Hydra to run through a password list and speed this 
process up for us, determining the correct password.

Hydra has the ability to bruteforce the following protocols: **Asterisk, AFP, 
Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP,  HTTP-FORM-GET,
 HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, 
HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, 
HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle 
Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, 
Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP 
v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), 
Telnet, VMware-Auth, VNC and XMPP.**

For more information on the options of each protocol in Hydra, read the official Kali Hydra tool page: [https://en.kali.tools/?p=220](https://en.kali.tools/?p=220)

This shows the importance of using a strong password, if your password is 
common, doesn't contain special characters and/or is not above 8 
characters, its going to be prone to being guessed. 100 million password
 lists exist containing common passwords, so when an out-of-the-box 
application uses an easy password to login, make sure to change it from 
the default! Often CCTV camera's and web frameworks use admin:password 
as the default password, which is obviously not strong enough.

Example of http brute force login for POST form

**hydra -l <username> -P <wordlist> 10.10.158.27 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V **

**hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.158.27 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect."**
	
hydra -l  /usr/share/wordlists/rockyou.txt ip http-post-form "/login:username=^USER^&password=^PASS^:Invalid username or password. "
	
	
	