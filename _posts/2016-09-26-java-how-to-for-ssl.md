---
layout: post
title: "Java How To SSL"
date: 2016-09-26 19:19:41 +0530
categories: security
---

This is the continuation of my previous post [**Understanding SSL/TLS**]({% link _posts/2016-09-24-understanding-ssl-tls.md %}). With this post I try to give an idea about how SSL/TLS connection should be handled from a java project, including both server side and client. What we should do and what we should not. Consider this post as a personal opinion when it comes to "Recommendation" from me.

> Nothing is better than the line of code you didn't have to write

## Yes That's True

Yes it is true, that you don't need to write any special code to connect to a SSL enabled HTTP endpoint. Along with numerous other examples, this is also true that you don't have to write anything extra to connect to a HTTPS site. **Wow then why so many developer struggle here?** And why this blog? Yes there are still something we will discuss here.

## When Nothing is Required

Normally when some website uses valid certificate, then as a java client can access the site endpoint as it would for a normal HTTP site. And also if it doesn't need a SSL authentication or 2-Way SSL. This is the good part, but the goodness just ends here. Although all internet service providers uses valid certificate, but when it comes to practical use, or the development environment, almost all the time certificates are self signed. When it comes to a intranet of a enterprise, they usually have their own certification authority setup. But java/jre is un aware of that. I will explain some of the option to handle these situations later but for now lets concentrate, When is a certificate is **considered to be valid**.

## Properties of a Valid Certificate

