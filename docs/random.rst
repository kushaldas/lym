Random things
==============

I have yet to figure out where to put this information, which is why they are
here, in the random chapter. These will be moved to different chapters in the
future.

.. index:: w

w command
----------

The **w** command shows all the users, logged in to the computer. If you pass
the *-f* flag, it toggles information about where each user is logged in from.

::

    $ w
    17:22:41 up 24 days, 11:37,  2 users,  load average: 0.56, 0.50, 0.59
    USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
    kdas     tty2      31May17 22days  3:07m  3:16  i3 -a --restart /run/user/1000/i3/restart-state.28641

.. index:: uptime

How long is the system running?
---------------------------------

We have the **uptime** command which gives us information about how long the
system is running. You can figure out the last time the system turned off or
rebooted at a glance. For my laptop, it was 24 days ago.

::

    $ uptime
    17:31:30 up 24 days, 11:46,  2 users,  load average: 0.76, 0.98, 0.81

.. index:: time

Finding CPU time of a command
------------------------------

The **time** command will help you to find the CPU time spent for any command.
The following example will tell us how much time ``du -sh`` took to calculate the
disk usage.

::

    $ time du -sh
    5.5G	.

    real	0m1.026s
    user	0m0.235s
    sys	0m0.783s

.. index:: dmesg

dmesg command
--------------

The **dmesg** command prints out messages from the kernel buffer. Using this
tool we can learn about the messages and information from the kernel drivers
during and after the boot up process. This can be very handy when
troubleshooting; for e.g. when the machine fails to boot or a certain piece of
hardware does not function correctly.


Setting up cron jobs
---------------------

One can schedule tasks using cron jobs. You can mention a certain time when a
given task will be executed. In latest Fedora/CentOS, we use **cronie**
package, in other systems we have **cron** or **anacron** package.


To view any existing jobs

::

    crontab -l


To add a new cronjob or edit a provious one, use the command

``crontab -e``.


Format of a crontab file

::

    * * * * * /path/to/command
    + + + + +
    | | | | |
    | | | | |
    | | | | |
    | | | | |
    | | | | |
    | | | | +------>    Day of the week (0-7)
    | | | |
    | | | +-------->    Month of the year (1-12)
    | | |
    | | +---------->    Day of the month (1-31)
    | |
    | +------------>    Hour (0-23)
    |
    +-------------->    Minute (0,59)


Say we want to execute a shell script everyday at midnight.

::

    0 0 * * * /usr/bin/myscript.sh


Another example can be executing the same script in every 15 minutes in every hour.


::

    0,15,30,45 * * * * /usr/bin/myscript.sh




.. index:: last

Finding out details about previous logins or system reboots
------------------------------------------------------------

**last** command will give you the details about all the previous logins and
shutdown/reboots. This command actually checks */var/tmp/wtmp* file for the
logs.

The */var/log/btmp* file stores all the bad login details, and */var/log/utmp*
file stores the details of the currently logged in users (**who** command
reads this file).

You can read the *btmp* file using **last** command.

::

    last -f /var/log/btmp

To know more, you can read the man page of *wmtp*.


Whats next?
============

After you are familiar with the commands in this book, we would suggest you to learn
shell scripting.

Start from `https://www.shellscript.sh <https://www.shellscript.sh>`_ and then
you can read the `beginners bash guide
<http://mirrors.kernel.org/LDP/LDP/Bash-Beginners-Guide/Bash-Beginners-Guide.pdf>`_.
