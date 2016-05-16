.. _policy:

Development and Core Tool Policy
================================

This section contains policy statements regarding software development that all
developers working on the DIMS project are expected to adhere to.

In order to prevent core tools being used by developers being incompatible,
rendering installation instructions buggy and/or causing random failures in a
complicated build environment, everyone on the DIMS project **must** use the
same core tools, and use the same workflow processes.  This will allow
controlled updates and provide stability in the tools we are using within the
project.

Without some discipline and adherence to documented policies, far too much time
ends up being wasted when one person can do something, but another can't, or
something runs fine on one system, but fails on another system. In either case,
team members get blocked from making forward progress and the project suffers
as a result. These policies are not being imposed to stifle anyone's creativity,
but to help *everyone* on the team be more productive.

.. attention::

    The requirement to adhere to the policies stated here is partly to keep
    the project moving forward smoothly, but also to ensure that the sofware
    products developed by this project are suitable for public open source
    release as required by the contract (see :ref:`dimssr:opensourcerelease` in
    :ref:`dimssr:dimssystemrequirements`) and in conformance with University of
    Washington policy.

..

.. _generalphilosophy:

General Software Development Philosophy
---------------------------------------

This section covers some very high-level philosophical points
that DIMS software developers should keep in mind.

There are a huge `List of software development philosophies`_ on
Wikipedia. One of the most relevant to the DIMS project, based on
a contractual requirement (see :ref:`dimssr:agileDevelopment`)
is the `Agile Manifesto`_. This manifesto is based on twelve
principles:

#. Customer satisfaction by early and continuous delivery of valuable software
#. Welcome changing requirements, even in late development
#. Working software is delivered frequently (weeks rather than months)
#. Close, daily cooperation between business people and developers
#. Projects are built around motivated individuals, who should be trusted
#. Face-to-face conversation is the best form of communication (co-location)
#. Working software is the principal measure of progress
#. Sustainable development, able to maintain a constant pace
#. Continuous attention to technical excellence and good design
#. Simplicity—the art of maximizing the amount of work not done—is essential
#. Self-organizing teams
#. Regular adaptation to changing circumstance

.. _Agile Manifesto: http://www.agilemanifesto.org/principles.html
.. _List of software development philosophies: https://en.wikipedia.org/wiki/List_of_software_development_philosophies

* **Avoid friction** - "Friction" is anything that slows down an otherwise
  smooth running process. Little things that are broken, missing facts,
  new programs that you wrote that don't yet have any documentation,
  all make it harder for someone to get work done because something
  causes friction. Everything grinds to a halt until the little roadblock
  can be removed and then it takes more time to ramp back up to speed.

* **Take control** - Relying on the default behaviors that are programmed into
  an open source product that we use within the DIMS project, without fully
  understanding them, can cause problems. When possible, being explicit about
  how programs are configured and how they are invoked can make these opaque
  default behaviors less of a problem.

* **Make it simple** - It may take a little effort, but being focused on
  finding a simple solution that can be applied uniformly makes it easier
  to intergrate a large set of components. The more differences there are
  the way a subsystem or service is configured on multiple hosts (like
  DNS, for example) means the behavior is random and unpredictable from
  one computer system to another, causing friction

* **Make it work first, then make it better** - Trying to engineer a complete
  solution to some need can mean delays in getting something working, which
  delays getting that component integrated with other components. Or worrying
  about how slow something might be during initial development and trying to
  optimize the solution before it is even working and tested by someone
  else. Make it work first, doing something simple, then deal with
  optimization and a comprehensive feature set later.

* **Avoid hard coding!!!** - When ever possible, avoid using hard-coded
  values in programs, configuration files, or other places where a
  simple change of plans or naming conventions results in having to
  go find and edit dozens of files.  A complete system made up of
  multiple services and software components that must be replicated
  as a whole *cannot* possibly be replicated if someone has to hunt
  down and change hundreds of values in files spread all over the
  place.

* **Ansible-ize all the things** - All configuration, package installation,
  or entity creation on a computer system should be looked at in terms
  of how it can be automated with Ansible. Whenever you are tempted to
  run a command to change something, or fire up an editor to set a
  variable, *put it in Ansible and use Ansible to apply it*. Manual
  processes are not well documented, are not managed under version
  control, are not programatically repeatable, and make it harder to
  scale or replicate a system of systems because they cause *huge*
  amounts of friction.

* **Template and version control all configuration** - Adding a new service
  (e.g., Nginx, or RabbitMQ) that may have several configuration files is not
  just a one-time task. It will be repeated many times, for testing, for
  alternate deployments, or when hardware fails or virtual machines get
  upgraded.  Don't think that cutting corners to get something up and running
  fast by just hand-configuration is the right way to go, because doing it
  again will take as much time (or maybe even longer, if someone unfamiliar
  with the process has to do it the next time).  Take the time when adding a
  new service to learn how it is configured, put all of its configuration files
  under Ansible control, and use Ansible playbooks or other scripts to do the
  configuration at deployment time and at runtime.

* **Done means someone else can do it, not just you.** A program that
  compiles, but nobody else can run, is not done. A bug that was fixed,
  but hasn't been tested by someone other than the person who wrote the
  code or fixed the bug, is not done. Something that doesn't have
  documentation, or test steps that explain how to replicate the
  results, are not done.

* **You can't grade your own exam** Tickets should not be closed until
  someone else on the team has been able to validate the results.

* **Document early, document often** - A program that has no documentation,
  or a process someone learns that has no documentation to record that
  knowledge and how to use it, doesn't contribute much to moving the
  project forward. We are a team who mostly works independently, across
  multiple timezones and on different daily schedules.

.. _sourcecontrol:

Source Code Control
-------------------

As pointed out by Jeff Knupp in his blog post `Open Sourcing a Python Project
the Right Way`_, "git and GitHub have become the de-facto standard for Open
Source projects." Just as Knupp's post suggests, the DIMS project has been
following the same *git-flow* model described by Vincent Driesen in his `A
successful Git branching model`_ blog post, using *Sphinx* and *RST* (see the
section :ref:`documentation`), and using *continuous integration* via Jenkins
(see :ref:`continuousintegration`).

.. _Open Sourcing a Python Project the Right Way: https://www.jeffknupp.com/blog/2013/08/16/open-sourcing-a-python-project-the-right-way/
.. _A successful Git branching model: http://nvie.com/posts/a-successful-git-branching-model/

.. _copyright:

Copyright
---------

All source code should include a copyright statement with the year the
project started (2013) and the current year, as shown here:

.. code-block:: python

   #!/usr/bin/env python
   #
   # Copyright (C) 2013, 2015 University of Washington. All rights reserved.
   #
   # ...

..

.. note::

    Where possible, include the actual copyright symbol. For example, in Sphinx
    documents, follow the instructions in Section :ref:`textsubstitution`.

..

.. _license:

License
-------

All source code repositories shall include the following license statement
to accompany the Copyright statement in the previous section.

.. include:: ../../license.txt
   :literal:


.. _developingongithub:

Developing on a fork from GitHub
--------------------------------

In this section, we will go through the steps for using Hub Flow for
developing on a branch forked from GitHub, publishing the results back
to GitHub for others to share.

For this example, there has already been a fork made on GitHub.  Start by
cloning it to your local workstation: ::

    [dittrich@localhost git (master)]$ git clone https://github.com/uw-dims/sphinx-autobuild.git
    Cloning into 'sphinx-autobuild'...
    remote: Counting objects: 366, done.
    remote: Total 366 (delta 0), reused 0 (delta 0)
    Receiving objects: 100% (366/366), 62.23 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (180/180), done.
    Checking connectivity... done.
    [dittrich@localhost git (master)]$ cd sphinx-autobuild/
    [dittrich@localhost sphinx-autobuild (develop)]$ git branch -a
    * develop
      remotes/origin/HEAD -> origin/develop
      remotes/origin/develop
      remotes/origin/feature/1-arbitrary-watch
      remotes/origin/feature/tests
      remotes/origin/master
    [dittrich@localhost sphinx-autobuild (develop)]$ git checkout master
    Branch master set up to track remote branch master from origin by rebasing.
    Switched to a new branch 'master'
    [dittrich@localhost sphinx-autobuild (master)]$ git branch -a
      develop
    * master
      remotes/origin/HEAD -> origin/develop
      remotes/origin/develop
      remotes/origin/feature/1-arbitrary-watch
      remotes/origin/feature/tests
      remotes/origin/master
    [dittrich@localhost sphinx-autobuild (develop)]$ ls
    AUTHORS				NEWS.rst			fabfile.py			requirements-testing.txt
    CONTRIBUTING.rst		README.rst			fabtasks			requirements.txt
    LICENSE				docs				pytest.ini			setup.py
    MANIFEST.in			entry-points.ini		requirements-dev.txt		sphinx_autobuild

..

.. todo::

    .. warning::

        NOT FINISHED YET.

    ..

..

Developing
----------

Developing new features for the DIMS CI Utilities...

.. todo::

    .. warning::

        NOT FINISHED YET.

    ..

..
