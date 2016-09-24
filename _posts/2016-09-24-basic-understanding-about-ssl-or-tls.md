---
layout: post
title: "Basic Understanding About SSL or TLS"
date: 2016-09-24 12:40:41 -0530
categories: security ssl tls
---

Secure Socket Layer, a lot of developer has misconception about it. What is it? Why it is used? I will try to cover some detail about it. Also what is SSL authentication or 2 way SSL? Will also try to cover some details about certificate and certificate chain.

### What is SSL? And why it is used?

SSL, which stands for `Secure Socket Layer`, is a protocol to communicate over network with a secure channel. It adds a layer of security over the simple socket communication. It is built directly on top of TCP protocol (although there are some implementation which works on UDP). It gives a layer of security to the application level protocols such as HTTP. With the fact that SSL works on top of TCP, HTTP becomes virtually equals to HTTPS.

### What is Encryption/Decryption

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

Now for asymmetric algorithm, `k1 != k2`. So server can share one key with client and one key can be kept private. After this key exchange server can send some data to client after encrypting it with server's key and client can decrypt that with the key shared - simple; but will cover [detail later](#high-level-details). Before that lets be clear on some terminology and fact.

* `k1` and `k2` are generated together and generated in such a way that they make the above equation true, and only between themselves.
* One key among them is called **Private Key**, and the another is called **Public Key**.
* Private Key is designed to be kept secret.
* Public Key is meant for sharing.

#### Example of algorithms are

* Symmetric: **AES**, **DES**
* Asymmetric: **RSA**, **DSA**

### How SSL/TLS Works

Yes actually for a secure communication this procedure is not so easy. Consider few of the problems, someone sitting in the middle and create a proxy of trust with each party and watch the communicated data, and many others. SSL and its successor TLS solves this issue.

* After building a TCP connection