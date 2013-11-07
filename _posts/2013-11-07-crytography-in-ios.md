---
layout: post
title: "Cryptography in iOS - How to protect your precious data?"
description: "(Intro here, complete after)"
category: cryptography
tags: [cryp tography]
---
{% include JB/setup %}
(Intro here, complete after)

## Basic terminology
This section is for someone who is not familiar with cryptography. Some basic terms will be unleashed here.

#### Symmetric and Asymmetric cipher


#### Key and Salt
Key is not a password! If we have a password, we must convert it into a key. This key is for encrypting and decrypting document. Needless to say, the key is not public as the password. For making a key, we need a salt described as follows.

Salt is usually a large, random number for generating a key from a password. Because password is usually short and easy to remember, we should make a key which is long and hard to read which prevents brute-force attack. To do this task, a salt is needed. It will be combined with password, the result is a key.

In fact, salt is public, not need to be secret.



#### HMAC (Hash-based Message Authentication Code)


#### Initialization Vector (IV)


#### Padding
Cipher-block is must be multiple of block size. Sometimes the last block may be smaller than block size. Therefore, we must add pseudo-data to the end of plaintext. This block size problem is dealt with by padding.

Padding is the practice of expanding plaintext to the correct length prior to encryption. It needs to be done in a way that the padding can be unambiguously removed after decryption. There are several ways to achieve this, but the only way supported by iOS is called PKCS #7 padding.

#### Cipher-block chaining




## Reference
- [Chapter 15, iOS 6 Programming Pushing the Limits](http://www.amazon.com/iOS-Programming-Pushing-Limits-Application/dp/1118449959)

- [RNCryptor](https://github.com/rnapier/RNCryptor) by author of the above book


