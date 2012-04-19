---
layout: page
title: "Browser interaction with the OpenXPKI server"
---
{% include JB/setup %}


<p>
A single OpenXPKI server running on a Unix host can serve several CAs, RAs and public interfaces depending on the topology of the PKI as resulted after the deployment procedure. 
The normal way to access OpenXPKI server is via the web server running on the same Unix host.
This access is done with a web browser from the local Unix host or (if allowed) from any host on the intranet or internet.
CA operator, RA operator, regular users: everybody can access OpenXPKI server this way.
</p><p>
Different browsers have different understanding and support of cryptography. 
That is why different browsers provide different cryptography related functionality when accessing the OpenXPKI server.
</p>

<h2>All browsers can do</h2>
<p>Provided cookies are supported and enabled:
</p>

<dl>
<dt>Generation of a private key on the server side </dt>
  <dd>This is a part of procedure to generate a Certificate Signing Request (CSR).
This could be performed by a regular user or by RA operator.
It is implemented by built-in tools of the crypto-backend which is installed on the server side.
The user can choose from a list of crypto-algorithms and their parameters available at the server side.
  </dd>
<dt>Upload to the OpenXPKI server of a preliminary prepared CSR</dt>
  <dd>Could be performed by a regular user or by RA operator.
CSRs to be uploaded should be rendered to PKCS#10 format by external means.
  </dd>
</dl>


<h2>MS Internet Explorer version 6+ specific features</h2>
<p>These features are implemented with scripts provided by the OpenXPKI project
and with regular tools built into Windows. 
No permissions and no digital signatures from MS are needed to make use of these features.
</p>

<dl>
<dt>Generation of a private key on the client side </dt>
  <dd>This is a part of the procedure to generate a Certificate Signing Request (CSR). 
It could be performed by a regular user or by a RA operator. 
Private key generation is implemented with a Visual Basic Script 
and uses the standard MS XEnroll ActiveX control. 
This has not yet been tested under MS Windows Vista.
If several crypto service providers (CSPs) are installed at the client's host, 
a list of CSP's names is shown on the screen along with
a list of parameters for supported crypto-algorithm. 
It is assumed that one CSP provides only one type of crypto-algorithm. 
E.g. only RSA, only DSA, only GOST, or only GOST-on-eliptic-curves.
A CSP can provide an interface to a hardware cryptographic device such as a smart card or USB token. 
In this case hardware is fully masqueraded by this CSP from the OpenXPKI related scripts.
  </dd>
<dt>Smart card personalization procedure</dt>
  <dd>Is meant for initial massive issuing of smartcards based on data from LDAP directory of personnel department.
It offers the possibility to automatically create a key and a corresponding certificate
request for the CA. The CA then signs the request and within a few seconds, the certificate is
returned to the user and is automatically installed on the user's smartcard. 
  </dd>
<dt>Signing to approve a CSR</dt>
  <dd>Could be performed by RA operator on the client side. 
Is implemented with a Visual Basic Script, which uses the CAPICOM ActiveX component.
Optionally cryptographic cards and tokens could be used.
  </dd>
<dt>Download and install certificate to the Windows certificate store</dt>
  <dd>Is implemented with a Visual Basic Script.
The windows certificate store can be used by arbitrary software (including IE) installed at the client side.
  </dd>
<dt>Automatic browser detection</dt>
  <dd>on server side: works reliably.
  </dd>
<dt>https</dt>
  <dd>is supported.
  </dd>
<dt>UTF-8</dt>
  <dd>is supported in web interface and inside PKI data.
  </dd>
<dt>Certificate-based logon</dt>
  <dd>Certificate-based logon to the OpenXPKI server is supported. 
    CAPICOM ActiveX component is used for this.
Optionally cryptographic cards and tokens could be used.
  </dd>
</dl>

<h2>Mozilla/Firefox/Netscape specific features</h2>
<p>
</p>

<dl>
<dt>Generation of a private key on the client side </dt>
  <dd>This is a part of procedure to generate a Certificate Signing Request (CSR).
Could be performed by a regular user or by RA operator.
Private key generation is implemented using the proprietary &lt;keygen&gt; tag, 
the public key is uploaded to the server in SPKAC format.
Optionally cryptographic cards and tokens could be used.
Personal data of the certificate requestor goes to the server separately.
Server collects data and public key thus effectively forming
an equivalent of a CSR in the server database.
  </dd>
<dt>Signing to approve a CSR</dt>
  <dd>Could be performed by RA operator on the client side. Is implemented with a JavaScript.
Optionally cryptographic cards and tokens could be used.
  </dd>
<dt>Download and install certificate to the browser</dt>
  <dd>The 
download of the certificate triggers subsequent installation of this certificate
to the browser store. This is implemented by sending the certificate with Mozilla-specific MIME-types.
  </dd>
<dt>Automatic browser detection</dt>
  <dd>on server side: works reliably.
  </dd>
<dt>https</dt>
  <dd>is supported.
  </dd>
<dt>UTF-8</dt>
  <dd>is supported in web interface and inside PKI data.
  </dd>
<dt>Certificate-based logon</dt>
  <dd>Certificate-based logon to the OpenXPKI server is supported, 
using built-in functionality of the browser.
Optionally cryptographic cards and tokens could be used.
  </dd>
</dl>

<h2>Opera</h2>                                                                                           
<p>Tries to be a browser which is better that IE and Mozilla.
In reality imitates partly IE, partly Mozilla.
Starting with version 9, Opera is changing inter-platform portability concept,
actually sacrificing it.
SPKAC is claimed to work, but seems like it does not.
Not recommended for use with the OpenXPKI. Not fully tested yet.
</p>
   
<dl>
<dt>Automatic browser detection</dt>
  <dd>on server side: works sporadically and heavily depends on user preferences chosen in the browser.
  </dd>
<dt>https</dt>
  <dd>is supported.
  </dd>
<dt>UTF-8</dt>
  <dd>is supported in web interface and inside PKI data.
  </dd>
</dl>

<h2>Konqueror</h2>                                                                                           
<p>SPKAC should work. Not fully tested yet.
</p>
   
<h2>Lynx</h2>
<p>Not fully tested yet.
</p>
  
<h2>Links</h2>
<p>Not fully tested yet.
</p>
  
<hr/>


