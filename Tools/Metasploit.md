# METASPLOIT
#tool

Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. Maintained by Rapid 7, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools. Throughout this room, we will explore the basics of using this massive framework and a few of the modules it includes.

### Interesting commands:

**setg** - sets the variables globally;

**banner** - shows the graphical banner, just for fun;

**info** - displays info about a certain module;

**unset** - set the value of a variable to null;

**spool** - to save the console output to a file.

**whoami /all | whoami /priv** - shows current user privilegies.

**getprivs.**

### **Interesting metasploit modules:**

-   **post/windows/gather/checkvm** - _checks whether or not the machine is a virtual machine._
    
-   **run post/multi/recon/local_exploit_suggester** - _this checks for various exploits which we can run within our session to elevate our privileges.
    
-   **run post/windows/manage/enable_rdp**  - tries forcing RDP to be available.
 


![[Pasted image 20211008120843.png]]