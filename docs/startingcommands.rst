Shell commands
===============

Linux shell or the terminal is the lifeline of the developers, and of any
power user. Things which can be done on the GUI (by clicking on different
buttons), can be done much efficiently on the terminal by using commands. One
can not remember all the commands, but with regular usage one can easily
remember the most useful ones.

The following guide will introduce you to some basic minimal commands
required to use your Linux computer efficiently.

Gnome Terminal
---------------

.. figure:: img/terminal1.png
   :width: 600px
   :align: center

The above is the screenshot of the Gnome terminal application. As you can see
the command prompt contains these following information::

    [username@hostname directoryname]

In our case the username is *babai*, hostname is *kdas-laptop*, and directory
is mentioned as *~*. This *~* is a special character in our case. It means
the home directory of the user. In our case the home directory path is
*/home/babai/*.

date command
-------------

*date* command tells the current date time.

::

    $ date
    Sun Jun 25 10:13:44 IST 2017

cal command
------------

*cal* command is used to display calendar in your shell, by default it
will display the current month

::

    $ cal
          June 2017     
    Su Mo Tu We Th Fr Sa
                1  2  3 
    4  5  6  7  8  9 10 
    11 12 13 14 15 16 17 
    18 19 20 21 22 23 24 
    25 26 27 28 29 30    

    $ cal 07 2017
        July 2017     
    Su Mo Tu We Th Fr Sa
                    1 
    2  3  4  5  6  7  8 
    9 10 11 12 13 14 15 
    16 17 18 19 20 21 22 
    23 24 25 26 27 28 29 
    30 31                



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

Here you can see that first we moved to */tmp* directory, and then we moved
back to the home directory by using
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

We use *ls* command to view the files and directories inside any given
directory. If you use *ls* command without any argument, then it will work on
the current directory. We will see few examples of the command below.::

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

In the last two commands we provided a path as the argument to the *ls*
command. */* is a special directory, which represents root directory in Linux
filesystem. You will know more in the next chapter.

mkdir command
-------------

We can create new directories using *mkdir* command. For our example we will
create a *code* directory in our home directory.::

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

*rm* command is used to remove a file, or directory. The -rf option is being
*used to remove in a recursive way.
But, always double check before you use *rm -rf* command, if you by mistake
give this command in your home directory, or any other important directory,
it will not ask to confirm, but it will delete everything there.
*-f* stands for force, it will just delete everything. So, please be careful
*and read twice before pressing enter key.

::

    [babai@kdas-laptop ~]$ rm -rf dir1/dir2/dir3
    [babai@kdas-laptop ~]$ ls dir1/ dir1/dir2/ 
    dir1/:
    dir2

    dir1/dir2/:

Coping a file using cp command
-------------------------------

We use *cp* command to copy a file in the Linux shell. To copy
recursively use the *-r* flag to the *cp* command. We use the command
as *cp file_to_copy new_location* format.  In the example below, we
are copying the *hello.txt* to *hello2.txt*.

::

    $ cp hello.txt hello2.txt
    $ ls -l
    -rw-rw-r--. 1 fedora fedora   75 Jun 25 04:47 hello2.txt
    -rw-rw-r--. 1 fedora fedora   75 Jun 25 04:33 hello.txt

In another example, I will copy the file *passwordauthno.png* from the
Pictures directory under my home directory to the current directory.

::

    $ cp ~/Pictures/passwordauthno.png .


In the following example, I will be copying the *images* directory
(and everything inside it) from the *Downloads* directory under home
to the */tmp/* directory.

::

    $ cp -r ~/Downloads/images /tmp/

Renaming or moving a file
--------------------------

*mv* command is used to rename or move a file or directory.

::

    $ mv hello.txt nothello.txt
    $ ls -l nothello.txt
    -rw-rw-r--. 1 fedora fedora 75 Jun 25 04:33 nothello.txt

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


wc command
-----------

*wc* is an useful command which can help us to count newline, word and bytes
of a file.

::

    $ cat hello.txt
    HI that is a file.
    This is the second line.
    And we also have a third line.
    $ wc -l hello.txt
    3 hello.txt
    $ wc -w hello.txt
    17 hello.txt

The *-l* flag finds the number of line in a file, *-w* counts the number
of words in the file.

echo command
-------------

*echo* command echos any given string to the display.

::

    $ echo "Hello"
    Hello

Redirecting the command output
-------------------------------

In Linux shells, we can redirect the command output to a file, or as input to
another command. *|* is the most common way to do so. Using this we can now
count the number of directories in the root (*/*) directory very easily.

::

    $ ls /
    bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
    $ ls / | wc -w
    20

Using > to redirect output to a file
------------------------------------

We can use *>* to redirect the output of one command to a file, if the file
exists this will remove the old content and only keep the input. We can use
*>>* to append to a file, means it will keep all the old content, and
it will add the new input to the end of the file.

::

    $ ls / > details.txt
    $ cat details.txt 
    bin
    boot
    dev
    etc
    home
    lib
    lib64
    lost+found
    media
    mnt
    opt
    proc
    root
    run
    sbin
    srv
    sys
    tmp
    usr
    var
    $ ls /usr/ > details.txt 
    $ cat details.txt 
    bin
    games
    include
    lib
    lib64
    libexec
    local
    sbin
    share
    src
    tmp
    $ ls -l /tmp/ >> details.txt 
    $ cat details.txt 
    bin
    games
    include
    lib
    lib64
    libexec
    local
    sbin
    share
    src
    tmp
    total 776
    -rwxrwxr-x. 1 fedora fedora     34 Jun 24 07:56 helol.py
    -rw-------. 1 fedora fedora 784756 Jun 23 10:49 tmp3lDEho


### man pages

*man* shows the system's manual pages. This is the command we use to
view the help document (manual page) for any command. The man pages are
organized based on *sections*, and if the same command is found in many
different sections, only the first one is shown.

The general syntax is *man section command*.

You can know about different sections below. Press *q* to quit the program.

```
1      1   Executable programs or shell commands
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions eg /etc/passwd
       6   Games
       7   Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
       8   System administration commands (usually only for root)
       9   Kernel routines [Non standard]
```
