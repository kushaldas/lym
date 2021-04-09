Package management
==================

In the Free and Open Source Software world, most software is released in source
code format by developers. This means that generally, if you want to install a
piece of software, you will find the source code on the website of the project.
As a user, you will have to find and install all the other bits of software,
that this particular piece depends on (the *dependencies*) and then install the
software. To solve this *painful* issue, all Linux distributions have something
called a *package management system*. Volunteers (mostly) all across the world
help make binary software packages out of source code released by the
developers, in such a way that users of the Linux distribution can easily
install, update or remove that software.

It’s generally recommended, we use the package management system that comes with
the distribution, to install software for the users. If you are really sure
about what you’re doing in the system, you can install from the source files
too; but that can be dangerous.

.. index:: dnf

dnf command
-------------

**dnf** is the package management system in Fedora. The actual packages come in
the *rpm* format. *dnf* helps you search, install or uninstall any package from
the Fedora package repositories. You can also use the same command to update
packages in your system.

Searching for a package
------------------------

::

  $ dnf search pss
  Fedora 25 - x86_64                                                        34 MB/s |  50 MB     00:01    
  Fedora 25 - x86_64 - Updates                                              41 MB/s |  23 MB     00:00    
  Last metadata expiration check: 0:00:07 ago on Sun Jun 25 04:14:22 2017.
  =========================================== N/S Matched: pss ============================================
  pss.noarch : A power-tool for searching inside source code files
  pssh.noarch : Parallel SSH tools

First the tool, downloads all the latest package information from the
repository, and then gives us the result.

Finding more information about a package
-----------------------------------------

*dnf info* gives us more information about any given package.

::

  $ dnf info pss
  Last metadata expiration check: 0:04:59 ago on Sun Jun 25 04:14:22 2017.
  Available Packages
  Name        : pss
  Arch        : noarch
  Epoch       : 0
  Version     : 1.40
  Release     : 6.fc25
  Size        : 58 k
  Repo        : fedora
  Summary     : A power-tool for searching inside source code files
  URL         : https://github.com/eliben/pss
  License     : Public Domain
  Description : pss is a power-tool for searching inside source code files.
              : pss searches recursively within a directory tree, knows which
              : extensions and file names to search and which to ignore, automatically
              : skips directories you wouldn't want to search in (for example .svn or .git),
              : colors its output in a helpful way, and does much more.



.. index:: dnf install

Installing a package
---------------------

The *dnf install* command helps us install any given package. We can pass more
than one package name as the argument.

::

  $ sudo dnf install pss wget
  Last metadata expiration check: 0:37:13 ago on Sun Jun 25 03:44:07 2017.
  Package wget-1.18-3.fc25.x86_64 is already installed, skipping.
  Dependencies resolved.
  =====================================================================================================================================================
  Package                         Arch                               Version                                 Repository                          Size
  =====================================================================================================================================================
  Installing:
  pss                             noarch                             1.40-6.fc25                             fedora                              58 k

  Transaction Summary
  =====================================================================================================================================================
  Install  1 Package

  Total download size: 58 k
  Installed size: 196 k
  Is this ok [y/N]: y
  Downloading Packages:
  pss-1.40-6.fc25.noarch.rpm                                                                                           969 kB/s |  58 kB     00:00    
  -----------------------------------------------------------------------------------------------------------------------------------------------------
  Total                                                                                                                118 kB/s |  58 kB     00:00     
  Running transaction check
  Transaction check succeeded.
  Running transaction test
  Transaction test succeeded.
  Running transaction
    Installing  : pss-1.40-6.fc25.noarch                                                                                                           1/1 
    Verifying   : pss-1.40-6.fc25.noarch                                                                                                           1/1 

  Installed:
    pss.noarch 1.40-6.fc25                                                                                                                             

  Complete!

To list the available updates
-----------------------------

The following command shows all the available updates for your system.

::

        # dnf list updates

        Last metadata expiration check: 0:52:28 ago on Fri 09 Apr 2021 08:51:39 PM IST.
        Available Upgrades
        fedora-gpg-keys.noarch               33-4              updates
        fedora-repos.noarch                  33-4              updates
        fedora-repos-modular.noarch          33-4              updates


.. index:: apt

apt command
-----------

**apt** is the package management system for the *Debian* Linux distribution. As
Ubuntu is downstream of the *Debian* distribution, it also uses the same package
management system.

apt-get update
---------------

::

  $ apt-get update
  ... long output


The **apt-get update** command is used to update all the package information for
the Debian repositories.

apt-get install
----------------

**sudo apt-get install** is the command used to install any given package from
the repository.
