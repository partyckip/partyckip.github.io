---
layout: post
title: "YubiKey PGP setup"
date: 2018-05-31
---

How to configure 2 YubiKeys with the same PGP

1. Install the latest version of Gpg4win (here 3.1.1 version was used, which contains GnuPG version 2.2.7)

2. Let's check if installation worked, start command line
<code>
set LANG=C
gpg --version
</code>

3. Check if you do not have any keys on your YubiKeys (both)
gpg --card-status

Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

4. Create your offline master key (if you do not have it yet)
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
