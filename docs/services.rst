Linux Services
===============

This is also a chapter related to the *systemd* tool.

.. index:: bootup

What is a service?
------------------

A service is a process or application which is running in the background, either
doing some predefined task or waiting for some event. If you remember our
process chapter, we learned about *systemd* for the first time there. It is the
first process to run in our system; it then starts all the required processes
and services. To know about how the system boots up, read the **bootup** man
page. `Click here
<https://www.freedesktop.org/software/systemd/man/bootup.html>`_ to read it
online.

::

  $ man bootup

.. index:: daemon

What is a daemon?
------------------

Daemon is the actual term for those long-running background processes. A service
actually consists of one or more daemons.

Make sure that you don't get confused between Daemons and Demons :) Here is a
gem from Internet:

.. figure:: img/demon_hackers.png

What is the init system?
-------------------------

If you look at Unix/Linux history, you will find the first process which starts
up, is also known as *init process*. This process used to start other processes
by using the rc files from */etc/rc.d* directory. In the modern Linux systems,
**systemd** has replaced the init system.


Units in systemd
-----------------

Units are a standardized way for the systemd to manage various parts of a
system. There are different kinds of units, *.service* is for system services,
*.path* for path based ones. There is also *.socket* which are socket based
systemd units. There are various other types, we can learn about those later.


.. index:: services

.service units in systemd
--------------------------

These are service units, which explains how to manage a particular service in
the system. In our daily life, we generally only have to work with these unit
files.


.. index:: systemctl

How to find all the systemd units in the system?
-------------------------------------------------

::

  $ systemctl
  ... long output
    -.mount                                                     loaded active     mounted   /
    boot.mount                                                  loaded active     mounted   /boot
    dev-hugepages.mount                                         loaded active     mounted   Huge Pages File System
    dev-mqueue.mount                                            loaded active     mounted   POSIX Message Queue File System
    home.mount                                                  loaded active     mounted   /home
    proc-fs-nfsd.mount                                          loaded active     mounted   NFSD configuration filesystem
    run-user-1000-doc.mount                                     loaded active     mounted   /run/user/1000/doc
    run-user-1000-gvfs.mount                                    loaded active     mounted   /run/user/1000/gvfs
    run-user-1000.mount                                         loaded active     mounted   /run/user/1000
    run-user-42.mount                                           loaded active     mounted   /run/user/42
  ... long output


In the output of the **systemctl** command, you should be able to see all the
different kinds of units in the system. If you want to see only the service
units, then use the following command.

::

  $ systemctl --type=service


Working with a particular service
----------------------------------

Let us take the *sshd.service* as an example. The service controls the sshd
daemon, which allows us to remotely login to a system using the **ssh** command.

To know the current status of the service, I execute the following command.

::

  $ sudo systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; disabled; vendor preset: enabled)
    Active: inactive (dead)
      Docs: man:sshd(8)
            man:sshd_config(5)

  Jun 19 12:07:29 kdas-laptop sshd[19533]: Accepted password for kdas from 192.168.1.101 port 61361 ssh2
  Jun 20 17:57:53 kdas-laptop sshd[30291]: Connection closed by 192.168.1.101 port 63345 [preauth]
  Jun 20 17:58:02 kdas-laptop sshd[30293]: Accepted password for kdas from 192.168.1.101 port 63351 ssh2
  Jun 20 18:32:11 kdas-laptop sshd[31990]: Connection closed by 192.168.1.101 port 64352 [preauth]
  Jun 20 18:32:17 kdas-laptop sshd[32039]: Accepted password for kdas from 192.168.1.101 port 64355 ssh2
  Jun 20 18:45:57 kdas-laptop sshd[32700]: Accepted password for kdas from 192.168.1.101 port 64824 ssh2
  Jun 21 08:44:39 kdas-laptop sshd[15733]: Accepted password for kdas from 192.168.1.101 port 51574 ssh2
  Jun 22 18:17:24 kdas-laptop systemd[1]: Stopping OpenSSH server daemon...
  Jun 22 18:17:24 kdas-laptop sshd[20932]: Received signal 15; terminating.
  Jun 22 18:17:24 kdas-laptop systemd[1]: Stopped OpenSSH server daemon.


To start the service, I’ll use the following command, and then I can use the
*status* argument to the **systemctl** to check the service status once again.

::

  $ sudo systemctl start sshd
  $ sudo systemctl status sshd
  ● sshd.service - OpenSSH server daemon
    Loaded: loaded (/usr/lib/systemd/system/sshd.service; disabled; vendor preset: enabled)
    Active: active (running) since Thu 2017-06-22 18:19:28 IST; 1s ago
      Docs: man:sshd(8)
            man:sshd_config(5)
  Main PID: 3673 (sshd)
      Tasks: 1 (limit: 4915)
    CGroup: /system.slice/sshd.service
            └─3673 /usr/sbin/sshd -D

  Jun 22 18:19:28 kdas-laptop systemd[1]: Starting OpenSSH server daemon...
  Jun 22 18:19:28 kdas-laptop sshd[3673]: Server listening on 0.0.0.0 port 22.
  Jun 22 18:19:28 kdas-laptop sshd[3673]: Server listening on :: port 22.
  Jun 22 18:19:28 kdas-laptop systemd[1]: Started OpenSSH server daemon.


In the same way, we can use either the *stop* or *restart* arguments to the
**systemctl** command.


Enabling or disabling a service
-------------------------------

Even if you start a service, you’ll find that after you reboot the computer, the
service did not start at the time of boot up. To do so, you will have to enable
the service, or to stop a service from starting at boot, you will have to
disable the service.

.. code-block:: Bash

  $ sudo systemctl enable sshd.service
  Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/lib/systemd/system/sshd.service.
  $ sudo systemctl disable sshd.service
  Removed /etc/systemd/system/multi-user.target.wants/sshd.service.


Shutdown or reboot the system using systemctl
----------------------------------------------

We can also reboot or shutdown the system using the systemctl command.

.. code-block:: Bash

  $ sudo systemctl reboot
  $ sudo systemctl shutdown

.. index:: journalctl

journalctl
-----------

`systemd` runs the **systemd-journald.service**, which stores logs in the journal
from the different services maintained by `systemd`. We use `journalctl`
command to read these log entries from the journal. If you execute the command
without any arguments, it will show you all the log entries starting from the
oldest in the journal. One needs to be `root` to be able to use the
`journalctl` command. Remember that `systemd-journald` stores all the logs in
binary format, means you can not just `less` the files and read them.

If you want any normal user to execute `journalctl` command, then add them into
**systemd-journal** group.

Finding the logs of a service
------------------------------

We can use the **journalctl** command to find the log of a given service. The
general format is *journalctl -u servicename". Like below is the log for *sshd*
service.

.. code-block:: Bash

  $ sudo journalctl -u sshd
  -- Logs begin at Thu 2017-06-22 14:16:45 UTC, end at Fri 2017-06-23 05:21:29 UTC. --
  Jun 22 14:17:39 kushal-test.novalocal systemd[1]: Starting OpenSSH server daemon...
  Jun 22 14:17:39 kushal-test.novalocal systemd[1]: sshd.service: PID file /var/run/sshd.pid not readable (yet?) after start: No such file or directory
  Jun 22 14:17:39 kushal-test.novalocal sshd[827]: Server listening on 0.0.0.0 port 22.
  Jun 22 14:17:39 kushal-test.novalocal sshd[827]: Server listening on :: port 22.
  Jun 22 14:17:39 kushal-test.novalocal systemd[1]: Started OpenSSH server daemon.
  Jun 22 14:22:08 kushal-test.novalocal sshd[863]: Accepted publickey for fedora from 103.249.881.17 port 56124 ssh2: RSA SHA256:lvn4rIszmfB14PBQwh4k9C
  Jun 22 14:29:24 kushal-test.novalocal systemd[1]: Stopping OpenSSH server daemon...
  Jun 22 14:29:24 kushal-test.novalocal sshd[827]: Received signal 15; terminating.
  Jun 22 14:29:24 kushal-test.novalocal systemd[1]: Stopped OpenSSH server daemon.
  Jun 22 14:29:24 kushal-test.novalocal systemd[1]: Starting OpenSSH server daemon...
  Jun 22 14:29:24 kushal-test.novalocal sshd[2164]: Server listening on 0.0.0.0 port 22.
  Jun 22 14:29:24 kushal-test.novalocal sshd[2164]: Server listening on :: port 22.
  Jun 22 14:29:24 kushal-test.novalocal systemd[1]: Started OpenSSH server daemon.
  Jun 22 14:54:26 kushal-test.novalocal sshd[13522]: Invalid user  from 139.162.122.110 port 51012
  Jun 22 14:54:26 kushal-test.novalocal sshd[13522]: input_userauth_request: invalid user  [preauth]
  Jun 22 14:54:26 kushal-test.novalocal sshd[13522]: Failed none for invalid user  from 139.162.122.110 port 51012 ssh2
  Jun 22 14:54:26 kushal-test.novalocal sshd[13522]: Connection closed by 139.162.122.110 port 51012 [preauth]
  Jun 22 15:15:29 kushal-test.novalocal sshd[13541]: Did not receive identification string from 5.153.62.226 port 48677


To view only the last N entries
--------------------------------

You can use the `-n` argument to the `journalctl` command to view only the last
N number of entries. For example, to view the last 10 entries.

.. code-block:: Bash

    # journalctl -n 10


Continuous stream of logs
--------------------------

In case you want to monitor the logs of any service, that is keep reading the
logs in real time, you can use *-f* flag with the *journalctl* command.

.. code-block:: Bash

  $ sudo journalctl -f -u sshd
  -- Logs begin at Thu 2017-06-22 14:16:45 UTC. --
  Jun 23 03:39:09 kushal-test.novalocal sshd[14095]: Did not receive identification string from 158.85.81.118 port 10000
  Jun 23 04:13:32 kushal-test.novalocal sshd[14109]: Received disconnect from 221.194.47.242 port 55028:11:  [preauth]
  Jun 23 04:13:32 kushal-test.novalocal sshd[14109]: Disconnected from 221.194.47.242 port 55028 [preauth]
  Jun 23 04:33:59 kushal-test.novalocal sshd[14115]: Received disconnect from 59.45.175.64 port 36248:11:  [preauth]
  Jun 23 04:36:53 kushal-test.novalocal sshd[14121]: Did not receive identification string from 82.193.122.22 port 58769
  Jun 23 04:42:01 kushal-test.novalocal sshd[14123]: Received disconnect from 221.194.47.233 port 51797:11:  [preauth]
  Jun 23 04:42:01 kushal-test.novalocal sshd[14123]: Disconnected from 221.194.47.233 port 51797 [preauth]
  Jun 23 04:51:46 kushal-test.novalocal sshd[14130]: Did not receive identification string from 191.253.13.227 port 4668
  Jun 23 05:05:16 kushal-test.novalocal sshd[14189]: Received disconnect from 59.45.175.88 port 33737:11:  [preauth]
  Jun 23 05:05:16 kushal-test.novalocal sshd[14189]: Disconnected from 59.45.175.88 port 33737 [preauth]


I can see that someone was trying to break into this VM by trying random ports
:)

Listing of previous boots
--------------------------

In systems like Fedora, **journalctl** by default keeps history from past boots.
To know about all available boot history, type the following command.

.. code-block:: Bash

  $ sudo journalctl --list-boots
  [sudo] password for fedora: 
  -112 7a88e13a76434a1199f82ad90441ae7f Tue 2014-12-09 03:41:08 IST—Tue 2014-12-09 03:41:08 IST
  -111 b86086ed59b84b228e74f91ab08a66b3 Sun 2015-06-28 23:54:26 IST—Sun 2015-07-12 07:27:48 IST
  -110 71d3f6024f514653bfd2574243d096d1 Sun 2016-06-05 01:51:05 IST—Sun 2016-06-05 01:51:16 IST
  -109 b7721878a5144d009418cf269b5eea71 Fri 2016-08-19 19:47:57 IST—Sat 2016-08-20 01:16:07 IST
  -108 6102102fc7804379b888d83cea66838b Sat 2016-08-20 01:21:36 IST—Sun 2016-08-21 00:05:38 IST
  ... long output


To know about any particular boot log, you can use the hash along with *-b* flag
to the **journalctl** command.

.. code-block:: Bash

  $ sudo journalctl -b 7a88e13a76434a1199f82ad90441ae7f
  -- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 13:40:49 IST. --
  Dec 09 03:41:08 localhost.localdomain systemd[1344]: Stopping Default.
  Dec 09 03:41:08 localhost.localdomain systemd[1344]: Stopped target Default.
  Dec 09 03:41:08 localhost.localdomain systemd[1344]: Starting Shutdown.
  Dec 09 03:41:08 localhost.localdomain systemd[1344]: Reached target Shutdown.
  Dec 09 03:41:08 localhost.localdomain systemd[1344]: Starting Exit the Session..


Time-based log viewing
-----------------------

We can also use **journalctl** to view logs for a certain time period. For
example, if we want to see all the logs since yesterday, we can use the
following command.

.. code-block:: Bash

  $ sudo journalctl --since yesterday
  [sudo] password for fedora: 
  -- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 15:21:54 IST. --
  Jun 23 00:00:00 kushal-test.novalocal /usr/libexec/gdm-x-session[28622]: (evolution-alarm-notify:11609): evolution-alarm-notify-WARNING **: alarm.c:253: Reques
  Jun 23 00:01:01 kushal-test.novalocal CROND[22327]: (root) CMD (run-parts /etc/cron.hourly)
  ... long output


You can also use date time following *YYYY-MM-DD HH:MM:SS* format.

.. code-block:: Bash

  $ sudo journalctl --since "2015-11-10 14:00:00"
  -- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 15:25:30 IST. --
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Timers.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Paths.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Starting D-Bus User Message Bus Socket.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Listening on D-Bus User Message Bus Socket.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Sockets.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Basic System.
  Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Default.

Total size of the journal logs
-------------------------------

Use the `--disk-usage` flag to find out total amount entries and archive can be
stored. The following is the output from an Ubuntu Focal (20.04) system.

.. code-block:: Bash

    # journalctl --disk-usage
    Archived and active journals take up 56.0M in the file system.


Writing your own service file
------------------------------

In case you are developing a new service, or you want to run some script as a
systemd service, you will have to write a service file for the same.

For this example, we will add a new executable under `/usr/sbin` called
`myserver`. We will use Python3's builtin `http.server` module to expose the
current directory as a simple server.

.. code-block:: bash

    #!/usr/bin/sh
    python3 -m http.server 80


.. note:: Remember to use `chmod` command to mark the file as executable.


Next, we will create the actual service file, which is configuration file written in `INI format <https://en.wikipedia.org/wiki/INI_file>`_.


Add the following to the `/etc/systemd/system/myserver.service` file.

.. code-block:: ini

    [Unit]
    Description=My Web Server
    After=network.target

    [Service]
    Type=simple
    WorkingDirectory=/web/amazing
    ExecStart=/usr/sbin/myserver
    Restart=always

    [Install]
    WantedBy=multi-user.target

Using `ExecStart` we are specifying which executable to use, we can even pass
any command line argument to that. We are also mentioning the directory this
service will start. We are also saying in case of a failure, or unclean exit
code, or or on signal `always` restart the service.  The service will also start
after the `network` is up. We are also mentioning that the service will have
`/web/amazing` as the working directory. So, we should also create that.
We also add an `index.html` to the same directory for testing.

.. code-block:: bash

    # mkdir -p /web/amazing
    # echo "Hello from Python." > /web/amazing/index.html


To understand the other options you will have to read two man pages,

- `man systemd.service <https://www.freedesktop.org/software/systemd/man/systemd.service.html>`_
- `man systemd.unit <https://www.freedesktop.org/software/systemd/man/systemd.unit.html>`_.

After adding the service file, reload the daemons.

.. code-block:: bash

    # systemctl daemon-reload

Unless you do this, `systemd` will complain to you that something changed, and
you did not reloadl. Always remember to reload after you make any changes to any
systemd service files.


Now we can start and enable the service.

.. code-block:: bash


    # systemctl enable myserver
    # systemctl start myserver
    # systemctl status myserver
    ● myserver.service - My Web Server
      Loaded: loaded (/etc/systemd/system/myserver.service; enabled; vendor preset: disabled)
      Active: active (running) since Sat 2022-03-12 10:03:25 UTC; 1 day 3h ago
    Main PID: 21019 (myserver)
        Tasks: 2 (limit: 50586)
      Memory: 9.6M
      CGroup: /system.slice/myserver.service
              ├─21019 /usr/bin/sh /usr/sbin/myserver
              └─21020 python3 -m http.server 80

    Mar 12 10:03:25 selinux systemd[1]: Started My Web Server.


Verifying the service
~~~~~~~~~~~~~~~~~~~~~

We can verify that our new webservice is running properly via `curl`.


::

    $ curl http://localhost/
    Hello from Python.


Securing a service using systemd
=================================

systemd provides multiple features which can allow anyone to lock down a
particular service. These features help to migiate multiple security issues for
any service. We will learn about a few of those in this section.


Now, we will use an web application I developed to learn.  It is called `verybad
<https://github.com/kushaldas/verybad>`_.  This has multiple security issues, so
do not run it in your laptop or main system. If you want try it out and follow
along the steps, please do it in a VM.

.. note:: All of the following steps are done in a `Fedora 35 <https://getfedora.org>`_  virtual machine.


Installing verybad service
---------------------------

We will first install latest `Rust <https://www.rust-lang.org/>`_ and compile from source.

::

    $ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ...
    Rust is installed now. Great!

    To get started you may need to restart your current shell.
    This would reload your PATH environment variable to include
    Cargo's bin directory ($HOME/.cargo/bin).

    To configure your current shell, run:
    source $HOME/.cargo/env
    To get started you may need to restart your current shell.
    This would reload your PATH environment variable to include
    Cargo's bin directory ($HOME/.cargo/bin).

    To configure your current shell, run:
    source $HOME/.cargo/env

Press enter for the default configuration. You can logout and log back in to
make sure that correct environment varibles are being used.


Next, installing `gcc` and then clone the git repository and compile.

::

    $ sudo dnf install gcc -y
    $ git clone https://github.com/kushaldas/verybad
    $ cd verybad
    $ cargo build --release
    ...
    $ ls -l target/release/verybad
    -rwxrwxr-x. 2 almalinux almalinux 8193040 Mar 16 04:21 target/release/verybad


Now, we will copy the executable to `/usr/sbin` and also copy the service file and then start & enable the service.


::

    $ sudo cp ./target/release/verybad /usr/sbin/
    $ sudo cp verybad.service /etc/systemd/system/
    $ sudo mkdir -p /web/amazing
    $ sudo systemctl enable verybad
    $ sudo systemctl start verybad


Now, in one terminal you can run `journalctl` to see the logs from the service,
and then from another terminal we can use `curl` to do various operations on the
web service.

::

    $ sudo journalctl -u verybad -f


Vulnerabilities in the application
-----------------------------------

The `verybad` application contains multiple diffrent security vulnerabilities. You can see all the available API if you 
do a GET request to the root of the application.

::

  $ curl http://localhost:8000/
  Example of poorly written code.
      GET /getos -> will give the details of the OS.
      GET /filename -> will provide a file from the current directory
      GET /exec/date -> will give you the current date & time in the server.
      POST /filename -> Saves the data in filename.


We can get the details of the Operating system via `/getos` call, which
internally returning us the content of the `/etc/os-release` file.

::

  $ curl http://localhost:8000/getos
  NAME="Fedora Linux"
  VERSION="35 (Cloud Edition)"
  ID=fedora
  VERSION_ID=35
  VERSION_CODENAME=""
  PLATFORM_ID="platform:f35"
  PRETTY_NAME="Fedora Linux 35 (Cloud Edition)"
  ANSI_COLOR="0;38;2;60;110;180"
  LOGO=fedora-logo-icon
  CPE_NAME="cpe:/o:fedoraproject:fedora:35"
  HOME_URL="https://fedoraproject.org/"
  DOCUMENTATION_URL="https://docs.fedoraproject.org/en-US/fedora/f35/system-administrators-guide/"
  SUPPORT_URL="https://ask.fedoraproject.org/"
  BUG_REPORT_URL="https://bugzilla.redhat.com/"
  REDHAT_BUGZILLA_PRODUCT="Fedora"
  REDHAT_BUGZILLA_PRODUCT_VERSION=35
  REDHAT_SUPPORT_PRODUCT="Fedora"
  REDHAT_SUPPORT_PRODUCT_VERSION=35
  PRIVACY_POLICY_URL="https://fedoraproject.org/wiki/Legal:PrivacyPolicy"
  VARIANT="Cloud Edition"
  VARIANT_ID=cloud


Directory traversal vulnerability
----------------------------------

`Directory traversal
<https://portswigger.net/web-security/file-path-traversal>`_ is first
vulnerability we are going to look into. A **GET** request to `/filename` will give us the file. Let us read the `/etc/shadow`
file using this.

::

  $ curl http://localhost:8000/%2Fetc%2Fshadow
  root:!locked::0:99999:7:::
  bin:*:18831:0:99999:7:::
  daemon:*:18831:0:99999:7:::
  adm:*:18831:0:99999:7:::
  lp:*:18831:0:99999:7:::
  sync:*:18831:0:99999:7:::
  shutdown:*:18831:0:99999:7:::
  halt:*:18831:0:99999:7:::
  mail:*:18831:0:99999:7:::
  operator:*:18831:0:99999:7:::
  games:*:18831:0:99999:7:::
  ftp:*:18831:0:99999:7:::
  nobody:*:18831:0:99999:7:::
  dbus:!!:18926::::::
  systemd-network:!*:18926::::::
  systemd-oom:!*:18926::::::
  systemd-resolve:!*:18926::::::
  systemd-timesync:!*:18926::::::
  systemd-coredump:!*:18926::::::
  tss:!!:18926::::::
  unbound:!!:18926::::::
  sshd:!!:18926::::::
  chrony:!!:18926::::::
  fedora:!!:19069:0:99999:7:::
  polkitd:!!:19069::::::

In this system we don't have any password set, but in case we had any password,
the attacker can retrive that. The attacker can read any file in the system,
thus enables them to read the configuration or password details for any other
application running in the same system.


Arbitary file write vulnerability
---------------------------------

The attacker can also write to any file using **POST** request to `/filename`
API endpoint. Thus they can add new user, add any password for a given user (via
`/etc/shadow`). They can enable `ssh` access for the `root` user (via `/etc/ssh/sshd_config`).

In the below example we are rewriting the `/etc/shadow` file with a known
password for `root`. We prefilled the password part in the `local_shadow` file
for `root`.

::

  $ cat local_shadow
  root:$y$j9T$Ezgqn2AUuaBBQ25pABCoj/$QU3CfSAX4aLmb6mcZAqmMg4ZvEgGZpdjW632qsDtXX3:19075:0:99999:7:::
  bin:*:18831:0:99999:7:::
  daemon:*:18831:0:99999:7:::
  adm:*:18831:0:99999:7:::
  lp:*:18831:0:99999:7:::
  sync:*:18831:0:99999:7:::
  shutdown:*:18831:0:99999:7:::
  halt:*:18831:0:99999:7:::
  mail:*:18831:0:99999:7:::
  operator:*:18831:0:99999:7:::
  games:*:18831:0:99999:7:::
  ftp:*:18831:0:99999:7:::
  nobody:*:18831:0:99999:7:::
  dbus:!!:18926::::::
  systemd-network:!*:18926::::::
  systemd-oom:!*:18926::::::
  systemd-resolve:!*:18926::::::
  systemd-timesync:!*:18926::::::
  systemd-coredump:!*:18926::::::
  tss:!!:18926::::::
  unbound:!!:18926::::::
  sshd:!!:18926::::::
  chrony:!!:18926::::::
  fedora:!!:19069:0:99999:7:::
  polkitd:!!:19069::::::

  $ curl --data-binary @local_shadow http://localhost:8000/%2Fetc%2Fshadow
  Okay[fedora@selinux3 ~]$ su -
  Password: 
  Last login: Thu Mar 24 12:27:44 UTC 2022 on pts/2
  [root@selinux3 ~]#

You can see that after overwriting the `/etc/shadow` file, we could just use
`password` as root's password :) To pass `/` in the URL we had to URL encode it,
which is `%2F`.

Remote code execution (RCE) vulnerability
------------------------------------------

You can execute any command on the system using **GET** request to
`/exec/<command>` API. For example, we can see the contents of `/root/`
directory via `ls` command. Once again we URL encoded the command and the
argument. You can thus create a reverse shell, or any kind of damage as we are
running the service as `root` by default.

::

  $ curl http://localhost:8000/exec/id
  uid=0(root) gid=0(root) groups=0(root) context=system_u:system_r:unconfined_service_t:s0
  $ curl  http://localhost:8000/exec/ls%20%2Froot
  anaconda-ks.cfg
  original-ks.cfg

Through out rest of the chapter, we will learn how to migiate these 3 kinds of
vulnerabilities using systemd's builtin features.

Remove access to system's tmp directory
----------------------------------------

One of the very initial thing we can do is to provide a private temporary
directory structure only to the service. If we set `PrivateTmp=yes`, it will
create a new file system namespace for the service & will mount private `/tmp` &
`/var/tmp` inside of it. This option is only available for system services.

Only using `PrivateTmp` does not provide special security, but it stops the
chances where the service can write to a temporary file/socket created by
another service.

Protecting home dirctories
---------------------------

We can also use `ProtectHome=` to secure home directories in the system. It is
set to true like `ProtectHome=yes`, then `/root`, `/home` & `/run/user` are
empty & inaccessible. We can also set them as read only by doing
`ProtectHome=read-only`. The third available option is `tmpfs`, which mounts
temporary filesytem to those directories. For any long running service, we must
enable this feature.


Let us see how this affects our service. First we update the service file.

.. code-block:: ini

  [Unit]
  Description=Very Bad Web Application
  After=network.target

  [Service]
  Type=simple
  WorkingDirectory=/web/amazing
  ExecStart=/usr/sbin/verybad
  Restart=always
  ProtectHome=yes
  PrivateTmp=yes

  [Install]
  WantedBy=multi-user.target


We will have to reload the daemon & restart the service for the changes in effect.

::

  # systemctl daemon-reload
  # systemctl restart verybad


Now we will try to execute `ls` command against `/root`, `/home/fedora` & `/tmp` directory.
You will notice that the tool can not see any file in those directories.


::

  $ curl  http://localhost:8000/exec/ls%20%2Froot
  $ curl  http://localhost:8000/exec/ls%20%2Fhome%2Ffedora
  $ curl  http://localhost:8000/exec/ls%20%2Ftmp


