Shell commands
===============

Linux shell or the terminal is the lifeline of the developers, and of any power user. Things
which can be done on the GUI (by clicking on different buttons), can be done much efficiently
on the terminal by using commands. One can not remember all the commands, but with regular usage
one can easily remember the most useful ones.

The following guide will introduce you to some basic minimal commands required to use your Linux
computer efficiently.

Gnome Terminal
---------------

.. figure:: img/terminal1.png
   :width: 600px
   :align: center

The above is the screenshot of the Gnome terminal application. As you can see the command prompt contains
these following information::


    [username@hostname directoryname]

In our case the username is *babai*, hostname is *kdas-laptop*, and directory is mentioned as *~*. This *~*
is a special character in our case. It means the home directory of the user. In our case the home directory path
is */home/babai/*.

whoami command
---------------

*whoami* command will tell you which user account you are using in this system.

::

    $ whoami
    fedora

id command
-----------

*id* prints real user id, and various other details related to the account.

::

    $ id
    uid=1000(fedora) gid=1000(fedora) groups=1000(fedora),4(adm),10(wheel),190(systemd-journal) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

pwd command
------------

*pwd* command will help you to find out the current directory. Let us see an example below:
::

    [babai@kdas-laptop ~]$ pwd
    /home/babai

cd command
----------

The next command we will learn is *cd*. This command will help you to change your current directory. We will move
to */tmp* directory in our example.::

    [babai@kdas-laptop ~]$ cd /tmp
    [babai@kdas-laptop tmp]$ pwd
    /tmp
    [babai@kdas-laptop tmp]$ cd ~
    [babai@kdas-laptop ~]$ pwd
    /home/babai

Here you can see that first we moved to */tmp* directory, and then we moved back to the home directory by using
*~* character.

. and ..
----------

*.* and *..* has special meaning in the Linux. *.* means the current
directory and *..* means the parent directory. We can use these in various
situations for daily activities.

::

    $ cd ..

The above command moves to the parent directory.

ls command
----------

We use *ls* command to view the files and directories inside any given directory. If you use *ls* command
without any argument, then it will work on the current directory. We will see few examples of the command
below.::

    [babai@kdas-laptop ~]$ ls
    Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
    [babai@kdas-laptop ~]$ ls /tmp/
    cpython           systemd-private-759094c89c594c07a90156139ec4b969-colord.service-hwU1hR
    hogsuspend        systemd-private-759094c89c594c07a90156139ec4b969-rtkit-daemon.service-AwylGa
    hsperfdata_babai  tracker-extract-files.1000
    plugtmp           tracker-extract-files.1002
    [babai@kdas-laptop ~]$ ls /
    bin   cpython  etc   lib    lost+found  mnt  proc  run   srv  sysroot  usr
    boot  dev      home  lib64  media       opt  root  sbin  sys  tmp      var

In the last two commands we provided a path as the argument to the *ls* command. */* is a special
directory, which represents root directory in Linux filesystem. You will know more in the next chapter.

mkdir command
-------------

We can create new directories using *mkdir* command. For our example we will create a *code* directory
in our home directory.::

    [babai@kdas-laptop ~]$ mkdir code
    [babai@kdas-laptop ~]$ ls
    code  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos

We can also create directories in a recursive way using -p option.::

    [babai@kdas-laptop ~]$ mkdir -p dir1/dir2/dir3
    [babai@kdas-laptop ~]$ ls dir1/ dir1/dir2/ 
    dir1/:
    dir2

    dir1/dir2/:
    dir3

rm command
----------

*rm* command is used to remove a file, or directory. The -rf option is being used to remove in a recursive way.::

    [babai@kdas-laptop ~]$ rm -rf dir1/dir2/dir3
    [babai@kdas-laptop ~]$ ls dir1/ dir1/dir2/ 
    dir1/:
    dir2

    dir1/dir2/:

tree command
-------------

*tree* command prints the directory structure in a nice visual tree design way.::

    [babai@kdas-laptop ~]$ tree
    .
    ├── code
    ├── Desktop
    ├── dir1
    │   └── dir2
    ├── Documents
    ├── Downloads
    ├── Music
    ├── Pictures
    │   └── terminal1.png
    ├── Public
    ├── Templates
    └── Videos