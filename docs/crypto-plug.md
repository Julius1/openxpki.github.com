---
layout: page
title: "Cryptography abstraction concept"
---
{% include JB/setup %}


<h3>by Sergei Vyshenski</h3>
<p>
OpenXPKI as a software product is a collection of building blocks and construction
tools meant for deployment of an arbitrary complex and fully functional 
Public Key Infrastructure (PKI) as specified by the X.509 standard. 
Of course
full functionality is a goal to be reached in versions to come.
</p><p>
From the system point of view, PKI is a special purpose information system for processing, 
storing and propagation of a certain class of data units: requests, certificates and
revocation stuff. At certain stages of the data processing, PKI deals with cryptographic 
calculations and transformations. But the general structure and workflow of data processing
is independent of the employed cryptography.
</p><p>
For the first time in the open source world, OpenXPKI is offering a trustcenter concept
which is totally decoupled from the cryptographic details.
</p><p>
Technically, this is achieved by 
</p>

<ol>
<li>
providing a <em>cryptographic layer</em> (see
<a href="OpenXPKI-Architecture-Overview.pdf">Architecture White Paper</a>). It forms a
universal cryptographic interface, to which arbitrary (software or hardware) cryptographic backends
can be plugged in. 
</li><li>
introducing a cryptographic talking policy: all constituents of PKI in their OpenXPKI realization 
(ones that are higher than the cryptographic layer)
are only allowed
to speak to cryptographic tools in a specially designed 
<em>high level language for cryptographic needs of PKI</em>. <!-- API != language ... -->
</li>
</ol>

<h2>High level language for cryptographic needs of PKI at server side</h2>
<p>
Our analysis shows that all cryptographic needs of PKI at the server side can be 
expressed by just the following 19 commands. 
</p>

<dl>
<dt>convert_cert</dt>
  <dd>
        if given a certificate, transform it from PEM into DER or TXT<br/>
        if given more than one certificates, transform them into a PKCS#7 structure in PEM, 
                DER or TXT
  </dd>
<dt>convert_crl</dt>
        <dd>transform CRL from PEM into DER or TXT</dd>
<dt>convert_key</dt> 
        <dd>transform a key from RSA, DSA or PKCS#8 into PEM, DER or PKCS#8</dd> 
<dt>convert_pkcs10</dt>
        <dd>transform a request from PEM into DER or TXT</dd>
<dt>create_cert</dt>
        <dd>create self-signed certificate</dd>
<dt>create_key</dt>
        <dd>create a secret key</dd>
<dt>create_pkcs10</dt>
        <dd>create a certificate request</dd>
<dt>create_pkcs12</dt>
        <dd>store a secret key and related chain of certificates into a PKCS#12 structure</dd>
<dt>create_random</dt>
        <dd>generate a random number</dd>
<dt>is_prime</dt>
        <dd>check if a number is prime</dd>
<dt>issue_cert</dt>
        <dd>issue a certificate which is signed by a Certificate Authority (CA)</dd>
<dt>issue_crl</dt>
        <dd>issue a Certificate Revocation List (CRL)</dd>
<dt>list_algorithms</dt>
        <dd>return list of supported public key algorithms and their parameters</dd>
<dt>pkcs7_decrypt</dt>
        <dd>decrypt data</dd>
<dt>pkcs7_encrypt</dt>
        <dd>encrypt data</dd>
<dt>pkcs7_get_chain</dt>
        <dd>check a chain of certificates, used for signature, and/or encryption/decryption</dd>
<dt>pkcs7_sign</dt>
        <dd>sign data</dd>
<dt>pkcs7_verify</dt>
        <dd>verify signature for data</dd>
<dt>symmetric_cipher</dt>
        <dd>encrypt/decrypt data with one of the algorithms for symmetric encryption</dd> 
</dl>

<p>
Please note that only one of these commands, 
namely <em>create_key</em>, obligatoryly requires apriori knowledge of the fact that 
a certain cryptographic 
set of tools exists with a specific name (like RSA, DSA, GOST, etc).
This apriori knowledge can be acquired with another command: <em>list_algorithms</em>.
In interactive scripts you can ask your system to list algorithms and show the list to the
user. So that the user can pick up the preferred algorithm with a pointing device.
To facilitate automatic processing of this list, severall formats of list presentation is provided.
As a result, the high-level programmer needs no knowledge about which cryptographic backend and 
which cryptographic algorithms will be available in a really installed system.
</p>
<p>
All other commands could 
deduce necessary cryptographic details from the data which they process.
</p>
<hr/>

<h3>N.B.:</h3>

<blockquote>

<dl>
<dt>PKCS#7</dt> 
    <dd>
Cryptographic Message Syntax Standard. 
See RFC 2315. Used to sign and/or encrypt messages under a PKI. 
Used also for certificate dissemination (for instance as a response to a PKCS#10 message).
Structure contains data and (possibly) a signature, which signs the data.
</dd>
<dt>PKCS#8</dt> 
    <dd>Private-Key Information Syntax Standard. 
            Structure contains encrypted (or not encrypted) secret key.</dd>
<dt>PKCS#10</dt> 
   <dd>Certification Request Standard.
See RFC 2986. Format of messages sent to a Certification Authority to
request certification of a public key. Structure contains a request.</dd>
<dt>PKCS#12</dt> 
   <dd>Personal Information Exchange Syntax Standard.
Defines a file format commonly used to store private keys with accompanying 
chain of public key certificates protected with a password-based symmetric key.</dd>
</dl>

</blockquote>

<hr/>

<h2>A list of cryptographic backends recognized by OpenXPKI</h2>
<p>
Today this list consists of the following entries: 
</p>

<ol>
<li><a href="http://www.openssl.org/">OpenSSL</a> cryptographic library 
ver. 0.9.8x with x = a, b, c, d, e.</li>
<li><a href="gost-howto.html">OpenSSL-0.9.8d-gost</a> a clone of OpenSSL cryptographic library
with additional support for russian national (GOST) cryptographic algorithms.</li>
</ol>

<p>
Our <em>Cryptography abstraction concept</em> still pays a lot even with that modest
choice:
</p>

<ul>
<li>If one day OpenSSL acquires a new cryptographic algorithm, then it will take a minute 
to add support of it to the OpenXPKI.
</li><li>
If one day OpenSSL acquires a support for new hardware security module, 
then it will take a minute to add support of it to the OpenXPKI.
</li><li>
If one day we get an alternative to OpenSSL, that is some totally different (software or hardware) 
cryptographic product, then we have a ready template to fill in to add support 
of this product to the OpenXPKI.
</li>
</ul>

<p>
With cryptographic backend #2 plugged in, OpenXPKI
supports both sets of cryptography:
</p>

<ol>
<li>the same set of algorithms as with cryptographic backend #1, and plus to that:
</li>
<li>russian national cryptographic algorithms GOST.
See <a href="gost-howto.html">Working with russian (GOST) cryptography</a>.
</li>
</ol>

<p>
This makes OpenXPKI the very first example of an open source trustcenter product 
which supports two
absolutely different sets of cryptography (western and russian) at the same time.
So that RSA, DSA, and GOST certificates can coexist in the same 
working database and move around the same PKI.
</p><p>
Future version OpenSSL-0.9.9 is scheduled to link together both cryptographic 
backends listed above into a single product. It will provide
simultaneous and default support of arbitrary cryptographic sets.
And OpenXPKI is ready to support this new version of OpenSSL due to the same 
<em>Cryptography abstraction concept</em>.
</p>

