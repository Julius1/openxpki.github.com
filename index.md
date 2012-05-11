---
layout: page
title: OpenXPKI
tagline: An open, enterprise-grade PKI/Trustcenter
---
{% include JB/setup %}

# About OpenXPKI #

The OpenXPKI Project has created an enterprise-grade PKI/Trustcenter software
that supports well-established infrastructure components like RDBMS and 
Hardware Security Modules. Flexibility and modularity are the project's key
design objectives.

<p>
  Unlike many other OpenSource PKI projects OpenXPKI offers
  <a href="/features/index.html">powerful features</a>
  necessary for professional environments that are usually
  only found in commercial grade PKI products. (If you have ever wondered
  what could be done to provide <b>continuous operation</b> of a PKI without
  having to struggle with the system every time your CA certificate expires,
  OpenXPKI is probably the right thing for you.)
</p>
<p>
  However, we also target small scale installations by providing 
  quick-start configuration examples that allow to get a usable PKI 
  running quickly.
</p>
<p>
  OpenXPKI runs on most Unix-like operating system (verified on 
  FreeBSD, Linux, Solaris/OpenSolaris and Mac OS X).<br/>
  Database backends exist for MySQL, PostgreSQL, Oracle and DB2.<br/>
  OpenXPKI also integrates with the 
  <a href="http://bestpractical.com/rt/">RT Request Tracker</a> and
  supports nCipher's nShield Hardware Security Modules.
</p>

<dl>
<dt>Architecture White Paper available</dt>
<dd>The OpenXPKI Team has compiled a White Paper on the architecture 
  and key features of the OpenXPKI software. The paper is 
  <a href="docs/OpenXPKI-Architecture-Overview.pdf">available 
    as a PDF Document here</a> and outlines the architecture of the
  system. Development follows the concepts described there closely.
</dd>
</dl>


# News #

<p>
	  This section reports major milestones of the OpenXPKI
	  project as well as important events (such as Workshops).
</p>



<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



<dl>
<dt>2009-Jan-14
</dt>
<dd>
<a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/273">
OpenXPKI now works with all versions of Apache at all supported platforms in both mod_perl and cgi modes
</a>
</dd>

<dt>2008-Aug-14
</dt>
<dd><a href="http://wiki.openxpki.org">OpenXPKI wiki created
</a>
</dd>

<dt>2008-Jul-29
</dt>
<dd>
<a href="http://permalink.gmane.org/gmane.comp.security.openxpki.user/210">IRC channel #openxpki on freenode created
</a>
</dd>

<dt>2008-May-15
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/239">OpenXPKI Live (beta) affected by Debian OpenSSL bug
</a>
</dd>

<dt>2008-Jan-04
</dt>
<dd>
<a href="http://permalink.gmane.org/gmane.comp.security.openxpki.user/164">New OpenXPKI Live (beta) released
</a>
</dd>

<dt>2007-Sep-06</dt>
<dd>
<a href="http://permalink.gmane.org/gmane.comp.security.openxpki.user/142">OpenXPKI Live (beta) released
</a>
</dd>

<dt>2007-Jun-22
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/138">Workflow ACLs
</a>
</dd>

<dt>2007-Jun-12
</dt>
<dd>
<a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/127">Configuration versioning
</a>
</dd>

<dt>2007-May-22
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/121">Automatic generation of random passwords during deployment
</a>
</dd>

<dt>2007-May-09
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/113">Support for local changes to translation files
</a>
</dd>

<dt>2007-May-07
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/114">Automated testing reports
</a>
</dd>

<dt>2007-Apr-17</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/90">Template-based certificate subject generation
</a>
</dd>

<dt>2007-Mar-02
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/76">Browser-based approval using digital signatures
</a>
</dd>

<dt>2007-Feb-09
</dt>
<dd><a href="http://permalink.gmane.org/gmane.comp.security.openxpki.devel/60">Notification framework and Request Tracker interface
</a>
</dd>
<dt>2007-Feb-02
</dt>
<dd>Certificate revocation request workflow and interface functional
</dd>
<dt>2007-Jan-26
</dt>
<dd>First successful production deployment of OpenXPKI 
<a href="/2007/01/26/success-story">
	      (SmartCard personalization self-service application)
</a>
</dd>
<dt>2006-Dec-27
</dt>
<dd>Talk at the 23rd Chaos Communication Congress, Berlin (<a href="http://events.ccc.de/congress/2006/Fahrplan/attachments/1214-OpenXPKI_23C3_slides.pdf">slides
</a>, 
<a href="http://events.ccc.de/congress/2006/Fahrplan/attachments/1113-OpenXPKI_23C3_Paper.pdf">paper
</a>, 
<a href="ftp://ftp.ccc.de/congress/2006/video/23C3-1596-en-openxpki.m4v">video</a>, <a href="ftp://ftp.ccc.de/congress/2006/audio/23C3-1596-en-openxpki.mp3">audio
</a>)
</dd>
<dt>2006-Nov-14
</dt>
<dd>Anonymous interface completed (incl. some style changes).
</dd>
<dt>2006-Nov-11
</dt>
<dd>SCEP support for initial enrollment and renewal committed.
</dd>
<dt>2006-Sep-21
</dt>
<dd>First version of a data exchange committed.
</dd>
<dt>2006-Sep-20
</dt>
<dd>First documentation of the crypto layer concept published.
</dd>
<dt>2006-Sep-05
</dt>
<dd>First working workflows: CRL generation, CSR, certificate issuance
</dd>
<dt>2006-May-30
</dt>
<dd>The project officially migrated from BerliOS to SourceForge.net.
</dd>
<dt>2006-May-12
</dt>
<dd>First version of support for GOST algorithms works.
</dd>
<dt>2006-Apr-06
</dt>
<dd>First version of a CLI framework commited.
</dd>
<dt>2006-Mar-30
</dt>
<dd>Automatic CA rollover works.
</dd>
<dd>Validity specification for certificate and CRL profiles finished.
</dd>
<dt>2006-Feb-21
</dt>
<dd>Authentication and ACL framework is complete.
</dd>
<dt>2005-12-09</dt>
<dd>
<a href="../docs/OpenXPKI-Architecture-Overview.pdf">
	      Architecture White Paper</a> released
</dd>
<dt><b>2005-Oct-20
</b>
</dt>
<dd>
<b>OpenXPKI project kickoff.
</b>
</dd>

<dt>2005-Oct-17 to 2005-Oct-18
</dt>
<dd>
<a href="../docs/ws20051018.html">
      2. OpenCA Workshop
</a>
</dd>

<dt>2005-Feb-22</dt>
<dd>
<a href="../docs/dfn_bt_2005_02_22.pdf">
      Core Design
</a>
            presented on a meeting of Germany's National Research and Education Network
</dd>
<dt>2004-Oct-11 to 2004-Oct-12
</dt>
<dd>
<a href="../docs/ws20041012.html">
	      1. OpenCA Workshop
</a>
</dd>
</dl>

<h2>Security Advisories</h2>
<p>
<a href="../secadvs/index.html">
	    Security advisories</a> are listed in a dedicated section,
	  in order to make it possible to publish updated advisories 
	  there as well.
</p>
<h2>Releases</h2>
<p>OpenXPKI is currently under development. 
	  There are no official releases yet.
</p>



## To-Do

This site is still unfinished. If you'd like to be added as a contributor,
[please fork](http://github.com/openxpki/openxpki.github.com)!

We need to migrate content from the old website and clean up the theme. 


