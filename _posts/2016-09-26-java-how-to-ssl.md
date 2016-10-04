---
layout: post
title: "Java How To SSL"
date: 2016-09-26 19:19:41 +0530
categories: security
---

This is the continuation of my previous post [**Understanding SSL/TLS**]({% link _posts/2016-09-24-understanding-ssl-tls.md %}). With this post I try to give an idea about how SSL/TLS connection should be handled from a java project, including both server side and client. What we should do and what we should not. Consider this post as a personal opinion when it comes to "Recommendation" from me.

> Nothing is better than the line of code you didn't have to write

## Yes That's True

Yes it is true, that you don't need to write any special code to connect to a HTTPS endpoint other than what you do for a HTTP endpoint. **Wow then why so many developer struggle here?** And why this blog? Yes there are still something we will discuss here.

## When Nothing is Required

Normally when some website uses valid certificate, then as a java client you can access the site endpoint as you would for a normal HTTP site. Only exception if it need a SSL authentication or 2-Way SSL. This is the good part, but the goodness just ends here. Although all internet service providers uses valid certificate, but when it comes to practical use, or the development environment, almost all the time certificates are self signed. When it comes to a intranet of a enterprise, they usually have their own certification authority setup. But java/jre is un-aware of that. I will explain some of the option to handle these situations later, but for now lets concentrate when is a certificate is **considered to be valid**.

## Properties of a Valid Certificate

There are few things which determines whether the Certificate is valid or not. We can categorize these in three broad category. All proper Certificate validation should follow these. Below set of are not everything, there may be many more. Even for other security purposes.

> I would recommend a nice graphical application named [Keystore Explorer](http://www.keystore-explorer.org/) to understand more about the certificate, Key, Sign etc.

### Security algorithm, strength and validity

In this category, checks are as follows

1. Whether it uses an encryption with secure enough algorithm, and key length as per the modern standard.
   
   As an example, `SHA1WithRSA` is no more considered to be usable after year 2016.
2. Same way validation to be done for all certificates in the certificate chain.

### Signature validity, trust and Certificate Revocation List

Checks for this category are as follows

1. All the signature in all certificates is verifiable
2. Validate whether the top certificate/root certificate is trusted as per the database present with the validator
3. Check with global/local certificate revocation list, whether none of the certificate is revoked

### Certificate restrictions and applicability

Checks for this category are as per the certificate extension attribute specification. Some of the well known extensions should be checked as below.

1. Basic Constraint; if the certificate is a CA, if true then it should not be used for SSL.
   
   ![Basic Constraint]({{ site.url }}/images/{{ page.title | slugify }}/certificate_basic_constraint.PNG)

2. Key Usage; checks whether the key can be used for the inted purpose like Digital Signature, Encipher or any other.
   
   ![Key Usage]({{ site.url }}/images/{{ page.title | slugify }}/certificate_key_usage.PNG)

3. Extended Key Usage; checks if the certificate should be used for TLS server/TLS client. Or any other purpose like Code Sign or smart card login.

   ![Extended Key Usage]({{ site.url }}/images/{{ page.title | slugify }}/certificate_extended_key_usage.PNG)

4. Subject Alternative Name; checks if the certificate is really for the use of the hostname the client is connecting to.

   ![Subject Alternative Name]({{ site.url }}/images/{{ page.title | slugify }}/certificate_subject_alternate_name.PNG)

## How to handle Self Signed Certificate

The situation for trusting self signed certificate can be like this.

The project is in development environment and in this environment all certificates self signed, but when it goes to production, certificates are properly signed by Certificate Authority. In this situation there is three way to handle it, and again coding for it should be the last option.

1. Option 1 - Import the certificate to java global trust

   You import the self signed certificate into your development time machine's jre. This is usually available at `%JAVA_HOME\jre\lib\security\cacerts`. You can use java provided CLI tool `keytool` or use the GUI  mentioned above. Advantage of it: easy, doesnt need anything to be done in code or startup time. Disadvantage: maybe a security risk depending on how you use the JRE, you may not have access to this file.
    
2. Option 2 - Provide a truststore JVM parameter

   You can create a keystore file and import all the certificate you want to trust, and provide that during startup of your program in dev environment. The parameters are `-Djavax.net.ssl.trust*` available as standard JVM argument. 