Workbook
=========

The `Lym Workbook <https://github.com/kushaldas/lymworkbook>`_ is an effort
to create a small lab environment for the students to learn various commands
from book. It uses `Vagrant <https://www.vagrantup.com/>`_ to create a new
Virtual Machine (VM) on which you can practice commands, try out different
solutions and solve these problem without having to worry about messing up your
computer. We will slowly add more problems to it.


How to use this workbook?
-------------------------

Installation
~~~~~~~~~~~~

You will need latest `Vagrant <https://www.vagrantup.com/>`_ for this. Install
Vagrant following the steps from the website. On Windows you can use VirtualBox
along with Vagrant.

Getting the code
~~~~~~~~~~~~~~~~

Checkout latest workbook code from github.

::

    git clone https://github.com/kushaldas/lymworkbook
    cd lymworkbook
    vagrant up
    vagrant ssh workbook



The `vagrant up` command will create two vms.


.. note:: In case you manged to delete some configuration inside of the VM(s), you can very easily start from scratch.
         `vagrant destroy` will remove both the VMs, and `vagrant up` again will get them back. You can also destroy
         one particular VM, `vagrant destroy workbook`.

Setting up environment
~~~~~~~~~~~~~~~~~~~~~~

Each problem here has a unique "string" which can be used to setup this new
machine's environment according to that problem's specification, e.g., for
setting up environment for a problem with string `unicorn`, you should do:

::

   sudo lymsetup unicorn

After running the commands and making the changes mentioned in the problem
statement, you should run the following to verify that the changes are as per
the problem requirements:

::

   sudo lymverify unicorn


copy paste
-----------

ID string: `copypaste`

- Create a directory called `work` in your home directory.
- Copy the file `/tmp/problem1/work/files/hello.txt` into this newly created
  directory.
- Remember to remove the `/tmp/problem1/work/files/hello.txt` file afterwards.
- Create a file named `/tmp/chapter1/allusers` and add all of the directory
  names under your home directory into that file.

.. note:: To setup the problem environment, run:

  ::

      sudo lymsetup copypaste

  After performing the steps, verify the result using:

  ::

      sudo lymverify copypaste


Find your user id
------------------

Find your user id and write it down in a file `/tmp/myuserid.txt`.


Creating softlinks
------------------

To setup the problem environment:

::

    sudo lymsetup softlinks


Create a softlink called `docs` in your home directory which will point to
`/usr/share/doc/` directory. Also create another softlink called `memory` to
the `/proc/meminfo` file.


To verify:

::

    sudo lymverify softlinks


Basic vim usage
------------------

To setup the problem environment:

::

    sudo lymsetup basicvim


Read the file at `/etc/os-release` and write the value of ID_LIKE (without the
double quotes) in a file at `/tmp/id_like.txt`.


To verify:

::

    sudo lymverify basicvim



Adding a new user
------------------

To setup the problem environment:

::

    sudo lymsetup newuser


Add a new user called fatima to the system.


To verify:

::

    sudo lymverify newuser


Deleting an existing user
-------------------------

To setup the problem environment, remember to add the user first from the
previous problem.

::

    sudo lymsetup deleteuser


Remove the fatima user from the system.


To verify:

::

    sudo lymverify deleteuser


Finding the IP address of dgplug.org
------------------------------------

Find the IP address of dgplug.org and save it to /tmp/ip_dgplug.txt file.

To verify:

::

    sudo lymverify findip

Change the local timezone of the system
----------------------------------------

Change the timezone of the system to the same of San Francisco, USA.

To verify:

::

    sudo lymverify timezonechange


Add sudo access to an user
---------------------------

Grant administrative(sudo) privileges to an existing normal user account
"lym". Remember to create the user first.


To verify:

::

    sudo lymverify assignsudo
