---
layout: page
title: "Naming conventions"
---
{% include JB/setup %}

<p>
OpenXPKI employs a few naming conventions in its code and configuration files:
</p>

<ul>
<li><i>Methods</i> are usually in lower case and separated by an underscore, i.e. <tt>get_chain</tt>.</li>

<li><i>Internationalization identifiers</i> always start with <tt>I18N_OPENXPKI</tt> and are all caps seperated by underscores. They indicate the path to the module in which they are used, i.e. if you have an exception in <tt>OpenXPKI::Crypto::Toolkit</tt>, the identifier should start with <tt>I18N_OPENXPKI_CRYPTO_TOOLKIT_</tt>.</li>

<li><i>Workflow</i> configuration files only use internationalization identifiers in the type and description, workflow states are short words in capitals, seperated by underscores. Examples are <tt>INITIAL</tt> or <tt>CSR_EXTRACTED</tt>. Workflow activities (which are called actions in the configuration) are short words (usually verbs) in lowercase, such as <tt>extract_csr</tt> or <tt>null</tt>. Workflow conditions and validators use similar conventions. Conditions which have to do with ACLs use the prefix <tt>ACL::</tt> to distinguish them from normal conditions. </li>
</ul>


Of course, these rules are not set in stone, but make working together a bit easier.
