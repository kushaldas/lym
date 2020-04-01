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
it became part of the stable kernel. 

For the rest of the chapter, you will need a Fedora/CentOS/RHEL installation.

.. todo:: Add more history, like when was it introduced in Fedora.


SELinux Modes
--------------

There are 3 different modes.

- enforcing
- permissive
- disabled

By default you system will come with *enforcing* mode. In this mode the
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
