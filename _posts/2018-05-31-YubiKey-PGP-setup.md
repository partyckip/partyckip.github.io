---
layout: post
title: "YubiKey PGP setup"
date: 2018-05-31
---

# How to configure 2 YubiKeys with the same PGP on Windows 10

## Install the latest version of Gpg4win (here 3.1.1 version was used, which contains GnuPG version 2.2.7)

## Let's check if installation worked, start command line

To keep English output set LANG variable.

    set LANG=C
    gpg --version


#### Check if you do not have any keys on your YubiKeys (both)

    gpg --card-status
    
    Signature key ....: [none]
    Encryption key....: [none]
    Authentication key: [none]
    General key info..: [none]

#### Configure both YubiKeys

Issue the same commands on both YubiKeys. In the rest of tutorial I have anonymized by personal details.

```
gpg --expert --edit-card

Reader ...........: Yubico Yubikey 4 OTP U2F CCID 0
Application ID ...: D2760001240102010006072128450000
Version ..........: 2.1
Manufacturer .....: Yubico
Serial number ....: 07212845
Name of cardholder: [not set]
Language prefs ...: [not set]
Sex ..............: unspecified
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 0 3
Signature counter : 0
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

gpg/card> admin
Admin commands are allowed

gpg/card> passwd
gpg: OpenPGP card no. D2760001240102010006072128450000 detected

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? 3
PIN changed.

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? 1
PIN changed.

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? q

gpg/card> name
Cardholder's surname: John
Cardholder's given name: Smith

gpg/card> lang
Language preferences: en

gpg/card> sex
Sex ((M)ale, (F)emale or space): M

gpg/card> url
URL to retrieve public key: https://keybase.io/jsmith/pgp_keys.asc

gpg/card> login
Login data (account name): john.smith@gmail.com

gpg/card> list

Reader ...........: Yubico Yubikey 4 OTP U2F CCID 0
Application ID ...: D2760001240102010006072128450000
Version ..........: 2.1
Manufacturer .....: Yubico
Serial number ....: 07212845
Name of cardholder: John Smith
Language prefs ...: en
Sex ..............: male
URL of public key : https://keybase.io/jsmith/pgp_keys.asc
Login data .......: john.smith@gmail.com
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 0 3
Signature counter : 0
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
```

#### Create your offline master key (if you do not have it yet)

```
gpg --expert --full-generate-key

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
Your selection? 8
```

Toogle cappabilities to only have Certify

```
Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Sign Certify Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Certify Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? E

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
```

we’ll use a 4096-bit key as master

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
```

An expiry date is important so if you forget about the key (unlikely) it become invalid
In new version of GnuPG revocation keys are generated automatically

