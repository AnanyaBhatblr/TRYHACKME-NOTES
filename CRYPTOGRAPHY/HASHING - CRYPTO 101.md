###### HASH FUNCTIONS
A hash function takes input, digests it and makes a summary of fixed output

###### HASH COLLISION
when 2 inputs have same hash

If you have an 8 bit hash output, how many possible hashes are there?
ans 2^8 = 256

Rainbow table - collection of hashes and passwords
salts are used to make it resistant to rainbow tables

Resources
crackstation , https://hashes.com/en/decrypt/hash, https://pypi.org/project/hashID/, [hashcat](https://hashcat.net/wiki/doku.php?id=example_hashes)
``

On Linux, password hashes are stored in /etc/shadow. This file is normally only readable by root. They used to be stored in /etc/passwd, and were readable by everyone.
Here's a quick table of the most Unix style password prefixes that you'll see.

|   |   |
|---|---|
|Prefix|Algorithm|
|$1$|md5crypt, used in Cisco stuff and older Linux/Unix systems|
|$2$, $2a$, $2b$, $2x$, $2y$|Bcrypt (Popular for web applications)|
|$6$|sha512crypt (Default for most Linux/Unix systems)|

**TASKS - CRACK THESE HASHES**
1. ``$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG`
from the 2a we know it's bycrypt for which we need -m 3200 on hashcat
store the hash in a file called bycrpt.txt and run the command
```bash
$ hashcat -m 3200 -a 0 -o cracked.txt bycrypt.txt /usr/share/wordlists/rockyou.txt
```
now do cat cracked.txt to get the password: `85208520`

2. `9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1`
```bash
$ hashcat -a 0 -o cracked.txt bycrypt.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting in autodetect mode

OpenCL API (OpenCL 3.0 PoCL 3.1+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 15.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: pthread-skylake-avx512-Intel(R) Core(TM) i3-1005G1 CPU @ 1.20GHz, 2785/5635 MB (1024 MB allocatable), 4MCU

The following 8 hash-modes match the structure of your input hash:

      # | Name                                                       | Category
  ======+============================================================+======================================
   1400 | SHA2-256                                                   | Raw Hash
  17400 | SHA3-256                                                   | Raw Hash
  11700 | GOST R 34.11-2012 (Streebog) 256-bit, big-endian           | Raw Hash
   6900 | GOST R 34.11-94                                            | Raw Hash
  17800 | Keccak-256                                                 | Raw Hash
   1470 | sha256(utf16le($pass))                                     | Raw Hash
  20800 | sha256(md5($pass))                                         | Raw Hash salted and/or iterated
  21400 | sha256(sha256_bin($pass))                                  | Raw Hash salted and/or iterated

Please specify the hash-mode with -m [hash-mode].

Started: Fri Dec  8 10:29:52 2023
Stopped: Fri Dec  8 10:29:56 2023
```

now run this command with the hashmode to crack the hash
```bash
$ hashcat -m 1400 -a 0 -o cracked.txt bycrypt.txt /usr/share/wordlists/rockyou.txt
```
password: `halloween`

3. `$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0`
this is SHA512, mode 1800
```bash
$ hashcat -m 1800 -a 0 -o cracked.txt bycrypt.txt /usr/share/wordlists/rockyou.txt
```
password:`spaceman`

4. `b6b0d451bbf6fed658659a9e7e5598fe`
use [crackstation](https://crackstation.net/)as rockyou isn't enough
password:`funforyou`

TO FIND MODE YOU CAN ALSO
```bash
`hashcat -h | grep -i 'hmac-sha512`
```
