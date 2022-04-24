https://tryhackme.com/room/steelmountain


#################################
			Cosmin Tibuleac | 04.03.2022
#################################


# Task 1  Introduction

1. Who is the employee of the month?

` Bill Harper`


# Task 2  Initial Access


1. Scan the machine with nmap. What is the other port running a web server on?

`8080`

2. Take a look at the other web server. What file server is running?

`Rejetto HTTP File Server`

3.What is the CVE number to exploit this file server?

`2014-6287 `

4. Use Metasploit to get an initial shell. What is the user flag?

`b04763b6fcf51fcd7c13abc7db4fd365`


# Task 3  Privilege Escalation

1. Take close attention to the CanRestart option that is set to true. What is the name of the service which shows up as an _unquoted service path_ vulnerability?

`AdvancedSystemCareService9`

2. What is the root flag?

`9af5f314f57607c00fd09803a587db80`




ServiceName                     : AdvancedSystemCareService9                                                                  
Path                            : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe                             
ModifiableFile                  : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe                             
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}                      
ModifiableFileIdentityReference : STEELMOUNTAIN\bill                                                                          
StartName                       : LocalSystem                                                                                 
	AbuseFunction                   : Install-ServiceBinary -Name 'AdvancedSystemCareService9'                                    
CanRestart                      : True                                                                                        
Name                            : AdvancedSystemCareService9                                                                  
Check                           : Modifiable Service Files                 

# Task 4  Access and Escalation Without Metasploit

