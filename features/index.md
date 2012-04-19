---
layout: page
title: "Features"
group: navigation
---
{% include JB/setup %}


<h2>OpenXPKI features and requirements</h2>
<p>
  OpenXPKI makes a few assumptions about its operating environment. You
  will need some infrastructure components to make it work properly.
</p>

<h3>Operating environment</h3>
<h4>Supported operating systems</h4>
<p>
  OpenXPKI runs on most Unix-like operating systems that use the Unix
  process model and provide a POSIX environment.
  It has been successfully tested on
  <ul>
    <li>FreeBSD</li>
    <li>Linux (tested on Debian GNU/Linux and SuSE SLES)</li>
    <li>Mac OS X (tested on 10.4 and 10.5)</li>
    <li>Sun Solaris and OpenSolaris (tested on version 10 of both)</li>
  </ul>

  Because of some assumptions about the process environment it will
  <b>not</b> run natively under Microsoft Windows.
</p>

<h4>Supported databases</h4>
<p>
  OpenXPKI requires a relational database for operation. Drivers are
  included for
  <ul>
    <li>MySQL</li>
    <li>PostgreSQL</li>
    <li>Oracle</li>
    <li>DB2</li>
  </ul>
  (Adding support for databases not mentioned here should be possible if a 
  Perl DBD driver module exists for this particular database. 
  At a minimum, the database must support multiple concurrent 
  connections (ruling out SQLite for production use) and transaction support.)
</p>

<h4>Request tracking</h4>
<p>
  OpenXPKI provides built-in integration with the 
  <a href="http://bestpractical.com/rt/">RT Request Tracker</a>. It
  can automatically create and link tickets in the RT system for 
  incoming certificate requests and thus allows Registration Officers 
  to keep track of their workload.
</p>


<h3>Key features</h3>

<h4>Multiple CA instances</h4>
<p>
  OpenXPKI supports the configuration of multiple independent logical PKIs 
  ("PKI Realms") in one single application instance. This allows for
  configuration e. g. of a Root CA and one or more subordinate CAs within
  one single installation.
</p>

<h4>Fully automatic CA rollover</h4>
<p>
  Within a logical PKI (PKI Realm) OpenXPKI provides the possibility
  to configure multiple Issuing CAs with overlapping validity.
  Once a new Issuing CA becomes valid it takes over for issuing new
  certificates. This unique feature allows for a fully automatic
  CA rollover where administrators do not have to take down and
  reconfigure the whole PKI installation once a CA certificate is about
  to expire.
</p>

<h4>Highly customizable</h4>
<p>
  Instead of hard-wiring the interface and the PKI operations in a monolithic
  application, OpenXPKI utilizes a workflow engine that allows to
  easily modify and extend the basic operation of the PKI (e. g.
  certificate request and approval). Customizing the behaviour of the
  system is often accomplished by simply modifying the workflow description
  in XML format.
</p>
<p>
  In addition the workflow engine makes it possible to extend the
  system with customized workflows.
</p>

<h4>Hardware Security Module support</h4>
<p>
  Critical cryptographic operations such as Digital Signatures can be 
  performed via a Hardware Security Module. Currently OpenXPKI supports
  nCipher nShield modules.
</p>
