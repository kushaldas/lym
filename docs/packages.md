## Package management

In the Free and Open Source Software world, most of the software is being released in source code format by the developers. This means generally if you want to install a software, you will the source code in the website of the project. As a user, you will have to find all the dependencies and then install the software. To solve this issue, all the Linux distributions have something called *package management system*. There volunteers (mostly) all across the world who helps to make binary software packages out of the source code released by the developers, in such a way that users of the Linux distribution can easily install, update or remove that software.

It is generally recommend to the use the distribution provided software using the package management system for the users. If you are very much sure about what steps you are doing in the system, you can install from the source files too, but that can be dangerous.

```eval_rst
.. index:: dnf
```
### dnf command

*dnf* is the package management system in Fedora. The actual packages come in *rpm* format. *dnf* can help you to search, install or uninstall any package from the Fedora package repositories. You can also use the same command to update the packages in your system.

### Searching any package

```
$ dnf search pss
Fedora 25 - x86_64                                                        34 MB/s |  50 MB     00:01    
Fedora 25 - x86_64 - Updates                                              41 MB/s |  23 MB     00:00    
Last metadata expiration check: 0:00:07 ago on Sun Jun 25 04:14:22 2017.
=========================================== N/S Matched: pss ============================================
pss.noarch : A power-tool for searching inside source code files
pssh.noarch : Parallel SSH tools
```
First the tool downloads all the latest package information from the repository, and then gives us the result.

### Finding more information about a package

*dnf info* gives us more information about any given packages.

```
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
```

```eval_rst
.. index:: dnf install
```

### Installing a package

*dnf install* command helps us to install any given package. We can pass more than one package name as the argument.

```
sudo dnf install pss wget
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
```

### apt command

*apt* is the package management system for *Debian* Linux distribution. As Ubuntu is a downstream of the *Debian* distribution, it also uses the same package management system.

### apt-get update

```
$ apt-get update
... long output
```

*apt-get update* command is used to update all the package information for the Debian repositories.

### apt-get install

*sudo apt-get install* command is used to install any given package from the repository.