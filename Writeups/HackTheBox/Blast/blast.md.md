############################
 		Cosmin Tibuleac | 16.01.2022
############################




export IP=10.10.200.95


# Task 2: Activate Forward Scanners and Launch Proton Torpedoes

`1.How many ports are open on our target system?`
A: 2

`2.Looks like there's a web server running, what is the title of the page we discover when browsing to it?`
A: IIS Windows Server

`3.Interesting, let's see if there's anything else on this web server by fuzzing it. What hidden directory do we discover?`

*gobuster dir -u $IP --wordlist=/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | tee gobuster.log*

![[Pasted image 20220116040238.png]]
A:  /retro


`4.Navigate to our discovered hidden directory, what potential username do we discover?`

![[Pasted image 20220116040429.png]]

A: wade

`5.rawling through the posts, it seems like our user has had some difficulties logging in recently. What possible password do we discover?`

Going to the following link: http://10.10.200.95/retro/index.php/author/wade/ ,we can see there is a "comments" section and upon clicking the section we will download the comments file:

![[Pasted image 20220116041444.png]]


A: parzival

`6.Log into the machine via Microsoft Remote Desktop (MSRDP) and read user.txt. What are it's contents?`

For this we can use "Remmina" as the author of the room recommends.

To install it: apt install remmina

![[Pasted image 20220116041941.png]]

To log in, use the credentials found: **wade:parzival**

![[Pasted image 20220116042050.png]]
 
 ![[Pasted image 20220116042137.png]]
 
A: THM{HACK_PLAYER_ONE}



# Task 3: Breaching the Control Room

`1.When enumerating a machine, it's often useful to look at what the user was last doing. Look around the machine and see if you can find the CVE which was researched on this server. What CVE was it?`

Note: **took this answer from another writeup since there's a bug with the machine and there is no history being kept on the browser anymore.**

A: CVE-2019-1388


`2.Looks like an executable file is necessary for exploitation of this vulnerability and the user didn't really clean up very well after testing it. What is the name of this executable?`

