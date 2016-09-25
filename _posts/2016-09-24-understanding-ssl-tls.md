---
layout: post
title: "Understanding SSL/TLS"
date: 2016-09-24 12:40:41 -0530
categories: security
---

Secure Socket Layer, a lot of developer has misconception about it. What is it? Why it is used? I will try to cover some detail about it. Also what is SSL authentication or 2 way SSL? Will also try to cover some details about certificate and certificate chain.

### What is SSL? And why it is used?

SSL, which stands for `Secure Socket Layer`, is a protocol to communicate over network with a secure channel. It adds a layer of security over the simple socket communication. It is built directly on top of TCP protocol (although there are some implementation which works on UDP). It gives a layer of security to the application level protocols such as HTTP. With the fact that SSL works on top of TCP, HTTP becomes virtually equals to HTTPS.

### What is Encryption/Decryption?

To understand how SSL/TLS works, you need to know a little about encryption or encryption algorithms. 

Encryption is a data transformation logic where data is transformed into some other data with the help of some key, in such a way that the data can be recovered to its original state by another transformation logic (decryption) with the help of a key. Now if the data is `d`, encryption is `f1`, decryption is `f2`, encryption key is `k1`, decryption key is `k2` then the statement below is true, and only true for `k1` and `k2`.

```
d = f2( f1( d, k1 ), k2)
# and also
d = f2( f1( d, k2 ), k1)
```

> If k1 or k2 slightly changed then the above equation does not stand true.

#### Symmetric Encryption

Now here if the `k1` and `k2` is equal, then this encryption/decryption logic is called a **symmetric** algorithm, and if not then it is called a **asymmetric** algorithm.

#### Asymmetric Encryption

Lets think about what is required to use a symmetric algorithm. In symmetric algorithm `k1 = k2 = k`. So in a communication, when server sends some data to client encrypting the data with `k`, then to understand the data client also require to use `k`. Which means server need to send `k` to client unencrypted before they can engage into a secure communication. But sending the **key** over the network, even for once, un encrypted, makes the communication easy to be disclosed.

Now for asymmetric algorithm, `k1 != k2`. So server can share one key with client and one key can be kept private. After this key exchange server can send some data to client after encrypting it with server's key and client can decrypt that with the key shared - simple; but will cover [detail later](#how-ssltls-works). Before that lets be clear on some terminology and fact.

* `k1` and `k2` are generated together and generated in such a way that they make the above equation true, and only between themselves.
* One key among them is called **Private Key**, and the another is called **Public Key**.
* Private Key is designed to be kept secret.
* Public Key is meant for sharing.

#### Example of algorithms are

* Symmetric: **AES**, **DES**
* Asymmetric: **RSA**, **DSA**

### What is Digital Signature and Digital Certificate?

Signature is a special type of cypher where a data is uniquely represented by a smaller data, with the help of a key. This newly generated data can not recover the original data, but it is possible to verify the integrity of the data with the help of Signature and a key. Now if data is `d`, signing key is `sk`, verifying key is `vk` and signature is `s`, then these can be represented as below.

```
# create signature
s = sign( d, sk)
# verify integrity
verify( d, vk, s) is true
```

### How SSL/TLS Works

Consider few of the problems, someone sitting in the middle and create a proxy of trust with each party and watch the communicated data, and many others. SSL and its successor TLS solves this issue by a process called handshake. Handshake cak be broken into three further steps.

* Negotiate Basic Standards

   After building a TCP connection, client starts the SSL handshake. Here the client can be a browser, putty or any application which is trying to access a SSL enables endpoint. In this process client sends
   
   1. Supported **SSL/TLS version**
   2. Supported **cypher suites**
   3. Supported **compression methods**
   
   Server checks and matches with himself, what is the highest SSL/TLS version supported by both, a cypher suite supported by both and a compression method. And both agrees upon that.
   
* Establish Trust

   After the basic negotiation is done, server sends its `Certificate Chain` to client for verification. Clients checks the certificate with its own database if the certificate itself is found or the signer is found to be trusted. And the certificate matches with the server's name the client is talking to. Basically this authenticity check can be split into two part.
   
   1. Certificate Trust Check; checks if the certificate is trusted, or the signer of the certificate is trusted.
   2. Hostname Verification; checks with the hostname the client is connecting to withe the properties in certificate.
   
* Exchange Symmetric Key

   After client is sure about the trust for server, it now generates a symmetric key for the algorithm already been agreed upon in [negotiation](#negotiate-basic-standards) phase. Then client encrypts the key with the public key came from the server along with the certificate and sends that to the server. As it is encrypted with public key, it can only be decrypted with the private key available with server. Now the SSL handshake is done, and both parties can now use the symmetric key for encryption and decryption and enjoy a secure transport session.
   
After this handshake is over, normal HTTP protocol messages can be exchanged. This connection/session is normally kept open longer for repetitive use. At the nd of use client or server can initiate a connection close message and both can end the connection gracefully. After all these done properly an attacker in the middle can see every bit of information, but thanks to the encryption, he can not understand anything about it. Typically a attacker can understand

1. How much data is being transferred
2. The IP/PORT the client is connecting to
3. The hostname the client is connecting to (because of DNS query)
4. The cyphers which are being used by both parties
5. Attacker can end the connection forcefully. but in that case both party will be notified about that