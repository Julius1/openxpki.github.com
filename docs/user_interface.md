---
layout: page
title: "Layer Design"
---
{% include JB/setup %}



<p>
  OpenXPKI tries to implement a MVC design (Model-View-Controller).
  The parts are defined as follows:
</p>
<dl>
  <dt>Model</dt>
  <dd>
    The model is implemented by a workflow engine. This
    includes things like the configuration or the state
    handling.
  </dd>
  <dt>Controller</dt>
  <dd>
    The controllers are represented by the different workflow
    activities. Every activity can manipulate the state
    of a workflow including its properties.
  </dd>
  <dt>View</dt>
  <dd>
    The view is represented by the user interfaces. I have to
    admit though that the term is not fully correct. The
    user interface consists of several layers to get better code:
    <ul>
      <li>the service layer</li>
      <li>the messaging or serialization layer</li>
      <li>the transport layer</li>
    </ul>
  </dd>
</dl>
<p>
  The major idea is to make the user interfaces much
  more flexible. It should be possible to use different
  transport protocol and it must also be possible to
  user other frontend language than Perl (e.g. PHP).
  The different layers are defined as follows:
</p>
<ol>
  <li>The service layer
    <p>
      This layer parses queries, executes necessary
      commands and build resulting answers.
      The format of the queries and answers are defined by
      the interface itself but in the native language (Perl).
      This means that a message can look like this:
    </p>
<pre>
  my %msg = (COMMAND => "search_csr",
             PARAMS  => {CN => "*Doe*"},
             LIMIT   => 20);
</pre>
    <p>
      The service layer defines a communication protocol but
      only on the level of native Perl messages. This means
      that a client send the above message but the answer is:
    </p>
<pre>
  my %msg = (ANSWER_TYPE => "COMMAND",
             COMMAND     => "GET_PKI_REALM",
             PKI_REALMS  => {"root_ca" => "Our super root CA",
                             "user_ca" => "User CA"});
</pre>
    <p>
      If this happens then the client must answer with root_ca
      or user_ca. Usually the next answer is the request for
      a login stack followed by the real login. Additionally
      the first answer can include a session ID. Like you see
      it is possible to implement a real communication protocol
      here. The only important thing is that we do not define
      any aspect of the serialization (the message representation)
      and the transport here.
    </p>
    <p>
      Please note that it is highly important that every message
      contains a hash key <i>SERVICE</i>. This is necessary to
      signal the receiver of a message which service should be
      used after the deserialization of a message.
    </p>
  </li>
  <li>The messaging or serialization layer
    <p>
      The service layer only defines how messages should look
      like in the used native language (Perl). This means a
      message is usually a hash. The serialization layer must
      produce now a plain text representation of this data.
      This can be a XML file or every other specially formatted
      text file. Please note that file does not mean that we
      write it on the disk. It is only a string in the memory.
    </p>
    <p>
      This layer only has to serialize datastructures and
      to deserialize strings. It is not the job of this layer
      to distinguish between different message types. This
      is the job of the service layer which defines the
      real protocol. It is possible of course that some
      messages are serialized another way because they carry
      a special flag but this makes no difference for the
      interface usage and behaviour of this layer.
    </p>
  </li>
  <li>The transport layer
    <p>
      The job of the transport layer is simple. It gets a plain
      text string and must bring this string to the other peer.
      The transport layer must not contain any knowledge about
      the communication protocol.
    </p>
    <p>
      The minimum interface looks like this until now:
    </p>
    <ul>
      <li>new (incl. open)</li>
      <li>close</li>
      <li>write</li>
      <li>read</li>
    </ul>
    <p>
      The problem is that this interface is fully synchronous.
      Actually I see no problem here but perhaps we must extend
      this interface in the future to support asynchronous
      mechanisms too. Nevertheless PKI is today mainly
      a synchronous business.
    </p>
  </li>
</ol>

<h2>Reference Implementation</h2>

<p>
  The practical handling of this protocol stack is really
  simple. The server gets an incoming connection. The
  first line of the incoming connection must be
  <i>start simple</i>. This signals the server which transport
  protocol must be used. After the initialization of the
  transport protocol the first message is read. The first
  message is the name of the serialization protocol.
  An example could be <i>Simple</i>. The server answers
  <i>OK</i> and the next message is parsed with the
  appropriate class <i>OpenXPKI::Serialization::Simple</i>.
  The parsed data structure must include a hash key
  <i>SERVICE</i> which announces the used service class.
</p>
  
<h3>Service Protocol</h3>

<p>
  The first step is that we define a normal protocol
  workflow. If it is possible then we should express
  the whole protocol as one formula.
</p>
<pre>
    session ::= (init_session|continue_session).
                use_session*.
                (stop_session|nothing)

    init_session ::= create_session_id.
                     select_pki_realm.
                     select_auth_stack.
                     login

    use_session ::= request_command.
                    (
                     token_login*.
                     send_answer*.
                    )*.
                    (answer_complete|error)

    stop_session ::= delete_session_id
</pre>
<p>
  This is a first abstraction for a session. Please note
  that this works for web clients which cut the connection
  everytime and for shell clients which are connected until
  they logout.
</p>

<h3>Serialization</h3>

<p>
  Please see
  <a href="http://svn.sourceforge.net/viewvc/openxpki/trunk/perl-modules/core/trunk/OpenXPKI/Serialization/Simple.pm?view=markup">
    "perldoc OpenXPKI::Serialization::Simple"
  </a>.
</p>

<h3>Transport</h3>

<p>
  Please see
  <a href="http://svn.sourceforge.net/viewvc/openxpki/trunk/perl-modules/core/trunk/OpenXPKI/Transport/Simple.pm?view=markup">
    "perldoc OpenXPKI::Transport::Simple"
  </a>.
</p>

<h3>Example</h3>

<h4>Server View</h4>
<ul>
  <li>Established network connection</li>
  <li>Detected transport protocol</li>
  <li>Initialized transport protocol</li>
  <li>Send OK</li>
  <li>Read first message</li>
  <li>Detected serialization mechanism</li>
  <li>Initialized serialization mechanism</li>
  <li>Send OK</li>
  <li>Read service request</li>
  <li>Deserialize service request</li>
  <li>Initialize service with transport protocol and
    serialization mechanism</li>
  <li>Send OK</li>
  <li>Run service</li>
  <li>Service read next message</li>
  <li>Service performes the requested action</li>
  <li>Service prepares answer</li>
  <li>Service serializes (part) answer</li>
  <li>Service send answer</li>
  <li>Waiting for next message or terminating</li>
</ul>
<h4>Client View</h4>
<ul>
  <li>Connect server</li>
  <li>Send transport protocol identifier</li>
  <li>Read OK from server</li>
  <li>Send serialization mechanism</li>
  <li>Read OK from server</li>
  <li>Send requested service</li>
  <li>Read OK from server</li>
  <li>Creating query</li>
  <li>Serialize query</li>
  <li>Write query to transport layer</li>
  <li>Send serialized message</li>
  <li>Receive answer</li>
  <li>Deserialize answer</li>
  <li>Evaluate answer</li>
  <li>Continue with waiting for the next message (e.g. exports),
    sending a response or next query</li>
</ul>
  
