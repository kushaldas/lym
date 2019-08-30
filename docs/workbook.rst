Workbook
=========

The `Lym Workbook <https://github.com/kushaldas/lymworkbook>`_ is  an effort
to create a small lab environment for the students to learn various commands
from book. We will slowly add more problems to it.


How to install the workbook?
-----------------------------

You will need latest `Vagrant <https://www.vagrantup.com/>`_ for this. Install
Vagrant following the steps from the website. We suggest you to use the
libvirt provider along with it.

Then checkout latest workbook code from github.

::

    git clone https://github.com/kushaldas/lymworkbook
    cd lymworkbook
    vagrant up
    vagrant ssh
    cd lymworkbook
    sudo python setup.py install


copy paste
-----------

To setup the problem environment

::

    sudo lymsetup copypaste


Create a directory called `work` in your home directory, and copy the file
from `/tmp/problem1/work/files/hello.txt` into the newly created diretory.
Remember to remove the `/tmp/problem1/work/files/hello.txt` file afterwards.
Create a file called `/tmp/chapter1/allusers` and add all of the directory
names under your home directory into that file.


To verify

::

    sudo lymverify copypaste


Find your user id
------------------

To setup the problem environment

::

    sudo lymsetup findid


Find your user id and write it down in a file `/tmp/myuserid.txt`.


To verify

::

    sudo lymverify findid


Creating softlinks
------------------

To setup the problem environment

::

    sudo lymsetup softlinks


Create a softlink called `docs` in your home directory which will point to
`/usr/share/doc/` directory. Also create another softlink called `meomory` to
the `/proc/meminfo` file.


To verify

::

    sudo lymverify softlinks


Basic vim usage
------------------

To setup the problem environment

::

    sudo lymsetup basicvim


Read the file at `/etc/os-release` and write the value of ID_LIKE (without the
double quotes) in a file at `/tmp/id_like.txt`.


To verify

::

    sudo lymverify basicvim



Adding a new user
------------------

To setup the problem environment

::

    sudo lymsetup newuser


Add a new user called fatima to the system.


To verify

::

    sudo lymverify newuser


Deleing an user
------------------

To setup the problem environment

::

    sudo lymsetup deleteuser


Remove the fatima user from the system


To verify

::

    sudo lymverify deleteuser
