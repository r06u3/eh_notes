![[Pasted image 20211011104448.png]]


# Reconaissance

This is the first phase/stage of ethical hacking.
- Here we gather information about the target. 
- We can do this passively: 
       For instance, we can visit the target's LinkedIn, google about it, etc.
	   
- We can also do this actively, which kind of merges this phase with the second one (**Scanning & Enumeration**)


Resources: 
- https://hunter.io/ (hunter.io, great for searching email addresses)
- https://phonebook.cz/ (email addresses, URLs, Domains)
- clearbit connect
- https://www.emailhippo.com/ (validates an email)
- https://email-checker.net/

!! Don't underestimate "Forgot password"


## Physical/Social 
![[Pasted image 20211011112651.png]]

## Web/Host
![[Pasted image 20211011112738.png]]
# Scanning & Enumeration

This is the second phase: here we are starting to scan and enumerate the target: 
- we check what ports are open;
-  what services are running?
-  what are the services versions? Could the version be vulnerable to any exploits?
-  what is the OS of the target?


# Exploitation

After we are done enumerating (once we have gathered enough information) we can begin finding/developing an exploit. 


# Maintaining access

Once we exploit the target, we need to make sure we get to keep our access, so we can maybe come back at a later time. 


# Cleaning up (covering our tracks)

Once we are done, we should clean up and clear our track: remove any logs, files that might indicate that we have been present on the victim's system.

