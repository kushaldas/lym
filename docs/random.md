## Random things

I am yet to figure out where to put these information, that is why they are in 
the random chapter. These will be moved to different chapters in future.

```eval_rst
.. index:: w
```

### w command

*w* command shows who all are logged into the computer. If you pass the *-f* flag, it
toggles the information about from where each of the user is logged in.

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

We have *uptime* command which tells us about how long the system is running, you can figure out when did you last time turned it off or rebooted from this information. For my laptop, it was 24 days ago.

```
$ uptime
 17:31:30 up 24 days, 11:46,  2 users,  load average: 0.76, 0.98, 0.81
```

```eval_rst
.. index:: dmesg
```

### dmesg command

*dmesg* command prints out the various messages from the kernel buffer. Using this tool we can
learn about the messages from the kernel drivers during and after the boot up process.
