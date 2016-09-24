---
layout: post
title: "Basic Understanding About SSL or TLS"
date: 2016-09-24 12:40:41 -0530
categories: security ssl tls
---

Secure Socket Layer, a lot of developer has misconception about it. What is it? Why it is used? I will try to cover some detail about it. Also what is SSL authentication or 2 way SSL? Will also try to cover some details about certificate and certificate chain.

### What is SSL? And why it is used?

SSL, which stands for `Secure Socket Layer`, is a protocol to communicate over network with a secure channel. It adds a layer of security over the simple socket communication. It is done with a asymmetric encryption algorithm. It also servers a purpose of identifying the the server.

Well obviously it gives security to the communication. The data which is transferred from one end to the other is encrypted at one end and decrypted at the other end so any `man in middle` attack is prevented, no one else can see the data what is being transferred.

### How it works

To understand how it works, you need to know a little about encryption or asymmetric encryption algorithm. 

#### Summary of Encryption and Decryption

Encryption is a data transformation logic where data is transformed into some other data with the help of some key, in such a way that the data can be recovered to its original state by another transformation logic (decryption) with the help of a key. Now if the data is `d`, encryption is `f1`, decryption is `f2`, encryption key is `k1`, decryption key is `k2` then the statement below is true, and only true for `k1` and `k2`.

```
d = f2( f1( d, k1 ), k2)
# and also
d = f2( f1( d, k2 ), k1)
```

> If k1 or k2 slightly changed then the above equation does not stand true

Now here if the `k1` and `k2` is equal, then this encryption/decryption logic is called a **symmetric** algorithm, and if not then it is called a **asymmetric** algorithm.

Example of algorithms are

* Symmetric: **AES**, **DES**
* Asymmetric: **RSA**, **DSA**

#### Why Asymmetric Encryption

Lets think about what is required to use a symmetric algorithm. In symmetric algorithm `k1 = k2 = k`. So in a communication, when server sends some data to client encrypting the data with `k`, then to understand the data client also require to use `k`. Which means server need to send `k` to client unencrypted before they can engage into a secure communication. But sending the **key** over the network, even for once, un encrypted, makes the communication easy to be disclosed.

Now for asymmetric algorithm, `k1 != k2`