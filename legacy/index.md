---
layout: page
title: "OpenCA Legacy"
group: navigation
---
{% include JB/setup %}


<ul class="nav nav-tabs">
<li><a href="index.html" data-toggle="tab">OpenCA Legacy</a></li>
<li><a href="securityadvisories.html" data-toggle="tab">Security Advisories</a></li>
<li><a href="docs.html" data-toggle="tab">Legacy Docs</a></li>
</ul>

<p>
	  OpenCA is no longer maintained or supported by the OpenXPKI
	  developers. However, we retain some 
<a href="/legacy/docs.html">
	    documentation of the legacy OpenCA 0.9.2 version</a>, as this 
	  information is no longer available the project's website.
	</p>

<h2>OpenCA Sub Projects</h2>
<p>
	  OpenCA includes some tool programs that can be useful for
	  cryptographic software. OpenXPKI makes use of the openca-scep
	  program to provide an SCEP server.
        </p>

<h3>OpenCA SV</h3>
<p>
          This command line tool can sign, verify, encrypt and decrypt
          (binary) data. This includes signatures from HTML forms generated
          with Internet Explorer or Mozilla. You can use this tool for
          encryption of your backups too. The syntax resembles the
	  OpenSSL command line tool.
         </p>
<h3>OCSPD</h3>
<p>
          The OCSPD implements an OCSP server which can use an LDAP 
	  directory as backend.
        </p>
<h3>SCEP</h3>
<p>
          This tool can be used to create or extract SCEP messages. It
          uses a syntax similar to OpenSSL and is used by OpenXPKI
          to provide SCEP functionality.
        </p>
