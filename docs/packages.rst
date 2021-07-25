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

To list all security updates
-----------------------------

`dnf` can also tell you about all the updates which are marked as security updates.

::

    # dnf updateinfo list --security
    Last metadata expiration check: 2:06:38 ago on Sun 25 Jul 2021 03:44:47 AM UTC.
    FEDORA-2021-83fdddca0f Moderate/Sec.  curl-7.76.1-7.fc34.x86_64
    FEDORA-2021-08cdb4dc34 Important/Sec. dhcp-client-12:4.4.2-11.b1.fc34.x86_64
    FEDORA-2021-08cdb4dc34 Important/Sec. dhcp-common-12:4.4.2-11.b1.fc34.noarch
    FEDORA-2021-e14e86e40e Moderate/Sec.  glibc-2.33-20.fc34.x86_64
    FEDORA-2021-e14e86e40e Moderate/Sec.  glibc-common-2.33-20.fc34.x86_64
    FEDORA-2021-e14e86e40e Moderate/Sec.  glibc-doc-2.33-20.fc34.noarch
    FEDORA-2021-e14e86e40e Moderate/Sec.  glibc-langpack-en-2.33-20.fc34.x86_64
    FEDORA-2021-07dc0b3eb1 Critical/Sec.  kernel-core-5.13.4-200.fc34.x86_64
    FEDORA-2021-8b25e4642f Low/Sec.       krb5-libs-1.19.1-14.fc34.x86_64
    FEDORA-2021-83fdddca0f Moderate/Sec.  libcurl-7.76.1-7.fc34.x86_64
    FEDORA-2021-31fdc84207 Moderate/Sec.  libgcrypt-1.9.3-3.fc34.x86_64
    FEDORA-2021-2443b22fa0 Moderate/Sec.  linux-firmware-20210716-121.fc34.noarch
    FEDORA-2021-2443b22fa0 Moderate/Sec.  linux-firmware-whence-20210716-121.fc34.noarch
    FEDORA-2021-d1fc0b9d32 Moderate/Sec.  nettle-3.7.3-1.fc34.x86_64
    FEDORA-2021-0ec5a8a74b Important/Sec. polkit-libs-0.117-3.fc34.1.x86_64
    FEDORA-2021-a6bde7ab18 Moderate/Sec.  python3-urllib3-1.25.10-5.fc34.noarch


.. index:: apt

apt command
-----------

**apt** is the package management system for the *Debian* Linux distribution. As
Ubuntu is downstream of the *Debian* distribution, it also uses the same package
management system.

apt update
-----------

::

  # apt update
  ... long output


The **apt update** command is used to update all the package information for
the Debian repositories.

Installing a package via apt
-----------------------------

`apt install packagename` is the command used to install any given package from
the repository.

::

    # apt install htop
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Suggested packages:
      lsof strace
    The following NEW packages will be installed:
      htop
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    Need to get 92.8 kB of archives.
    After this operation, 230 kB of additional disk space will be used.
    Get:1 http://deb.debian.org/debian buster/main amd64 htop amd64 2.2.0-1+b1 [92.8 kB]
    Fetched 92.8 kB in 1s (113 kB/s)
    debconf: delaying package configuration, since apt-utils is not installed
    Selecting previously unselected package htop.
    (Reading database ... 6677 files and directories currently installed.)
    Preparing to unpack .../htop_2.2.0-1+b1_amd64.deb ...
    Unpacking htop (2.2.0-1+b1) ...
    Setting up htop (2.2.0-1+b1) ...

apt-cache search
-----------------

After you updated the cache, you can search for any package. Say, we want to search
the packge `neomutt`.

::

    # apt-cache search neomutt
    neomutt - command line mail reader based on Mutt, with added features

To know the exact policy (from where it will installed/upgrade or which version etc),
you can use the following command.

::

    # apt-cache policy libudev1
    libudev1:
      Installed: 241-7~deb10u7
      Candidate: 241-7~deb10u8
      Version table:
         241-7~deb10u8 500
            500 http://security.debian.org/debian-security buster/updates/main amd64 Packages
     *** 241-7~deb10u7 500
            500 http://deb.debian.org/debian buster/main amd64 Packages
            100 /var/lib/dpkg/status


Listing upgrades
-----------------

You can use `apt list --upgradable` to list all the packages that have updates in the repositories.

::

    # apt list --upgradable
    Listing... Done
    libsystemd0/stable 241-7~deb10u8 amd64 [upgradable from: 241-7~deb10u7]
    libudev1/stable 241-7~deb10u8 amd64 [upgradable from: 241-7~deb10u7]


Upgrading packages
------------------

Use `apt dist-upgrade` to upgrade all the packages to the latest from the repositories.

::

    # apt dist-upgrade 
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Calculating upgrade... Done
    The following packages will be upgraded:
      libsystemd0 libudev1
    2 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
    Need to get 483 kB of archives.
    After this operation, 0 B of additional disk space will be used.
    Do you want to continue? [Y/n] Y
    Get:1 http://security.debian.org/debian-security buster/updates/main amd64 libsystemd0 amd64 241-7~deb10u8 [331 kB]
    Get:2 http://security.debian.org/debian-security buster/updates/main amd64 libudev1 amd64 241-7~deb10u8 [151 kB]
    Fetched 483 kB in 1s (379 kB/s)  
    debconf: delaying package configuration, since apt-utils is not installed
    (Reading database ... 6677 files and directories currently installed.)
    Preparing to unpack .../libsystemd0_241-7~deb10u8_amd64.deb ...
    Unpacking libsystemd0:amd64 (241-7~deb10u8) over (241-7~deb10u7) ...
    Setting up libsystemd0:amd64 (241-7~deb10u8) ...
    (Reading database ... 6677 files and directories currently installed.)
    Preparing to unpack .../libudev1_241-7~deb10u8_amd64.deb ...
    Unpacking libudev1:amd64 (241-7~deb10u8) over (241-7~deb10u7) ...
    Setting up libudev1:amd64 (241-7~deb10u8) ...
    Processing triggers for libc-bin (2.28-10) ...

Listing available security updates in Debian systems
-----------------------------------------------------

We can use the Debian Security Analyzer, **debsecan** tool for this. You have
to install it via `apt` first. In the following example, we are checking system
(running Debian Buster) against the available updates for security updates.

::

    # apt install debsecan
    # debsecan --suite buster --format packages --only-fixed
    apache2-bin
    firefox-esr
    libnss-myhostname
    libnss-systemd
    libpam-systemd
    libsystemd0
    libudev1
    linux-libc-dev
    systemd
    systemd-sysv
    udev

