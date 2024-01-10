**Symmetric encryption** uses the same key to encrypt and decrypt the data. Examples of Symmetric encryption are DES (Broken) and AES. These algorithms tend to be faster than asymmetric cryptography, and use smaller keys (128 or 256 bit keys are common for AES, DES keys are 56 bits long).

**Asymmetric encryption** uses a pair of keys, one to encrypt and the other in the pair to decrypt. Examples are RSA and Elliptic Curve Cryptography. Normally these keys are referred to as a public key and a private key. Data encrypted with the private key can be decrypted with the public key, and vice versa. Your private key needs to be kept private, hence the name. Asymmetric encryption tends to be slower and uses larger keys, for example RSA typically uses 2048 to 4096 bit keys.

# RSA - Rivest Shamir Adleman

https://github.com/RsaCtfTool/RsaCtfTool
[https://github.com/ius/rsatool](https://github.com/ius/rsatool).

- “p” and “q” are large prime numbers, “n” is the product of p and q.
- The public key is n and e, the private key is n and d.
- “m” is used to represent the message (in plaintext) and “c” represents the ciphertext (encrypted text).

. [Diffie Hellman Key Exchang](https://www.youtube.com/watch?v=NmM9HA2MQGI)

# PGP, GPG and AES

PGP stands for Pretty Good Privacy. It’s a software that implements encryption for encrypting files, performing digital signing and more.

[GnuPG or GPG](https://gnupg.org/) is an Open Source implementation of PGP from the GNU project.

AES, sometimes called Rijndael after its creators, stands for Advanced Encryption Standard. It was a replacement for DES which had short keys and other cryptographic flaws.

```bash
$ binwalk -e gpg.zip            

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Zip archive data, at least v2.0 to extract, compressed size: 263, uncompressed size: 263, name: message.gpg
304           0x130           Zip archive data, at least v2.0 to extract, compressed size: 1366, uncompressed size: 1367, name: tryhackme.key
1829          0x725           End of Zip archive, footer length: 22
$ ls
0.zip  message.gpg  tryhackme.key

$ gpg --help  

$ gpg --import tryhackme.key
gpg: /home/ananya/.gnupg/trustdb.gpg: trustdb created
gpg: key FFA4B5252BAEB2E6: public key "TryHackMe (Example Key)" imported
gpg: key FFA4B5252BAEB2E6: secret key imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1

$ gpg --output message.txt --decrypt message.gpg
gpg: encrypted with 1024-bit RSA key, ID 2A0A5FDC5081B1C5, created 2020-06-30
      "TryHackMe (Example Key)"
      
$ cat message.txt      
You decrypted the file!
The secret word is Pineapple.

```

# The Future - Quantum Computers and Encryption
The NSA recommends using RSA-3072 or better for asymmetric encryption and AES-256 or better for symmetric encryption
