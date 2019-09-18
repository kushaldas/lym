Linux Firewall
===============

.. note:: This chapter is an ongoing work.

A firewall is a network security system, which can monitor and control network
packets coming in and going out from a system based on pre-defined rules.

In this chapter, we will learn about **iptables** command and how can we use
the same to create and manage the system's firewall. The `netfilter
<https://en.wikipedia.org/wiki/Netfilter>`_ subsystem in Linux Kernel handles
the actual packet filtering in the network level.



Installation
-------------

On CentOS

::

    yum install iptables-services


On Debian systems

::

    apt install iptables-persistent


Tables, chains and rules
-------------------------

There is a table based system which in turn uses chains of rules for the
firewall. Each table has a defined set of chains, and the rules get into the
get chain one after another.

When a network packet reaches the related table, and the related chain inside
of the table, the rules gets matched from top to bottom. If the packet matches
then the *target* of the rule gets executed. Each chain also has a default
policy, if no rule matches, then, the default poilicy gets applied on the
packet. We will learn more about these in details.

iptables has 5 built in chains.

- **INPUT** for all packets incoming to the system
- **OUTPUT** for all packets going out from the system
- **FORWARD** for the routed packets, this is when the system works as a router
- **PREROUTING** for port forwarding
- **POSTROUTING** for Source Network Address Translation (SNAT), this applies to all
  packets leaving the system

filter table
-------------

**filter** is the default table of iptables. It has 3 default chains.

- INPUT
- OUTPUT
- FORWARD

nat table
---------

**nat** table is a special table for SNAT and DNAT (port forwarding).
It has the following chains.

- PREROUTING
- POSTROUTING
- OUTPUT

There are two other different tables, **mangle** and **raw**.

.. index:: iptables

iptables command
-----------------

The following table will be helpful in remembering different arguments to
**iptables** command.

::

    +------------------+--------------+---------------------+------------------------+-------------+
    | Table            | Command      | Chain               | Matches                | Target/Jump |
    +------------------+--------------+---------------------+------------------------+-------------+
    | filter (default) | -A (append)  | INPUT               | -p protocol            | ACCEPT      |
    | nat              | -I (insert)  | OUTPUT              | -s source_ip           | DROP        |
    | mangle           | -D (delete)  | FORWARD             | -d destination_ip      | LOG         |
    | raw              | -R (replace) | PREROUTING          | --sport source_port    | REJECT      |
    |                  | -F (flush)   | POSTROUTING         | --dport destination_ip | DNAT        |
    |                  | -L (list)    | USER_DEFINED_CHAINS | -i incoming            | SNAT        |
    |                  | -S (show)    |                     | -o outgoing            | LIMIT       |
    |                  | -Z (zero)    |                     | -m mac                 | RETURN      |
    |                  | -N           |                     | -m time                | MASQUERADE  |
    |                  | -X           |                     | -m quota               |             |
    |                  |              |                     | -m limit               |             |
    |                  |              |                     | -m recent              |             |
    +------------------+--------------+---------------------+------------------------+-------------+


View the existing rules
------------------------

.. code-block:: bash

    # iptables -L
    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination         

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination         

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination 


The above command shows the default table **filter** and all chains and rules
inside of it. You can notice that each of the chains has a default policy
**ACCEPT**. It means if no rules match (in this case no rules are defined), it
will accept those packets.


Appending rules to INPUT chain
-------------------------------

We can test an initial rule to **drop** all incoming *icmp* packets to the
system. The following rule will append the rule to the **INPUT** chain.

.. note:: `ping` command uses `icmp <https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol>`_ packets. So, the following command will block
          `ping` into the system.

.. code-block:: bash

    iptables -A INPUT -p icmp -j DROP

Now, if you try to ping the system from any computer, you will not get any
response.

Flushing all rules
-------------------

::

    iptables -F

The above command will help to flush (remove) all the rules from the default
table. You can actually use *-t TABLE_NAME* argument to flush any particular
table.


Example of a series of rules
-----------------------------

Here is a list of rules to allow traffic to port 22 (ssh) and port 80 and 443
(http and https).

::

    iptables -A INPUT -i lo -j ACCEPT
    iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    iptables -A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    iptables -A INPUT -p tcp --dport 443 -j ACCEPT
    iptables -A OUTPUT -j ACCEPT
    iptables -A INPUT -j REJECT
    iptables -A FORWARD -j REJECT

The first rules allows all incoming traffic on the `loopback` device.
The second line allows packets related to an already established connection,
or the cases where a packet is trying to reconnect.
The last 3rd last line allows all outgoing packets, and the last 2 lines
reject everything else which does not match the rules.
If you want to view all the rules.

::

    # iptables -nL
    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination         
    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:443
    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination         
    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination         
    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0  

.. note:: For a desktop or laptop, you may want to drop all incoming connections, that will help in cases
          where someone in the local network may try to attack/scan your system.


Saving the rules
----------------

Any change made via **iptables** command stays on memory. To save it (so that 
it autoreloads in reboot), use the following command.


For Debian.

::

    # netfilter-persistent save


For CentOS 7+

::

    # iptables-save > /etc/sysconfig/iptables
    # systemctl enable iptables
    Created symlink from /etc/systemd/system/basic.target.wants/iptables.service to /usr/lib/systemd/system/iptables.service.
    # systemctl start iptables
