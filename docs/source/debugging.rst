.. _debugging:

Debugging and Development
=========================

This chapter covers some tools and tactics used for testing and debugging
misbehaving system components, or obtaining sufficient detail about how
subsystem components work in order to control them to achieve project goals and
requirement objectives. Executing some command line or triggering an
action in a user interface that results in the system appearing to
"hang" can be caused by many things. Just looking at the surface and
seeing no action is useless in determining the root cause of the
issue. The ability to turn a "black box" into a "transparent box"
is invaluable to the process of testing and debugging.

.. hint::

   Some useful resources on the processes of *testing and debugging*
   are listed here:

      * `Testing and Debugging`_
      * `White-box testing, Wikipedia`_
      * `White-Box Testing`_, by Oliver Cole, March 1, 2000

..

.. _Testing and Debugging: http://www.jodypaul.com/SWE/TD/TestDebug.html
.. _White-box testing, Wikipedia: https://en.wikipedia.org/wiki/White-box_testing
.. _White-Box Testing: http://www.drdobbs.com/tools/white-box-testing/184404030

.. _filesystemeffects:

Determining File System Affects of Running Programs
---------------------------------------------------

Many programs used in the DIMS project consume gigabytes of disk storage,
often in hidden locations that are not obvious to the user. The act of
taking an Ubuntu 14.04 install ISO image, converting it with Packer
into a BOX file, turning that into a Virtualbox image, and instantiating
a Vagrant virtual machine can turn just under 1 GB into 3-5 GB of storage.
Multiply that by a dozen or more virtual machines and this quickly can
add up. If you are not aware of what gets created, and you change names
of virtual machines, you can end up with a huge amount of wasted disk
space with unused virtual disks, virtual machine images, etc.

For this reason, every programmer developing tools that use programs
like this should be methodical about understanding every process in
terms of inputs, process, and **outputs**, such that it is possible
to **see** what is produced to help the person using your tools know
what is happening, and to **undo** those effects in a controlled way
to simplify cleaning up.

.. hint::

   An easy way to help the user of your tools is to be organized and
   put large files related to a workflow like Packer->Virtualbox->Vagrant
   VM creation under a single directory path like ``/vm`` that can
   deleted in one step, backed up and moved to another system with
   a larger hard drive, or expanded by mounting a second hard drive
   onto that directory path as a mount point. Scattering the files
   across many unrelated subdirectories in random locations and
   depths within the user's ``$HOME`` directory tree makes it much
   harder to handle a situation where the hard drive on a laptop
   reaches 100% utilization.

..

Let's take a look at a portion of the workflow of Packer->Virtualbox->Vagrant
creation to see how to white-box disk utilization and space management.

We start by changing directory into the ``$GIT/dims-packer`` repository where
tools for creating Vagrants using Packer are kept. We create an initial empty
file to serve as a marker in time for then locating any files that
were created **after** this file.

.. note::

   The example here will search through a set of directories that were
   chosen based on knowledge that they exist and are used by various tools.
   To obtain this knowledge, it is often helpful to start looking at the
   root of the filesystem (``/``) and look for **any** files in **any**
   directories, which you will quickly find has a lot of unrelated file
   system additions that just happen to have been made at the same time
   as the program you were running.  A more precise way to identify
   where files are created is to trace execution of the program in
   question, following all forked children, using a program like
   ``strace`` and/or ``ltrace``, however these tools require a much
   deeper understanding of how the Unix/Linux kernel works.

..

.. code-block:: none

    $ cd $GIT/dims-packer

..


.. code-block:: none

    $ cd $GIT/dims-packer
    $ find /home/dittrich/.vagrant.d/ /home/dittrich/.packer.d/ /home/dittrich/VirtualBox\ VMs/ . /vm -newer foo -ls
    56230373    4 drwxrwxr-x   7 dittrich dittrich     4096 Mar 15 13:14 /home/dittrich/.vagrant.d/
    56230688    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:14 /home/dittrich/.vagrant.d/data/machine-index/index.lock
    56230689    4 drwxr-xr-x   2 dittrich dittrich     4096 Mar 15 13:14 /home/dittrich/.packer.d/
    56230691    4 -rw-rw-r--   1 dittrich dittrich      318 Mar 15 13:14 /home/dittrich/.packer.d/checkpoint_cache
    55314574    4 drwx------   6 dittrich dittrich     4096 Mar 15 13:24 /home/dittrich/VirtualBox\ VMs/
    58589344 2183628 -rw-------   1 dittrich dittrich 2240348160 Mar 15 13:27 /home/dittrich/VirtualBox\ VMs/vagrant-run-ns1_default_1458069887689_42029/ns1_box-disk1.vmdk
    58987212    4 drwx------   2 dittrich dittrich     4096 Mar 15 13:24 /home/dittrich/VirtualBox\ VMs/devserver
    55574611    4 drwxrwxr-x  19 dittrich dittrich     4096 Mar 15 13:24 .
    58590167    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:24 ./vagrant-output
    58590705 633044 -rw-rw-r--   1 dittrich dittrich 648229139 Mar 15 13:24 ./vagrant-output/packer_devserver_box_virtualbox.box
    55574679    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:14 ./ubuntu_64_vagrant
    55574655    4 -rw-rw-r--   1 dittrich dittrich     3263 Mar 15 13:14 ./ubuntu_64_vagrant/devserver-base.json
    55574654    4 -rw-rw-r--   1 dittrich dittrich     3044 Mar 15 13:14 ./ubuntu_64_vagrant/devserver-box.json
    58590704    4 drwxr-xr-x   2 dittrich dittrich     4096 Mar 15 13:21 ./output-devserver
    58590711   12 -rw-------   1 dittrich dittrich    10629 Mar 15 13:20 ./output-devserver/devserver.ovf
    58590712 620528 -rw-------   1 dittrich dittrich 635416064 Mar 15 13:21 ./output-devserver/devserver-disk1.vmdk
    55574612    4 drwxrwxr-x   8 dittrich dittrich     4096 Mar 15 13:27 ./.git

..


.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/dims-packer (feature/dims-696*) $ touch foo2
    [dimsenv] dittrich@dimsdemo1:~/dims/git/dims-packer (feature/dims-696*) $ find /home/dittrich/.vagrant.d/ /home/dittrich/.packer.d/ /home/dittrich/VirtualBox\ VMs/ . /vm -newer foo2 -ls
    56230373    4 drwxrwxr-x   7 dittrich dittrich     4096 Mar 15 13:33 /home/dittrich/.vagrant.d/
    56230688    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:33 /home/dittrich/.vagrant.d/data/machine-index/index.lock
    55574612    4 drwxrwxr-x   8 dittrich dittrich     4096 Mar 15 13:33 ./.git
    53346305    4 drwxr-xr-x   5 dittrich dittrich     4096 Mar 15 13:33 /vm
    53346306    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:33 /vm/devserver
    53346314    4 -rw-rw-r--   1 dittrich dittrich        1 Mar 15 13:33 /vm/devserver/.vagrant-IP
    53346310    4 -rw-rw-r--   1 dittrich dittrich        6 Mar 15 13:33 /vm/devserver/.vagrant-ISDESKTOP
    53346311    4 -rw-rw-r--   1 dittrich dittrich        7 Mar 15 13:33 /vm/devserver/.vagrant-VMTYPE
    53346312    4 -rw-rw-r--   1 dittrich dittrich        7 Mar 15 13:33 /vm/devserver/.vagrant-PLATFORM
    53346309    4 -rw-rw-r--   1 dittrich dittrich       10 Mar 15 13:33 /vm/devserver/.vagrant-NAME
    53346313    4 -rw-rw-r--   1 dittrich dittrich       32 Mar 15 13:33 /vm/devserver/.vagrant-BOXNAME
    53346316    4 -rw-rw-r--   1 dittrich dittrich       26 Mar 15 13:33 /vm/devserver/.vagrant-VAGRANTFILEPATH
    53346319    8 -rwxrwxr-x   1 dittrich dittrich     4351 Mar 15 13:33 /vm/devserver/test.vagrant.ansible-current
    53346318    8 -rw-rw-r--   1 dittrich dittrich     4245 Mar 15 13:33 /vm/devserver/Makefile
    53346315    4 -rw-rw-r--   1 dittrich dittrich        1 Mar 15 13:33 /vm/devserver/.vagrant-FORWARDPORT
    53346308    4 -rw-rw-r--   1 dittrich dittrich     2738 Mar 15 13:33 /vm/devserver/Vagrantfile
    53346307    4 -rw-rw-r--   1 dittrich dittrich     2028 Mar 15 13:33 /vm/devserver/.vagrant_show
    53346317    4 -rw-rw-r--   1 dittrich dittrich      199 Mar 15 13:33 /vm/devserver/hosts

..



.. code-block:: none

    [dimsenv] dittrich@dimsdemo1:~/dims/git/dims-packer (feature/dims-696*) $ touch foo3
    [dimsenv] dittrich@dimsdemo1:~/dims/git/dims-packer (feature/dims-696*) $ find /home/dittrich/.vagrant.d/ /home/dittrich/.packer.d/ /home/dittrich/VirtualBox\ VMs/ . /vm -newer foo3 -ls
    56230373    4 drwxrwxr-x   7 dittrich dittrich     4096 Mar 15 13:48 /home/dittrich/.vagrant.d/
    56230681    4 drwxrwxr-x   4 dittrich dittrich     4096 Mar 15 13:34 /home/dittrich/.vagrant.d/data
    56232110    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:34 /home/dittrich/.vagrant.d/data/lock.dotlock.lock
    56230688    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:48 /home/dittrich/.vagrant.d/data/machine-index/index.lock
    56232608    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:34 /home/dittrich/.vagrant.d/data/lock.machine-action-fab0a1f680af28d59f47b677629a540a.lock
    56230682    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:35 /home/dittrich/.vagrant.d/tmp
    56230680    4 drwxrwxr-x  11 dittrich dittrich     4096 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes
    58987205    4 drwxrwxr-x   3 dittrich dittrich     4096 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox
    58987206    4 drwxrwxr-x   3 dittrich dittrich     4096 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0
    58987207    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0/virtualbox
    58987202 646144 -rw-rw-r--   1 dittrich dittrich 661647360 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0/virtualbox/devserver_box-disk1.vmdk
    58987203    4 -rw-rw-r--   1 dittrich dittrich       26 Mar 15 13:35 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0/virtualbox/metadata.json
    58987200    4 -rw-rw-r--   1 dittrich dittrich      258 Mar 15 13:34 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0/virtualbox/Vagrantfile
    58987201   12 -rw-rw-r--   1 dittrich dittrich    10785 Mar 15 13:34 /home/dittrich/.vagrant.d/boxes/packer_devserver_box_virtualbox/0/virtualbox/box.ovf
    55574611    4 drwxrwxr-x  19 dittrich dittrich     4096 Mar 15 13:48 .
    55574612    4 drwxrwxr-x   8 dittrich dittrich     4096 Mar 15 13:48 ./.git
    55575296    4 -rw-rw-r--   1 dittrich dittrich     2590 Mar 15 13:48 ./make-devserver-201603151348.txt
    53346306    4 drwxrwxr-x   5 dittrich dittrich     4096 Mar 15 13:48 /vm/devserver
    53346314    4 -rw-rw-r--   1 dittrich dittrich       14 Mar 15 13:48 /vm/devserver/.vagrant-IP
    53346310    4 -rw-rw-r--   1 dittrich dittrich        6 Mar 15 13:48 /vm/devserver/.vagrant-ISDESKTOP
    53346311    4 -rw-rw-r--   1 dittrich dittrich        7 Mar 15 13:48 /vm/devserver/.vagrant-VMTYPE
    53346312    4 -rw-rw-r--   1 dittrich dittrich        7 Mar 15 13:48 /vm/devserver/.vagrant-PLATFORM
    53346309    4 -rw-rw-r--   1 dittrich dittrich       10 Mar 15 13:48 /vm/devserver/.vagrant-NAME
    53346313    4 -rw-rw-r--   1 dittrich dittrich       32 Mar 15 13:48 /vm/devserver/.vagrant-BOXNAME
    53347678    4 drwxrwxr-x  10 dittrich dittrich     4096 Mar 15 13:48 /vm/devserver/dims-keys
    53347720    0 -rw-rw-r--   1 dittrich dittrich        0 Mar 15 13:48 /vm/devserver/dims-keys/README.rd
    53347719    4 -rw-rw-r--   1 dittrich dittrich       43 Mar 15 13:48 /vm/devserver/dims-keys/.gitignore
    53347722    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:48 /vm/devserver/dims-keys/ansible-pub
    . . .
    53347752    4 -rw-rw-r--   1 dittrich dittrich      402 Mar 15 13:48 /vm/devserver/dims-keys/ssh-pub/dims_andclay_rsa.pub
    53347775    4 -rw-rw-r--   1 dittrich dittrich       79 Mar 15 13:48 /vm/devserver/dims-keys/ssh-pub/dims_eliot_rsa.sig
    53346320    4 drwxrwxr-x   3 dittrich dittrich     4096 Mar 15 13:34 /vm/devserver/.vagrant
    53346321    4 drwxrwxr-x   3 dittrich dittrich     4096 Mar 15 13:34 /vm/devserver/.vagrant/machines
    53346322    4 drwxrwxr-x   3 dittrich dittrich     4096 Mar 15 13:34 /vm/devserver/.vagrant/machines/default
    53346323    4 drwxrwxr-x   2 dittrich dittrich     4096 Mar 15 13:34 /vm/devserver/.vagrant/machines/default/virtualbox
    53346316    4 -rw-rw-r--   1 dittrich dittrich       26 Mar 15 13:48 /vm/devserver/.vagrant-VAGRANTFILEPATH
    53346318    8 -rw-rw-r--   1 dittrich dittrich     4245 Mar 15 13:48 /vm/devserver/Makefile
    53346315    4 -rw-rw-r--   1 dittrich dittrich        1 Mar 15 13:48 /vm/devserver/.vagrant-FORWARDPORT
    53346308    4 -rw-rw-r--   1 dittrich dittrich     2751 Mar 15 13:48 /vm/devserver/Vagrantfile
    53346307    4 -rw-rw-r--   1 dittrich dittrich     2041 Mar 15 13:48 /vm/devserver/.vagrant_show
    53346317    4 -rw-rw-r--   1 dittrich dittrich      212 Mar 15 13:48 /vm/devserver/hosts

..



.. code-block:: none

    $ touch foo4
    $ find /home/dittrich/.vagrant.d/ /home/dittrich/.packer.d/ /home/dittrich/VirtualBox\ VMs/ . /vm -newer foo4 -ls
    58589344 2183628 -rw-------   1 dittrich dittrich 2240348160 Mar 15 14:17 /home/dittrich/VirtualBox\ VMs/vagrant-run-ns1_default_1458069887689_42029/ns1_box-disk1.vmdk
    55574611    4 drwxrwxr-x  19 dittrich dittrich     4096 Mar 15 14:13 .
    55576829   28 -rw-rw-r--   1 dittrich dittrich    27191 Mar 15 14:13 ./Makefile
    55574612    4 drwxrwxr-x   8 dittrich dittrich     4096 Mar 15 14:13 ./.git
    53870594    4 drwxrwxr-x   5 dittrich dittrich     4096 Mar 15 14:15 /vm/vagrant-run-ns1
    53870676    4 -rw-rw-r--   2 dittrich dittrich        4 Mar 15 14:13 /vm/vagrant-run-ns1/ns1.dims
    53870676    4 -rw-rw-r--   2 dittrich dittrich        4 Mar 15 14:13 /vm/vagrant-run-ns1/ns1.local
    53346306    4 drwxrwxr-x   5 dittrich dittrich     4096 Mar 15 13:51 /vm/devserver
    53347790    4 -rw-rw-r--   1 dittrich dittrich     2756 Mar 15 13:51 /vm/devserver/Vagrantfile

..
