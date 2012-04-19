---
layout: page
title: "Working with russian (GOST) cryptography"
---
{% include JB/setup %}


<h3>An example of using OpenXPKI with alternative cryptography</h3>
<h3>by Sergei Vyshenski</h3>
<p>
Since late summer of 2006 a production branch of OpenSSL version 0.9.9 has a built-in support 
for arbitrary asymmetric cryptography, and provides an extended set 
of russian national algorithms (GOST) as an example of <em>foreign</em>
cryptography.
Today this OpenSSL-0.9.9 branch is far from the stable state. As soon as it 
stabilizes, the OpenXPKI project will surely support it. That is both 
classical RSA - DSA cryptography, and GOST cryptography will be supported 
simultaneously and by default and out of the box.
</p>
<p>
For those who can not wait, provided here is a patched version of 
OpenSSL-0.9.8d equipped with the (same as in ver.0.9.9) set of GOST algorithms. 
This collection is called OpenSSL-0.9.8d-gost. It has  
full and simultaneous support of both western 
and russian cryptography. The recipes that follow have been tested to work with OpenXPKI
at FreeBSD@Intel-32, FreeBSD@AMD-64, Debian@Intel-32 platforms. <em>Tested to work</em>
here means that all built-in tests of the OpenXPKI pass ok, and that no
cryptographic-backend-related errors were found while 
working with the OpenXPKI via web interface. 
</p>
<p>
Roughly today's procedure to prepare support for GOST in OpenXPKI is as follows:
</p>
<ul>
      <li>unzip a regular 
<a href="http://www.openssl.org/source/openssl-0.9.8d.tar.gz">OpenSSL-0.9.8d version</a>
</li>
      <li>apply
<a href="http://prdownloads.sourceforge.net/openxpki/openssl-asymm-0.9.8d-20061110.diff.gz">
a patch</a>
which enables support of arbitrary asymmetric cryptography,</li>
      <li>install thus patched OpenSSL version to a specially dedicated directory 
           OPENSSL_INSTALLDIR, so that your system's OpenSSL installation is not violated,</li>
      <li>configure 
<a href="http://prdownloads.sourceforge.net/openxpki/engine-gost-20061110.tar.gz">
engine-gost</a> (which is a set of GOST algoritms)
to work with this OpenSSL and install it,</li>
    </ul>
<p>
The details of the above procedure are given in 
<a href="openssl-gost.sh">
a shell script</a>. 
For simplicity it knows internet references for only one of many 
SourceForge mirrors to get tarballs from. To run this script you need to install <tt>wget</tt>.
You may have to edit first lines of the script to match your system and preferences:
</p>
<ul>
      <li><tt>OPENSSL_SOURCEDIR</tt> points to the directory to which tarballs will be unzipped.</li>
      <li><tt>OPENSSL_INSTALLDIR</tt> points to the directory to which OpenSSL-gost software collection 
          will be installed. Beware: this directrory will be cleaned before installing.</li>
      <li><tt>MAKE</tt> points to your gmake (on Linux it could be named just make).</li>
    </ul>

<p>
If the script fails at a download stage, you can help it by downloading 3 needed tarballs 
manually from the references above, and placing all of them into <tt>OPENSSL_SOURCEDIR</tt>. 
After that re-run the script.
<p>
If successful, the above procedure adds support for the following cryptographic 
algorithms (named here as recognized by the OpenSSL library):
</p>
    <ul>
      <li>md_gost94 message digest algorithm.</li>
      <li>gost89 symmetric encryption algorithm with 256 bit key.</li>
      <li>gost94 public key algorithm with 1024 bit public key.</li>
      <li>gost94cp public key algorithm with 1024 bit public key (CP mode<sup>1</sup>).</li>
      <li>gost2001 public key algorithm based on elliptic curves
                    with 512 bit public key.</li>
      <li>gost2001cp public key algorithm based on elliptic 
                                        curves with 512 bit public key (CP mode<sup>1</sup>).</li>
    </ul>
<p>
To test GOST support at the library level try:
</p>
<pre>    ${OPENSSL_INSTALLDIR}/bin/openssl engine gost -t -c -vvvv
</pre>
</p>
<p>
    You should see something similar to the following:
</p>
<pre>
    (gost) GOST engine
     [gost89, md_gost94, gost94, gost94cp, gost2001, gost2001cp]
         [ available ]
</pre>
<h4>
And do not forget to (re)install OpenXPKI based on the just installed OpenSSL-gost software 
collection.
</h4>
<p>
Environment variables:
</p>
<pre>
    OPENSSL_PREFIX=${OPENSSL_INSTALLDIR}
    GOST_OPENSSL_ENGINE=${OPENSSL_INSTALLDIR}/lib/engines/libgost.so
</pre>
<p>
should be defined while running the following commands related to the OpenXPKI's server:
</p>
<pre>
    perl Makefile.PL
    make
    make test
</pre>
<p>
After that in addition to the usual western cryptography, users and administrators of 
OpenXPKI will be able to enjoy the following GOST public key algoritms 
(listed in the spelling of OpenXPKI):
</p>
<ul>
      <li><b>GOST94</b> (public key algorithm)</li>
      <li><b>GOST2001</b> (elliptic curves public key algorithm)</li>
      <li><b>GOST94CP</b> (public key algorithm in CP mode<sup>1</sup>)</li>
      <li><b>GOST2001CP</b> (elliptic curves public key algorithm in CP mode<sup>1</sup>)</li>
    </ul>
<p>
In full accord with X.509 standard, all these GOST* algorithms along with all DSA and RSA
algorithms could be used to cross-sign certificates. Thus  chains of arbitrary algorithms
could be found in certification chains of a given PKI.
</p>
<p>
If environment variable:
</p>
<pre>
    GOST_OPENSSL_ENGINE
</pre>
<p>
is UNDEFINED during make procedure for OpenXPKI server,
then GOST-related tests of OpenXPKI are skipped, and support of GOST algorithms in OpenXPKI 
is suspended.
This also applies to the case when cryptographic backend #1 is used
(see <a href="crypto-plug.html">Cryptography abstraction concept</a>). 
Thus the presence of GOST-related code inside of the OpenXPKI makes no harm to the
customer who does not have GOST-enabled cryptographic backend, or knows nothing about GOST. 
</p>

<hr/>
<p>
(<sup>1</sup>)
Digital signature in CP mode has different byte ordering with respect to the 
regular mode. Strictly speaking this mode explicitly violates related Russian federal standards for
digital signature. Nonetheless it is widely employed in some of the MSWindows-based applications. 
Supported here for compatibility.  
</p>

