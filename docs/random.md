## Random things

I have yet to figure out where to put this information, which is why they are here, in 
the random chapter.   
These will be moved to different chapters in the future.

```eval_rst
.. index:: w
```

### w command

The *w* command shows all the users, logged in to the computer. If you pass the *-f* flag, it
toggles information about where each user is logged in from.

```
$ w
 17:22:41 up 24 days, 11:37,  2 users,  load average: 0.56, 0.50, 0.59
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
kdas     tty2      31May17 22days  3:07m  3:16  i3 -a --restart /run/user/1000/i3/restart-state.28641
```

```eval_rst
.. index:: uptime
```

### How long is the system running?

We have the *uptime* command which gives us information about how long the system is running.   
You can figure out the last time the sytem turned off or rebooted at a glance.   
For my laptop, it was 24 days ago.

```
$ uptime
 17:31:30 up 24 days, 11:46,  2 users,  load average: 0.76, 0.98, 0.81
```

```eval_rst
.. index:: dmesg
```

### dmesg command

The *dmesg* command prints out messages from the kernel buffer.
Using this tool we can learn about the messages and information from the kernel drivers during and after the boot up process.  
This can be very handy when troubleshooting; for e.g. when the machine fails to boot or a certain piece of hardware does not function correctly.
