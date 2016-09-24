---
layout: post
title: "2 Way SSL Everything About It"
date: 2016-09-24 12:40:41 -0530
categories: java ssl security
---

Secure Socket Layer, a lot of developer has misconception about it. What is it? Why it is used? I will try to cover 
some detail about it. Also what is SSL authentication or 2 way SSL? Will also try to cover some details about certificate 
and certificate chain.

### What is SSL? And why it is used?

SSL, which stands for `Secure Socket Layer`, is a protocol to communicate over network with a secure channel. It adds a 
layer of security over the simple socket communication. It is done with a asymmetric encryption algorithm.

Well obviously it gives security to the communication. The data which is transferred from one end to the other is encrypted at 
one end and decrypted at the other end so any `man in middle` attack is prevented, no one else can see the data what is being 
transferred.

### How it works

To understand how it works, you need to know a little about encryption or asymmetric encryption algorithm. 

#### Summary of Encryption and Decryption

Encryption is a data transformation logic where data is transformed into some other data with the help of some key, in such a way that the data can be 
recovered to its original state by another transformation logic (decryption) with the help of a key. Now if the data is `d`, 
encryption is `f1`, decryption is `f2`, encryption key is `ek`, decryption key is `dk` then the statement below is true.

```
d = f2( f1( d, ek ), dk)
```

Now here if the `ek` and `dk` is equal, then this encryption/decryption logic is called a **symmetric** algorithm, and if not 
then it is called a **asymmetric** algorithm.

Example of algorithms are

* Symmetric
   **AES**, **DES**
* Asymmetric
   **RSA**, **DSA**

#### Why asymmetric Encryption