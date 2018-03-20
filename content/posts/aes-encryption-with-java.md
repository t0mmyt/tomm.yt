---
title: "AES encryption with Java"
date: 2018-03-20T19:41:31Z
draft: false
tags: ["java", "encryption"]
---
The following code is for encrypting short messages (it acts on byte arrays and not streams so the whole message needs to fit in memory). It creates a random Initialisation Vector (IV) for each message which is prepended to the start of the encrypted stream.
<!--more-->

It is safe to leave your IV in the open so long as your key is kept secure (think salt on a password hash).  This means that the same key can be used throughout without the same data being encrypted in the same way twice.  This does add 16 bytes to the length of each message so if your throughput is high volume but small messages, that could be a factor.  This implementation also uses GCM for message authentication though so that is an additional 16 bytes again.  This means your encrypted message will be 32 bytes (or 2x 128 bits) longer than the input.  The string used below in the test is 152 bytes originally but 184 after GCM and the IV payload.

**Disclaimer:**  I am not a cryptography guru nor do I mean to imply that the following code is suitable for use without more than a passing understanding of the concepts therein.

Firstly the test.  This test takes a string and encrypts it twice with the same key (the class *AesUniqueIv* will generate a key if none is provided which is made available by the *getKey()* method).  It should be fairly obvious but the assertions are that the same message can be encrypted twice and decrypted twice and that the two encrypted messages are *not* the same.  It also asserts that the length of the encrypted message is indeed 32 bytes longer as stated earlier.

{{< gist t0mmyt ca61d34d13251f79d48e0826ee381348 "AesUniqueIvTest.java" >}}

Now the actual code, firstly the keysize and cipher used require either the use of OpenJDK or the Java Cryptography Extensions installed and [BouncyCastle](https://www.bouncycastle.org/java.html) library available.  If you don't have OpenJDK (or the JCE available) then you are limited to 128 bit encryption.  BouncyCastle provides the GCM authentication.  If the constructor is called with no arguments, then it uses the built in key generator to create a key of the correct length.  Otherwise it expects a byte array of the encoded key.  On each call to *encrypt*, a the random IV is generated and copied to the start of the array.  Equivilently, on *decrypt*, the IV is stripped from the start of the array.  Evrything else should be fairly self explanitory.

{{< gist t0mmyt ca61d34d13251f79d48e0826ee381348 "AesUniqueIv.java" >}}

**Disclaimer once again:**  I am not a cryptography guru nor do I mean to imply that the following code is suitable for use without more than a passing understanding of the concepts therein.  This code is [public domain](https://unlicense.org/).
