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


## Implementation in iOS
Fortunately, Rob Napier opens his work at [RNCryptor](https://github.com/rnapier/RNCryptor). That helps us save a lots time but it is better if we know a litte bit about these techniques.


#### Offline Security


#### Streaming Security



## Notice
#### Do not try to encrypt a file in separate parts
Firstly, do not try to encrypt a file in separate parts. Normally, an encrypt algorithm will save a header in cipher text, included key (gen from password), HMAC, salt and so on. Therefor, decrypt algorithm will read the header first to get these information before decrypting. If you encrypt a file by chunking this file and encrypting each part separately, cipher text will contains many headers. That will make decrypt algorithm goes fail because it cannot know where is header and where is cipher text. 

#### OpenSSL is deprecated from OSX 10.7 and not included in iOS
In addition to these APIs, a number of open source tools use OpenSSL for secure networking. If you use OpenSSL in your publicly shipping apps, you must provide your own copy of the OpenSSL libraries, preferably as part of your app bundle; the OpenSSL libraries that OS X provides are deprecated. [Refer link](https://developer.apple.com/library/mac/documentation/security/conceptual/cryptoservices/SecureNetworkCommunicationAPIs/SecureNetworkCommunicationAPIs.html).

The reason Apple raised for that is:
	OS X includes a low-level command-line interface to the OpenSSL open-source cryptography toolkit; this interface is not available on iOS.

	Further, although OpenSSL is commonly used in the open source community, **it does not provide a stable API from version to version**. For this reason, the programmatic interface to OpenSSL is deprecated in OS X and is not provided in iOS. Use of the Apple-provided OpenSSL libraries by apps is strongly discouraged.

One of Apple Engineers said in details at [here](http://rentzsch.tumblr.com/post/33696323211/wherein-i-write-apples-technote-about-openssl-on-os-x).

## Reference
- [Chapter 15, iOS 6 Programming Pushing the Limits](http://www.amazon.com/iOS-Programming-Pushing-Limits-Application/dp/1118449959)

- [PDF Protecting in iAnnotate](http://www.branchfire.com/lib-web/html/_security.html)

- [PDF Encrypting Algorithm](http://www.cs.cmu.edu/~dst/Adobe/Gallery/anon21jul01-pdf-encryption.txt) ([secondary link](https://www.dropbox.com/s/0fvg5byv9bx4lix/anon21jul01-pdf-encryption.txt))

- [Recommended Documention by Apple](https://developer.apple.com/library/ios/documentation/Security/Conceptual/Security_Overview/SeeAlso/SeeAlso.html#//apple_ref/doc/uid/TP30000976-CH7-SW1).

## Open source - Library

- [RNCryptor](https://github.com/rnapier/RNCryptor) by author of the above book
- [BBAES](https://github.com/benoitsan/BBAES)
- [NAChloride](https://github.com/gabriel/NAChloride). An Objective-C wrapper of [libsodium](https://github.com/jedisct1/libsodium) which a package of [NaCl](http://nacl.cr.yp.to/). "NaCl (pronounced "salt") is a new easy-to-use high-speed software library for network communication, encryption, decryption, signatures, etc. NaCl's goal is to provide all of the core operations needed to build higher-level cryptographic tools.".
- [Original Security Libray by Apple](http://opensource.apple.com/source/Security/Security-55471/libsecurity_ssl/).
- [Aerogear Crypto iOS](https://github.com/aerogear/aerogear-crypto-ios).




