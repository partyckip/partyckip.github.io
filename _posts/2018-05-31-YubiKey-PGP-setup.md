---
layout: post
title: "YubiKey PGP setup"
date: 2018-05-31
---

# How to configure 2 YubiKeys with the same PGP on Windows 10

## Install the latest version of Gpg4win

Here 3.1.1 version was used, which contains GnuPG version 2.2.7.

## Let's check if installation worked, start command line

To keep English output set LANG variable.

    set LANG=C
    gpg --version

## Check if you do not have any keys on your YubiKeys (both)

    gpg --card-status
    
    Signature key ....: [none]
    Encryption key....: [none]
    Authentication key: [none]
    General key info..: [none]

## Configure both YubiKeys

Issue the same commands on both YubiKeys.

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
    
## Create your offline master key (if you do not have it yet)

Toggle all actions and leave only Certify.
    
    gpg --expert --full-generate-key
    
    gpg: keybox 'C:/Users/John Smith/AppData/Roaming/gnupg/pubring.kbx' created
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
    
    Possible actions for a RSA key: Sign Certify Encrypt Authenticate
    Current allowed actions: Sign Certify Encrypt
    
       (S) Toggle the sign capability
       (E) Toggle the encrypt capability
       (A) Toggle the authenticate capability
       (Q) Finished
    
    Your selection? s
    
    Possible actions for a RSA key: Sign Certify Encrypt Authenticate
    Current allowed actions: Certify Encrypt
    
       (S) Toggle the sign capability
       (E) Toggle the encrypt capability
       (A) Toggle the authenticate capability
       (Q) Finished
    
    Your selection? e
    
    Possible actions for a RSA key: Sign Certify Encrypt Authenticate
    Current allowed actions: Certify
        
       (S) Toggle the sign capability
       (E) Toggle the encrypt capability
       (A) Toggle the authenticate capability
       (Q) Finished
    
    Your selection? q
    RSA keys may be between 1024 and 4096 bits long.

My YubiKey are supporting only 2048 bits, but master key should be created with 4096.

    What keysize do you want? (2048) 4096
    Requested keysize is 4096 bits
    
An expiry date is important so if you forget about the key (unlikely) it become invalid
	
	Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 1y
    Key expires at 06/01/19 15:56:10 Central European Daylight Time
    Is this correct? (y/N) y
    
    GnuPG needs to construct a user ID to identify your key.
    
    Real name: John Smith
    Email address: john.smith@gmail.com
    Comment:
    You selected this USER-ID:
        "John Smith <john.smith@gmail.com>"
    
    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    gpg: C:/Users/John Smith/AppData/Roaming/gnupg/trustdb.gpg: trustdb created
    gpg: key A81B833AF353ADF4 marked as ultimately trusted
    gpg: directory 'C:/Users/John Smith/AppData/Roaming/gnupg/openpgp-revocs.d' created
    gpg: revocation certificate stored as 'C:/Users/John Smith/AppData/Roaming/gnupg/openpgp-revocs.d\3FB82C90A3785C870D2201F3A81B833AF353ADF4.rev'
    public and secret key created and signed.
    
    pub   rsa4096 2018-06-01 [C] [expires: 2019-06-01]
          3FB82C90A3785C870D2201F3A81B833AF353ADF4
    uid                      John Smith <john.smith@gmail.com>
    
In new version of GnuPG revocation keys are generated automatically
    
    gpg> adduid
    Real name: John Smith
    Email address: jsmith@keybase.io
    Comment:
    You selected this USER-ID:
        "John Smith <jsmith@keybase.io>"

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

    sec  rsa4096/A81B833AF353ADF4
         created: 2018-06-01  expires: 2019-06-01  usage: C
         trust: ultimate      validity: ultimate
    [ultimate] (1)  John Smith <john.smith@gmail.com>
    [ unknown] (2). John Smith <jsmith@keybase.io>

    gpg> uid 1

    sec  rsa4096/A81B833AF353ADF4
         created: 2018-06-01  expires: 2019-06-01  usage: C
         trust: ultimate      validity: ultimate
    [ultimate] (1)* John Smith <john.smith@gmail.com>
    [ unknown] (2). John Smith <jsmith@keybase.io>

    gpg> primary

    sec  rsa4096/A81B833AF353ADF4
         created: 2018-06-01  expires: 2019-06-01  usage: C
         trust: ultimate      validity: ultimate
    [ultimate] (1)* John Smith <john.smith@gmail.com>
    [ unknown] (2)  John Smith <jsmith@keybase.io>

    gpg> addphoto

    Pick an image to use for your photo ID.  The image must be a JPEG file.
    Remember that the image is stored within your public key.  If you use a
    very large picture, your key will become very large as well!
    Keeping the image close to 240x288 is a good size to use.

    Enter JPEG filename for photo ID: c:\pgp.jpg
    Is this photo correct (y/N/q)? y

    sec  rsa4096/A81B833AF353ADF4
         created: 2018-06-01  expires: 2019-06-01  usage: C
         trust: ultimate      validity: ultimate
    [ultimate] (1)* John Smith <john.smith@gmail.com>
    [ unknown] (2)  John Smith <jsmith@keybase.io>
    [ unknown] (3)  [jpeg image of size 3233]

    gpg> setpref SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
    Set preference list to:
         Cipher: AES256, AES192, AES, CAST5, 3DES
         Digest: SHA512, SHA384, SHA256, SHA224, SHA1
         Compression: ZLIB, BZIP2, ZIP, Uncompressed
         Features: MDC, Keyserver no-modify
    Really update the preferences? (y/N) y

    sec  rsa4096/A81B833AF353ADF4
         created: 2018-06-01  expires: 2019-06-01  usage: C
         trust: ultimate      validity: ultimate
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> save
    
Now generate 3 subkeys for each single action (S, E and A). Those subkeys will be transferred to YubiKeys

    gpg --expert --edit-key 3FB82C90A3785C870D2201F3A81B833AF353ADF4
    addkey
    
Select 2048 bits, one year validity

After all keys are generated, export private key and make safe backup of this key. 
Also make temporary backup of directory C:\Users\John Smith\AppData\Roaming\gnupg.  We will use this backup copy in next step

Now transfer keys to first YubiKey

    gpg --expert --edit-key 3FB82C90A3785C870D2201F3A81B833AF353ADF4

    Secret key is available.

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 1

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb* rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> keytocard
    Please select where to store the key:
    (1) Signature key
    (3) Authentication key
    Your selection? 1

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb* rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 2

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb* rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> toggle

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb* rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 3

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb* rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb* rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 1

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb* rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 3

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> keytocard
    Please select where to store the key:
    (2) Encryption key
    Your selection? 2

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb* rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 2

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb  rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> key 3

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb* rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]

    gpg> keytocard
    Please select where to store the key:
    (3) Authentication key
    Your selection? 3

    sec  rsa4096/A81B833AF353ADF4
        created: 2018-06-01  expires: 2019-06-01  usage: C
        trust: ultimate      validity: ultimate
    ssb  rsa2048/9291B7082F74C020
        created: 2018-06-01  expires: 2019-06-01  usage: S
    ssb  rsa2048/EBF0E95C17B8EA80
        created: 2018-06-01  expires: 2019-06-01  usage: E
    ssb* rsa2048/C32F6A8DB70AD601
        created: 2018-06-01  expires: 2019-06-01  usage: A
    [ultimate] (1). John Smith <john.smith@gmail.com>
    [ultimate] (2)  John Smith <jsmith@keybase.io>
    [ultimate] (3)  [jpeg image of size 3233]
