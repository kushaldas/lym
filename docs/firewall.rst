Linux Firewall
===============

.. image:: img/firewall.png

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

    # iptables -nvL --line-numbers
    Chain INPUT (policy ACCEPT 82 packets, 4756 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         

    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         

    Chain OUTPUT (policy ACCEPT 42 packets, 3192 bytes)
    num   pkts bytes target     prot opt in     out     source               destination  


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

    # iptables -nvL --line-numbers
    Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1        0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
    2      122  9641 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    3        1    52 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    4        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:80
    5        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:443
    6       22  2044 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable

    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         

    Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1      104 12085 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0 

The *--line-numbers* argument shows the number of the each rule. We can use
these line numbers to delete any rule.

.. note:: For a desktop or laptop, you may want to drop all incoming connections, that will help in cases
          where someone in the local network may try to attack/scan your system.


Delete a rule based on rule number
-----------------------------------

Let us delete the rule number 4, which allows traffic to port 80.

::

    # iptables -D INPUT 4
    # iptables -nvL --line-numbers
    Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1        4   376 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
    2      221 15445 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    3        1    52 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    4        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:443
    5       22  2044 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable

    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         

    Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1      166 17248 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0 


Delete a rule directly
-----------------------

If you know the rule properly, you can also delete it based on the rule
directly.

::

    # iptables -D INPUT -p tcp --dport 443 -j ACCEPT
    # iptables -nvL --line-numbers
    Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1        4   376 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
    2      344 22417 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    3        1    52 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    4       22  2044 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable

    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         

    Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
    num   pkts bytes target     prot opt in     out     source               destination         
    1      234 22564 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0


Saving the rules
----------------

Any change made via **iptables** command stays on memory. To save it (so that
it autoreloads in reboot), use the following command.


For Debian.

::

    # netfilter-persistent save


For CentOS 7+

::

    # systemctl stop firewalld && systemctl disable firewalld
    # iptables-save > /etc/sysconfig/iptables
    # systemctl enable iptables
    Created symlink from /etc/systemd/system/basic.target.wants/iptables.service to /usr/lib/systemd/system/iptables.service.
    # systemctl start iptables

The first line stops and then disables the **firewalld** service, it is a newer
type of frontend for the same *netfilter* subsystem of the kernel.

A blog post from Major Hayden
------------------------------

Now, you should read the `following blog post
<https://major.io/2010/04/12/best-practices-iptables/>`_ from Major Hayden
best practices.


Debugging firewall rules
-------------------------

In case you want to debug the rules, and wan to see which packet matches which
rule in the chain, you can add these two following rules. After that, do
**tail -f /var/log/kern.log** to see the messages. Remember to use the proper
IP address and port number.

::

    # iptables -t raw -A PREROUTING -p tcp --destination YOUR_IP/24 --dport PORT_NUMBER -j TRACE
    # iptables -t raw -A OUTPUT -p tcp --destination YOUR_IP/24 --dport PORT_NUMBER -j TRACE


.. index:: nft

nftables
=========

**nftables** is the modern replacement for iptables, ip6tables, arptables, and
ebtables. It provides a single unified framework for packet filtering,
uses a simpler syntax, and offers better performance. nftables is available
in Linux kernel 3.13 and later, and is the default firewall framework in many
modern distributions.


Installing nftables
--------------------

On Debian/Ubuntu

::

    apt install nftables

On Fedora/CentOS/RHEL

::

    dnf install nftables


To enable nftables to start at boot:

::

    # systemctl enable nftables
    # systemctl start nftables


nftables concepts
------------------

nftables uses a similar concept to iptables with tables, chains, and rules,
but the syntax and structure are different.

- **Tables** contain chains. Unlike iptables, tables have no predefined
  meaning and you can name them as you wish.
- **Chains** contain rules. Chains must specify their type (filter, nat, route)
  and hook point (input, output, forward, prerouting, postrouting).
- **Rules** define the matching criteria and actions (called verdicts in nftables).

Common verdicts in nftables:

- **accept** - accept the packet
- **drop** - silently drop the packet
- **reject** - reject the packet and send an error response
- **queue** - send packet to userspace
- **continue** - continue with the next rule
- **return** - return from the current chain
- **jump** - jump to another chain
- **goto** - go to another chain (without returning)


The nft command
----------------

The **nft** command is used to manage nftables rules. Here are the basic
operations:

::

    nft list ruleset          # show all rules
    nft list tables           # list all tables
    nft list table inet mytable   # show rules in a specific table
    nft flush ruleset         # remove all rules
    nft flush table inet mytable  # remove all rules from a table


Address families
-----------------

nftables supports multiple address families:

- **ip** - IPv4 only
- **ip6** - IPv6 only
- **inet** - both IPv4 and IPv6 (recommended for most cases)
- **arp** - ARP packets
- **bridge** - bridge packets
- **netdev** - packets from ingress


View the current ruleset
-------------------------

To view all active rules:

.. code-block:: bash

    # nft list ruleset

If no rules are configured, the output will be empty. Let us create a basic
firewall configuration.


Creating a table
-----------------

First, create a table. We will use the **inet** family which handles both
IPv4 and IPv6.

::

    # nft add table inet filter


Creating chains
----------------

Now we create chains inside the table. We need to specify the type of chain
and when it should be evaluated (the hook).

::

    # nft add chain inet filter input { type filter hook input priority 0 \; policy drop \; }
    # nft add chain inet filter forward { type filter hook forward priority 0 \; policy drop \; }
    # nft add chain inet filter output { type filter hook output priority 0 \; policy accept \; }

The **priority** determines the order of evaluation (lower numbers are
evaluated first). The **policy** sets the default action when no rules match.

.. note:: The semicolons need to be escaped with backslash when running from
          the shell. If you use a configuration file, escaping is not needed.


Adding rules
-------------

Let us add rules to allow established connections and traffic on the loopback
interface:

::

    # nft add rule inet filter input iif lo accept
    # nft add rule inet filter input ct state established,related accept

To allow SSH (port 22), HTTP (port 80), and HTTPS (port 443):

::

    # nft add rule inet filter input tcp dport 22 accept
    # nft add rule inet filter input tcp dport { 80, 443 } accept

Notice how nftables allows specifying multiple ports using curly braces.


Viewing rules with handles
---------------------------

To delete or insert rules at specific positions, you need the rule handles:

.. code-block:: bash

    # nft -a list table inet filter
    table inet filter {
        chain input {
            type filter hook input priority filter; policy drop;
            iif "lo" accept # handle 4
            ct state established,related accept # handle 5
            tcp dport 22 accept # handle 6
            tcp dport { 80, 443 } accept # handle 7
        }
        chain forward {
            type filter hook forward priority filter; policy drop;
        }
        chain output {
            type filter hook output priority filter; policy accept;
        }
    }


Deleting a rule
----------------

To delete a rule, use its handle number:

::

    # nft delete rule inet filter input handle 7

This removes the rule allowing ports 80 and 443.


Inserting a rule at a specific position
----------------------------------------

To insert a rule before another rule (using its handle):

::

    # nft insert rule inet filter input position 5 tcp dport 80 accept

This inserts the rule before the rule with handle 5.


Example: Complete firewall configuration
-----------------------------------------

Here is a complete example of a server firewall configuration. You can put
this in a file (for example, **/etc/nftables.conf**):

::

    #!/usr/sbin/nft -f

    flush ruleset

    table inet filter {
        chain input {
            type filter hook input priority 0; policy drop;

            # allow loopback
            iif lo accept

            # allow established/related connections
            ct state established,related accept

            # drop invalid packets
            ct state invalid drop

            # allow ICMP (ping)
            ip protocol icmp accept
            ip6 nexthdr icmpv6 accept

            # allow SSH
            tcp dport 22 accept

            # allow HTTP and HTTPS
            tcp dport { 80, 443 } accept
        }

        chain forward {
            type filter hook forward priority 0; policy drop;
        }

        chain output {
            type filter hook output priority 0; policy accept;
        }
    }


Loading a configuration file
-----------------------------

To load a configuration file:

::

    # nft -f /etc/nftables.conf

To make sure your rules are loaded at boot, the systemd service will
automatically load **/etc/nftables.conf** when nftables starts.


Saving the current ruleset
---------------------------

To save the current rules to a file:

::

    # nft list ruleset > /etc/nftables.conf

Make sure the file starts with ``#!/usr/sbin/nft -f`` and includes
``flush ruleset`` at the beginning if you want it to replace all existing
rules when loaded.


NAT with nftables
------------------

nftables can also handle Network Address Translation. Here is an example
of setting up masquerading (SNAT) for a system acting as a router:

::

    table inet nat {
        chain postrouting {
            type nat hook postrouting priority 100;
            oif "eth0" masquerade
        }
    }

For port forwarding (DNAT), to forward port 8080 to an internal server:

::

    table inet nat {
        chain prerouting {
            type nat hook prerouting priority -100;
            tcp dport 8080 dnat to 192.168.1.100:80
        }
    }


Rate limiting
--------------

nftables has built-in support for rate limiting. To limit SSH connections
to 3 per minute per source IP:

::

    # nft add rule inet filter input tcp dport 22 ct state new limit rate 3/minute accept


Logging packets
----------------

To log packets before accepting or dropping them:

::

    # nft add rule inet filter input tcp dport 22 log prefix \"SSH connection: \" accept

Logs appear in the kernel log (use **dmesg** or check **/var/log/kern.log**).


Differences from iptables
--------------------------

Key differences to remember when moving from iptables to nftables:

- No built-in tables or chains - you create your own
- Single tool (**nft**) replaces iptables, ip6tables, arptables, ebtables
- Uses **inet** family for dual-stack IPv4/IPv6 rules
- Sets can be used directly in rules: ``tcp dport { 22, 80, 443 }``
- Syntax is more consistent and readable
- Rules can be added atomically from a file
- Better performance with large rulesets

.. note:: Many distributions now include **iptables-nft**, which provides
          iptables commands that translate to nftables rules. You can check
          which backend you are using with ``iptables -V``.
