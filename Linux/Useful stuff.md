  
# Interesting/helpful commands:
**echo $SHELL** = *displays the current user's shell.*

**hostnamectl** = *displays distribution name and version.*

**export IP=10.10.10.10** = *sets the variable IP to be equal to a number, in this case an ip, which can help us using the IP along the session.*

| tee

# SEARCH THE MAN WITH / -example

# Interesting/helpful files:

**/etc/passwd** = *great for enumerating users, but also for seeing a user's shell.*

cat /etc/update-motd.d/00-header


# FIND SUID FILES
find / -perm -4000 2>/dev/null
find / -perm -6000 2>/dev/null




# REMEMBER!

## IF YOU ARE DEALING WITH A CERTAIN APP/WEBSITE THAT REQUIRES TO LOG IN, A GOOD IDEA IS TO TRY THE DEFAULT CREDS.


## If you are presented with an upload form, and you try to upload a file with a certain extension and it seems to be not allowed, try other similar extensions: if .php is not allowed, try php2, php3, php4, php5, phtml.


# Creating a Symbolic link exameple:
**ln -s /home/kali/Downloads/./Obsidian-0.12.19.AppImage /bin/obsidian**