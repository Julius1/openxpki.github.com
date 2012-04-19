---
layout: page
title: "OpenXPKI Git Collaboration Howto"
---
{% include JB/setup %}


<p>
OpenXPKI uses Subversion as primary and authoritative source code 
control system. Keeping this repository in a clean state should be the 
primary goal of all developers.
</p>
<p>
However, this central approach reduces the possible interaction
between developers: if one of them is working on some highly 
experimental code, it is frequently not desirable to check in this 
code into the repository. It would "pollute" the clean state and 
possibly break things that used to work. In this state the developer
can only work alone on his personal source, nobody can help him properly.
Even if interaction is not desired, Git can be used to easily create
topic branches which makes life easier for a developer working on a
number of different topics at the same time.
</p>
<p>
The main idea now is to continue using Subversion for the official checkins,
i. e. development stages that are consistent within themselves (at least
the automatic tests still run properly).
</p>
<p>
The dirty work gets offloaded to a <em>second</em> source code control
system that works separately from Subversion and that allows ad-hoc
collaboration: <a href="http://git.or.cz/">Git</a>.
</p>

<h2>Collaboration models</h2>
<p>
One way around this problem is to use Git as a localized version
control system <em>in addition to</em> Subversion. This document should
help developers setting up their local Subversion OpenXPKI repositories
for collaboration use with Git.
</p>
<p>
The OpenXPKI project supports three possible "development modes" for
contributors:
</p>
<dl>
  <dt>Subversion only</dt>
  <dd>Good old way of contributing code to OpenXPKI. You are not
    doing anything wrong this way. If you are happy, why are you
    reading this document at all...?
  </dd>
  <dt>Subversion plus Git</dt>
  <dd>
  Ideal for single developers or small teams collaborating on
  experimental features,
    yielding maximum efficience without "polluting" the official
    repository. This is appropriate if you are a registered 
    OpenXPKI developer yourself.
  </dd>
  <dt>Git only</dt>
  <dd>Suitable for non-permanent developers or contributors. The easiest
    way if you plan to collaborate with another developer who has already 
    created and published an own Git repository of OpenXPKI. 
  </dd>
</dl>

<h2>Common prerequisites</h2>
<p>
These steps should be performed if you wish to use Git with OpenXPKI
regardless of collaboration model.
</p>
<p>
Set up proper author name and email address:
</p>
<pre>
$ export GIT_AUTHOR_EMAIL="johndoe@example.com"
$ export GIT_AUTHOR_NAME="John Doe"
</pre>
<p>
You can also set these values within the Git repository (in which case
they only apply to this single repository) by adding the following
to .git/config
</p>
<pre>
[user]
        email = johndoe@example.com
        name = "John Doe"
</pre>

<h2>Subversion plus Git</h2>
<h3>Initial setup</h3>
<p>
To checkout a subversion tree that is usable using git, you can use
git-svn clone:
<pre>
$ mkdir openxpki.git
$ cd openxpki.git
$ git-svn clone https://johndoe@openxpki.svn.sourceforge.net/svnroot/openxpki 
</pre>
This will take quite a while as it will import the complete revision history from the subversion repository. Alternatively, you can clone a publicly available git repository, for example the one at <a href="http://repo.or.cz/w/openxpki.git">repo.or.cz</a>:
<pre>
$ mkdir openxpki.git
$ cd openxpki.git
$ git clone git://repo.or.cz/openxpki.git
</pre>
</p>

<p>
<h3>Daily usage</h3>
</p>

<p><tt>git-svn rebase</tt> fetches changes from the subversion repository
and rebases your changes onto the latest subversion HEAD. If you want to push your git tree somewhere, you will want to do this only on the master tree (which will be your "clean SVN tree") and use <tt>git merge master</tt> in your topic branches.
</p>
<p/>
<p><tt>git checkout -b mytopic master</tt> creates a new topic branch off of the master branch for you and changes into it. You can now work there, use <tt>git commit</tt> as often as you like until you're happy with your work. You can commit all your changes as single commits back to subversion using <tt>git-svn dcommit</tt>. If you're committing a lot locally (which is a good thing), you might want to create patches using <tt>git-format-patch master</tt> for everything you committed since branching off of the master branch, apply them manually to a temporary branch and combine things that belong together into one Git commit and run <tt>git-svn dcommit</tt> from that temporary branch.</p>

<p>For visualizing your commits and changes, using a graphical tool such as <a href="http://sourceforge.net/projects/qgit">QGit</a> or <a href="http://jonas.nitro.dk/tig/">tig</a> (if you like the console better) is recommended.
<h3>Collaboration</h3>
Add remote Git repositories for all people you want to collaborate with, for example:
</p>
<pre>
$ git-remote add alech git://repo.or.cz/openxpki/alech.git
</pre>
<p>
You are now ready to fetch or pull changes from the remote repositories,
e.g. from the "alech" remote we created above:
</p>
<pre>
$ git fetch alech
</pre>
<p>
The changes "alech" published will now show up as tracking branches
as "alech/master", "alech/sometopicbranch", etc. You can branch away from them, merge them into your local branches, etc.
</p>
<p>
In order to allow others to get your own changes you have to publish
your Git repository. You will have to set up a publicly available Git 
repository to make this possible. Only you will need write access anybody
else needs read-only access. We will assume that you will publish via
ssh and that others get access via HTTP. 
</p>
<p>
On the publicly available system create the remote repository 
directory and create the Git infrastructure:
</p>
<pre>
remote$ cd ...
remote$ mkdir openxpki.git
remote$ cd openxpki.git
remote$ git-init-db
</pre>
<p>
Configure your system to provide read-only access to ...openxpki.git
for the public via HTTP.
</p>
<p>
Now add your own remote repository to your local git remotes by adding
the following to your .git/config file:
</p>
<pre>
[remote "mypublic"]
    url = ssh://johndoe@somehost.example.com/path/to/openxpki.git/
</pre>
<p>
And finally push your local directory to the public repository:
</p>
<pre>
$ git push mypublic
</pre>
<p>
You can also push all your branches by saying git push --all mypublic
</p>
<p>
If you don't have a place where you can push your changes, have a look
at <a href="http://repo.or.cz/">repo.or.cz</a>. There, you can set up
your repository easily and for free, and you can even mark it as an
OpenXPKI "fork", so it will be easier for people to find.
</p>

<h2>Git only</h2>
<p>
For Git-only-mode you can skip the Subversion checkout. Instead choose
a Git repository to clone from and run:
</p>
<pre>
$ git clone http://somehost.example.com/....git/openxpki.git
</pre>
<p>
This automatically sets up Git to pull from this repository to the "origin"
branch (check the .git/remotes/origin file for this). Setting up
public repository replicas basically works the same as outlined in the above
section.
</p>

<h2>Further reading</h2>
The following documentation might prove useful:
<ul>
    <li><a href="http://utsl.gen.nz/talks/git-svn/intro.html">An introduction to git-svn for Subversion/SVK users and deserters</a></li>
    <li><a href="http://git.or.cz/course/svn.html">Git - SVN Crash Course</a></li>
    <li><a href="http://www.kernel.org/pub/software/scm/git/docs/everyday.html">Everyday GIT With 20 Commands Or So</a></li>
</ul>
