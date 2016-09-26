---
layout: post
title: "Java How To SSL"
categories: security
---

This is the continuation of my previous post **Understanding SSL/TLS**({% link _posts/2016-09-24-understanding-ssl-tls.md %}). With this post I try to give an idea about how SSL/TLS connection should be handled from a java project, including both server side and client. What we should do and what we should not. Consider this post as a personal opinion when it comes to "Recommendation" from me.

> Nothing is better than the line of code you didn't have to write

## Yes That's True

Yes it is true, that you don't need to write any special code to connect to a SSL enabled HTTP endpoint, along with neumerous