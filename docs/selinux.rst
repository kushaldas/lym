SELinux
========


.. image:: img/selinux.png


Security-Enhanced Linux (SELinux) is a Linux kernel security module that
provides a way to have access control security policies. This also allows a
way to have Mandatory access control (MAC), according to the `Wikipedia
<https://en.wikipedia.org/wiki/Mandatory_access_control>`_:

| In computer security, mandatory access control (MAC) refers to a type of
| access control by which the operating system constrains the ability of a
| subject or initiator to access or generally perform some sort of operation on
| an object or target.

The first version of SELinux was released in the year 2000 by NSA, and in 2003
it became part of the stable kernel. It was introduced in the Fedora Core 2, but
by default it was disabled. From Fedora Core 3 it was enabled in the system.

For the rest of the chapter, you will need a Fedora/CentOS/RHEL installation.


SELinux Modes
--------------

There are 3 different modes.

- enforcing
- permissive
- disabled

By default your system will come with *enforcing* mode. In this mode the
policies will be enforced in the system, and this should be used in every
production system. In the *permissive* mode the policies will not be enforced
but any denial is logged. The *disabled* mode completely disable the SELinux.

.. index:: getenforce

getenforce
------------

The *getenforce* command will tell you the current SELinux mode.

::

    $ getenforce
    Enforcing


.. index:: setenforce


setenforce
----------

Using *setenforce* command you can change the mode till the system reboots.

::

    # setenforce
    usage:  setenforce [ Enforcing | Permissive | 1 | 0 ]
    # setenforce Permissive
    # getenforce
    Permissive
    # setenforce 1
    # getenforce
    Enforcing


.. warning:: Never disable SELinux on production systems, if required you can put them into permissive mode,
   so that you can get the denial logs, and create proper policies from those logs. Also
   check `this website <https://stopdisablingselinux.com/>`_ before further reading.

To change the label permanently, we modify the */etc/selinux/config* file.

::

    $ sudo cat /etc/selinux/config

    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=enforcing
    # SELINUXTYPE= can take one of these three values:
    #     targeted - Targeted processes are protected,
    #     minimum - Modification of targeted policy. Only selected processes are protected.
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted

Change the value of *SELINUX* in the above mention file and then reboot the system to verify
the change.


Labels/Contexts
----------------

Every process and object in the system has a corresponding label or context. This label defines which
all processes can access which all objects. They have the following format:

::

    user:role:type:range


The Fedora and other distributions use the `type` to define access control, the *range* is optional.

Checking contexts of files/directories or processes
----------------------------------------------------

You can use the *-Z* flag along with standard *ls* or *ps* command to see the SELinux context.

For example if you execute **ls -lZ** in your home directory.

::

    $ ls -lZ
    total 0
    drwxr-xr-x. 11 vagrant vagrant unconfined_u:object_r:user_home_t:s0 222 Mar 21 05:38 lymworkbook
    drwxrwxr-x.  3 vagrant vagrant unconfined_u:object_r:user_home_t:s0  21 Mar 29 11:55 Video

You can see the *unconfined_u:object_r:user_home_t:s0* and if you execute the
same command against */tmp* then you will see the following:

::

    $ ls -lZ /tmp
    total 4
    -rw-rw-r--. 1 vagrant vagrant unconfined_u:object_r:user_tmp_t:s0   0 Apr  2 03:18 example.txt
    drwx------. 3 root    root    system_u:object_r:tmp_t:s0           17 Mar 29 16:59 systemd-private-2aad7f8cd577426094e46ae7f4da1426-chronyd.service-gFq0Yn
    -rwx--x--x. 1 vagrant vagrant unconfined_u:object_r:user_tmp_t:s0 205 Mar 21 05:17 vagrant-shell

The type context for temporary directory is *tmp_t* and when the user created
those files under */tmp*, the context is *user_tmp_t*, for the user home
directory it is *user_home_t*. The labels get matched against defined SELinux
rules. The file's label stays in the extended attribute in the file system.

Now, let us execute the *ps* command with the *Z* flag.

::

    $ ps auZ
    LABEL                           USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    system_u:system_r:getty_t:s0-s0:c0.c1023 root 776 0.0  0.3 15668 1812 ttyS0    Ss+  Mar29   0:00 /sbin/agetty -o -p -- \u --keep-baud 115200,38400,9600 ttyS0
    system_u:system_r:getty_t:s0-s0:c0.c1023 root 777 0.0  0.3 13100 1696 tty1     Ss+  Mar29   0:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
    unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 vagrant 5373 0.0  0.8 27192 4308 pts/0 Ss Mar31   0:00 -bash
    unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 vagrant 29048 0.0  0.7 57184 3824 pts/0 R+ 03:21   0:00 ps auZ


Here you can see how different processes have different kind of *type*
contexts. All *type* contexts generally ends with **_t**.


SELinux booleans
----------------

SELinux booleans are the rules which can be turned on or off. You can see all values (or a specific one) by using
*getsebool* command.


::

    $ getsebool -a
    abrt_anon_write --> off
    abrt_handle_event --> off
    abrt_upload_watch_anon_write --> on
    antivirus_can_scan_system --> off
    antivirus_use_jit --> off
    auditadm_exec_content --> on
    authlogin_nsswitch_use_ldap --> off
    authlogin_radius --> off
    authlogin_yubikey --> off
    awstats_purge_apache_log_files --> off
    boinc_execmem --> on
    cdrecord_read_content --> off
    cluster_can_network_connect --> off
    ...


