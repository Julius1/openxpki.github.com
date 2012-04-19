---
layout: page
title: "Steps to create a new custom workflow"
---
{% include JB/setup %}


<h3>by David Hetzl</h3>
<p>
The new workflow will be called "example_wf_request" throughout this document.
For creating customized workflows this text has to be substituted with
whatever name is chosen for the actual workflow.
</p>

Files that need to be created:

<p>
<ul>
<li><a href="#eins">workflow_def_example_wf_request.xml</a></li>
<li><a href="#zwei">workflow_activity_example_wf_request.xml</a></li>
<li><a href="#drei">workflow_validator_example_wf_request.xml</a></li>
</ul>
</p>

<p>
Files that need to be changed:
<ul>
<li><a href="#vier">workflow.xml</a></li>
<li><a href="#fuenf">workflow_condition.xml</a> (only if conditions are used)</li>
<li><a href="#sechs">notification.xml</a> (only if notification is required)</li>
</ul>
</p>


<br>
<h2><a name="eins">1) File 'workflow_def_example_wf_request.xml'</a></h2>

<p>
This file specifies the different states which are valid for the corresponding workflow.
For each state zero to many activities (= actions) can be stated, which will define the valid state transitions. These are explained in detail in the next section.
</p>

<p>
States can be declared as autorun, meaning that the stated action should be run automatically once the
workflow switches to this state. If more than one workflow activity is specified a condition must be used to decide
which of the actions must be executed.
</p>

<p>
<b>Example:</b>
<pre>
  &lt;state name="<b>APPROVED</b>" autorun="yes"&gt;
    &lt;description&gt;I18N_OPENXPKI_WF_STATE_EXTERNAL_CERTIFICATE_TRUST_REQUEST_APPROVAL&lt;/description&gt;
    &lt;action name="<b>create_database_entry</b>"
	    resulting_state="<b>ENTRY_CREATED</b>"&gt;
          &lt;condition name="<b>secure_email_set</b>"/&gt;
    &lt;/action&gt;

    &lt;action name="<b>finish_transaction</b>"
	    resulting_state="<b>SUCCESS</b>"&gt;
          &lt;condition name="<b>!secure_email_set</b>"/&gt;
    &lt;/action&gt;
  &lt;/state&gt;
</pre>
</p>

<p>
This example specifies the state "<i>APPROVED</i>". Once the workflow reaches this state it automatically executes the action
"<i>create_database_entry</i>" or "<i>finish_transaction</i>", depending on the current value of the evaluated condition
"<i>secure_email_set</i>". The resulting state also differs based on this evaluation ("<i>ENTRY_CREATED</i>" or "<i>SUCCESS</i>"). Note that '!' can be used to invert the result of a condition.
</p>

<p>
<b>Activities (=actions)</b> mentioned in this file are specified as following:
</p>

<pre>
  &lt;action name="<b>finish_transaction</b>"
	    resulting_state="<b>SUCCESS</b>"&gt;
          &lt;condition name="<b>!secure_email_set</b>"/&gt;
  &lt;/action&gt;
</pre>
  
<p>
For a detailed explanation about 'Activities' please refer to the section <a href="#zwei">File 'workflow_activity_example_wf_request.xml'</a>.
</p>

<p>
<b>Conditions</b> are specifed as follows:
</p>

<pre>
  &lt;condition name="<b>secure_email_set</b>"/&gt;
</pre>
  
<p>
Please refer to section <a href="#fuenf">File 'workflow_condition.xml'</a> for a detailed explanation on conditions.
</p>

<br>
<h2><a name="zwei">2) File 'workflow_activity_example_wf_request.xml'</a></h2>

<p>
This file goes into more detail regarding activities (which are called "actions" in the Workflow.pm definitions). Activities are Perl modules which should return 1 after succesfull completion.
Any kind of operations can be performed here, but typically this involves workflow manipulations or even database transactions. E.g.
adding a certificate file which was uploaded through the OpenXPKI web interface to the certificate database.
The module must have an execute() method, which will be executed during runtime. All activities inherit from the "Activity" module
('use base qw( OpenXPKI::Server::Workflow::Activity );').
Parameters for activities can be passed in the 'action' tag of the XMl definition file.
</p>

<p>
Every action of this custom workflow must be specified in this file and all fields (variables that are saved in the workflow) have to be stated which are changed
or created during this activity's execution. Failure to do so will result in an exception when trying to access a field name which has not
been specified. Also all validators which have to be evaluated during this activity are named here.
The actual Perl module/class that is called when executing the action is part of the action tag, as can be seen in the next example:
</p>

<pre>
  &lt;action name="import_data"
      class="<b>Workflow::Action::Null</b>"&gt;

      &lt;field name="cert_data"/&gt;	&lt;!-- X.509-Certificate in PEM --&gt;
      &lt;field name="company"/&gt;	&lt;!-- Name of the external company --&gt;
      &lt;field name="comment"/&gt;	&lt;!-- Comments from creator --&gt;

      &lt;!-- Checks if the RT ticket ID is existing--&gt;
      &lt;validator name="TicketExists"&gt;
      &lt;/validator&gt;

      &lt;!-- Checks if CRL is accessible, when specified --&gt;
      &lt;validator name="CRLaccessible"&gt;
      &lt;/validator&gt;

  &lt;/action&gt;
</pre>

<p>
This action will invoke the module <i>workflow::Action:Null</i>, which just does nothing but can be used when there is no need to do anything "special". It is called from the HTML::Mason code which just saves the mentioned fields into the workflow if no validators
fail. It has two validators which will both be evaluated and the action will only complete successfully if both return the value 1.
</p>

<p>
Another example for an action tag which includes a parameter follows:
</p>

<pre>
  &lt;action name="process_crr"
	  class="OpenXPKI::Server::Workflow::Activity::Tools::Export"
          <b>use_destination</b>="0"&gt;
  &lt;/action&gt;
</pre>

<p>
This action parameters can be queried from within the Perl module like this:
</p>

<pre>
  my $self = shift;
  &lt;...&gt;
  my $dest = $self->param('<b>use_destination</b>');
</pre>


<p>
A special kind of action uses the notification module. This can be called when there is the need to interact with the request tracking system. An example can be seen below:
</p>

<pre>
  &lt;action name="close_removal_ticket"
          class="<b>OpenXPKI::Server::Workflow::Activity::Tools::Notification</b>"
          message="<b>ect_removal_close</b>"&gt;
  &lt;/action&gt;
</pre>

<p>
It invokes the special "<i>Notification</i>" module which reads the <a href="#sechs">notification.xml</a> file to see which interactions are linked with the parameter that is passed in the message tag ("<i>ect_removal_close</i>") and executes them. 
Currently there is the option to open new tickets, close existing ones, create a new comment in a ticket and create a correspondence (which sends out an email to the requestor of the ticket). When this module is used all messages have to be specified accordingly in the <a href="#sechs"><i>notification.xml</i></a> file.
</p>

<p>
The validators are specified in this file like the following example:
</p>

<pre>
  &lt;validator name="CRLaccessible"
  &lt;/validator&gt;
</pre>

<p>
Please refer to the next section for a detailed explanation of validators.
</p>

<br>
<h2><a name="drei">3) File 'workflow_validator_example_wf_request.xml'</a></h2>

<p>
This file describes the validators which have been named in the file of the last section.
Validators must evaluate to true (=1) or else the corresponding activity will not be executed. They inherit from the base class
'Workflow::Validator' ('use base qw( Workflow::Validator );') and all processing is done in the validate() method.
If the validation fails it should return a descriptive exception of what went wrong.
Also paramater names can be specified, whose values can be derived from within the Perl module. This is useful for setting config items without the need to change the Perl source code and recompile it.
The following example specifies the validator "<i>ValidApprovalSignature</i>":
</p>

<b>Example:</b>
<pre>
  &lt;validator name="<b>ValidApprovalSignature</b>"
             class="OpenXPKI::Server::Workflow::Validator::ApprovalSignature"&gt;
      &lt;!-- if you set the following parameter to 1, you can enforce
           signatures on all CSR approvals --&gt;
      &lt;param name="<b>signature_required</b>" value="0"/&gt;
  &lt;/validator&gt;
</pre>

<p>
The validator parameter "<i>signature_required</i>" can be accessed from within a Perl module in the _init() method with the following syntax example:
</p>

<pre>
sub _init {
    my ( $self, $params ) = @_;
    if (exists $params->{'<b>signature_required</b>'}) {
        $self->signature_required($params->{'signature_required'});
    }
	return 1;
}
</pre>


<br>
<h2><a name="vier">4) File 'workflow.xml'</a></h2>

<p>
This file lists all 'worflows', 'activities', 'validators' and 'conditions' below the corresponding sections. For our example we would need to add the following three lines:
</p>

<pre>
    &lt;xi:include xmlns:xi="http://www.w3.org/2001/XInclude" href="workflow_def_example_wf_request.xml"/&gt;
    &lt;xi:include xmlns:xi="http://www.w3.org/2001/XInclude" href="workflow_activity_example_wf_request.xml"/&gt;
    &lt;xi:include xmlns:xi="http://www.w3.org/2001/XInclude" href="workflow_validator_example_wf_request.xml"/&gt;
</pre>

<p>
Alternatively, if an all-in-one style deployment is used, you will need
to include these files at the corresponding place in your <i>config.xml</i>.
If you want to check in the changes, you will need to edit the template
files in <i>trunk/deployment/etc/templates/default</i>, which specify both options.
</p>


<br>
<h2><a name="fuenf">5) File 'workflow_condition.xml'</a></h2>

<p>
This file specifies all conditions which can be used for the state transition evaluation mentioned in the '<i>workflow_def_example_wf_request</i>'
section. Conditions are Perl modules which inherit from the base class 'Workflow::Condition' ('use base qw( Workflow::Condition );').
After evaluation they either return 1 (=condition is true) or an exception (=condition is false). The condition check is done by calling
its <i>evaluate()</i> method, so this is where the implementation should go.
</p>

<p>
The '<i>condition</i>' tag includes a logical name and a binding to a Perl module, which is evaluated. The condition used in our former
example is defined as follows:
</p>

<pre>
  &lt;condition name="secure_email_set" class="OpenXPKI::Server::Workflow::Condition::SecureEmailSet"&gt;
    &lt;param name="<b>RDN_filter</b>" value="CN,O"/&gt;
  &lt;/condition&gt;
</pre>

<p>
The condition parameter, used in this example, can be accessed from within the Perl module in the <i>_init()</i> method like follows:
</p>

<pre>
  __PACKAGE__->mk_accessors( '<b>RDN_filter</b>' );
  sub _init
  {
    my ( $self, $params ) = @_;
    if (exists $params->{<b>RDN_filter</b>}) {
        $self->RDN_filter($params->{RDN_filter});
    }
  }
</pre>

<p>
<i>
Note: The mk_accessors method is used to also allow the parameter to be called from outside of the _init() method using '$self->RDN_filter()'.
</i>
</p>

<p>
ACL conditions for authorization are stated similiar, with the keyword '<i>ACL</i>' in front of the name.
</p>

<b>Example:</b>
<pre>
  &lt;condition name="<b>ACL</b>::reject_crr" class="OpenXPKI::Server::Workflow::Condition::<b>ACL</b>"&gt;
    &lt;param name="activity" value="reject_crr"/&gt;
  &lt;/condition&gt;
</pre>


<br>
<h2><a name="sechs">6) File 'notification.xml'</a></h2>

<p>
This file specifies all "messsages" and the corresponsing actions that should be executed when this message is passed as parameter. As already mentioned there are four possible transactions that can be performed. The following section lists all of them together with an examplary usage:
</p>

<ul>
<li><h4>Create new ticket</h4>
<pre>
  &lt;action type="<b>open</b>"&gt;
    &lt;requestor>[% creator %]&lt;/requestor&gt;
    &lt;queue>External CA Trust Request&lt;/queue&gt;
    &lt;subject>Implementation Request for [% company %]&lt;/subject&gt;
  &lt;/action>
</pre>
</li>

<li><h4>Close a ticket</h4>
<pre>
  &lt;action type="<b>close</b>"/>
</pre>
</li>

<li><h4>Create a comment</h4>
<pre>
  &lt;action type="<b>comment</b>"&gt;
    &lt;template file="/etc/openxpki/instances/trustcenter1/notification/en/ect_request_comment.txt" lang="en_EN"/&gt;
  &lt;/action&gt;
</pre>
</li>

<li><h4>Correspond (write Email to requestor)</h4>
<pre>
  &lt;action type="<b>correspond</b>"&gt;
    &lt;template file="/etc/openxpki/instances/trustcenter1/notification/en/ect_approved_correspond.txt" lang="en_EN"/&gt;
  &lt;/action&gt;
</pre>
</li>
</ul>
