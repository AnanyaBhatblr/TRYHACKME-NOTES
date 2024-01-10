Hashes are computed and security is maintain because of the implementation of the mathematical concept P vs NP relationship that proves fundamental to computing and cryptography, abstractly it means that the algorithm to hash the value will be "NP" and can therefore be calculated reasonably. However an un-hashing algorithm would be "P" and intractable to solve- meaning that it cannot be computed in a reasonable time using standard computers.

This process is called a **dictionary attack** and John the Ripper, or John as it's commonly shortened to, is a tool to allow you to conduct fast brute force attacks on a large array of different hash types.

##### syntax
basic
`john [options] [path to file]`
automatic cracking
`john --wordlist=[path to wordlist] [path to file]`
format specific
`john --format=[format] --wordlist=[path to wordlist] [path to file]`
`john --list=formats`
`john --list=formats | grep -iF "md5"`
##### hash identifier  
- [hashidentifier](https://hashes.com/en/tools/hash_identifier)
- `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`
`python3 hash-id.py`

just use hash-id.py
```bash
python hash-id.py thehash
```

hash1 - `2e728dd31fb5949bc39cac5a9f066498`
use 
```bash
$ python hash-id.py 2e728dd31fb5949bc39cac5a9f066498
```

```
```bash
$ john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hash1.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 512/512 AVX512BW 16x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
biscuit          (?)     
1g 0:00:00:00 DONE (2023-12-08 17:40) 3.448g/s 10593p/s 10593c/s 10593C/s skyblue..dangerous
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed.
```

password - biscuit

hash2 -`1A732667F3917C0F4AA98BB13011B9090C6F8065`

doing same as above, type is raw-sha1
password: kangeroo

hash3 - `D7F4D3CCEE7ACD3DD7FAD3AC2BE2AAE9C44F4E9B7FB802D73136D4C53920140A`
password: microphone

hash4 - `c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf`
password: colossal

# NTLM
NThash is the hash format that modern Windows Operating System machines will store user and service passwords in. It's also commonly referred to as "NTLM" which references the previous version of Windows format for hashing passwords known as "LM", thus "NT/LM".

# Cracking /etc/shadow Hashes
`unshadow [path to passwd] [path to shadow]`
John can be very particular about the formats it needs data in to be able to work with it, for this reason- in order to crack /etc/shadow passwords, you must combine it with the /etc/passwd file in order for John to understand the data it's being given.

```bash
$ echo 'root:x:0:0::/root:/bin/bash'>passwd                                     
```

```bash
$ echo 'root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::'>shadow
```

```bash
unshadow passwd passwd > unshadowed.txt
```

# Single Crack Mode
So far we've been using John's wordlist mode to deal with brute forcing simple., and not so simple hashes. But John also has another mode, called Single Crack mode. In this mode, John uses only the information provided in the username, to try and work out possible passwords heuristically, by slightly changing the letters and numbers contained within the username.

``john --single --format=[format] [path to file]``

```bash
$ john --single --format=raw-md5 hash7.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 512/512 AVX512BW 16x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 23 candidates buffered for the current salt, minimum 48 needed for performance.
Jok3r            (Joker)     
1g 0:00:00:00 DONE (2023-12-08 18:30) 4.761g/s 971.4p/s 971.4c/s 971.4C/s JOKER1..gjoker
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```
before using john the ripper make sure to add the username to the text file in this format
`username:hash`

# Custom Rules
The first line:

`[List.Rules:THMRules]` - Is used to define the name of your rule, this is what you will use to call your custom rule as a John argument.

We then use a regex style pattern match to define where in the word will be modified, again- we will only cover the basic and most common modifiers here:

`Az` - Takes the word and appends it with the characters you define  

`A0` - Takes the word and prepends it with the characters you define  

`c` - Capitalises the character positionally

`[0-9]` - Will include numbers 0-9  

`[0]` - Will include only the number 0  

`[A-z]` - Will include both upper and lowercase  

`[A-Z]` - Will include only uppercase letters  

`[a-z]` - Will include only lowercase letters  

`[a]` - Will include only a  

`[!£$%@]` - Will include the symbols !£$%@

example
`[List.Rules:PoloPassword]`

`cAz"[0-9] [!£$%@]"`

---
`--rule=PoloPassword` flag.  

As a full command: `john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]`


---

# ZIP2JOHN
converts zip file to hash format
`zip2john [options] [zip file] > [output file]`
to crack it
``john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt``
- follow the above process and open the file with the password (pass123) to get the flag
**FLAG - `THM{w3ll_d0n3_h4sh_r0y4l}`**

```bash
$ zip2john secure.zip>secure.txt                                     
ver 1.0 efh 5455 efh 7875 secure.zip/zippy/flag.txt PKZIP Encr: 2b chk, TS_chk, cmplen=38, decmplen=26, crc=849AB5A6 ts=B689 cs=b689 type=0

$ john --wordlist=/usr/share/wordlists/rockyou.txt secure.txt        
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
pass123          (secure.zip/zippy/flag.txt)     
1g 0:00:00:00 DONE (2023-12-08 18:59) 1.449g/s 11872p/s 11872c/s 11872C/s 123456..whitetiger
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

# Cracking Password Protected RAR Archives

`rar2john [rar file] > [output file]   `

`john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt`

on solving the challenge using the above syntax
**FLAG - `THM{r4r_4rch1ve5_th15_t1m3}`**

# Cracking SSH Keys with John
`ssh2john [id_rsa private key file] > [output file]`
`john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt`

password: `mango`

