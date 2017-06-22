## Process in Linux

A process is a program (think about any Linux application) in running state. It contains various
details, like the memory space the program needs, it has a process id, the files opened by
the process, etc.


### How to view all the running processes?

The following command shows all the processes from your computer.

```
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 215356  4984 ?        Ss   May29   0:28 /usr/lib/systemd/systemd --system --deserialize 19
root         2  0.0  0.0      0     0 ?        S    May29   0:00 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   May29   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    May29   0:11 [ksoftirqd/0]
root         7  0.0  0.0      0     0 ?        S    May29   8:27 [rcu_sched]
... long output
```

You can see that the output also tells you under which user the process is running, what is actual command being used, the percentage of CPU and memory usage.

*PID* column shows the process id, you can see that the *systemd* process has PID 1, means it is
the first process to start in the system.

### How to find some particular process?

Say I know to know what is the process id of the Firefox browser in my system. I can use the
following command to find that information.

```
$ ps aux | grep firefox
kdas     26752 96.1  9.7 2770724 763436 ?      Sl   16:16   0:35 /usr/lib64/firefox/firefox
kdas     26919  0.0  0.0 118520   980 pts/3    S+   16:17   0:00 grep --color=auto firefox
```

Here, we are first running the ps command, and then passing the output of that to the next
command using a | character. In this case, grep is that second command, as you can see, we can find
some text using grep tool. We will learn more about grep in future.

### How to kill a particular process?

We can kill any process using the *kill* command. As we found out that the id of the Firefox process in my computer is 26752, we can use that id to kill it.

```
$ kill 26752
```

If there is no error message, you should be able to see that the Firefox has disappeared. 

### top command

*top* is a very special command while using a Linux system, this is a quick way to know about
all the running processes in the system, and status about CPU and memory usage in general. To get
out of top, press the key *q*.

```
Tasks: 372 total,   2 running, 370 sleeping,   0 stopped,   0 zombie
%Cpu(s): 11.6 us,  2.6 sy,  0.0 ni, 84.9 id,  0.1 wa,  0.3 hi,  0.5 si,  0.0 st
KiB Mem :  7858752 total,  1701052 free,  4444136 used,  1713564 buff/cache
KiB Swap:  3268604 total,  1558396 free,  1710208 used.  2431656 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                          
28300 kdas      20   0 1502016 287340  44396 R  25.0  3.7 290:56.60 chrome                                                                           
 2668 kdas       9 -11 2067292   9756   7164 S   6.2  0.1 166:06.48 pulseaudio                                                                       
15122 kdas      20   0  771844  33104  11352 S   6.2  0.4  39:24.60 gnome-terminal-                                                                  
24760 kdas      20   0 1945840 209128  76952 S   6.2  2.7   1:41.15 code                                                                             
27526 kdas      20   0  156076   4268   3516 R   6.2  0.1   0:00.01 top                                                                              
    1 root      20   0  215356   4880   3108 S   0.0  0.1   0:28.25 systemd                                                                          
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.66 kthreadd                                                                         
    4 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H                                                                     
    6 root      20   0       0      0      0 S   0.0  0.0   0:11.79 ksoftirqd/0                                                                      
    7 root      20   0       0      0      0 S   0.0  0.0   8:28.06 rcu_sched 
... long output
```

Btw, feel free to press *1* and see if anything changes in the top command output.

### htop tool

*htop* is a modern version of the top tool. This is many more features, interactiveness
is the biggest among them. *htop* does not come by default in most of the Linux installations,
means you will have to install it using the command package management tool.

The following are the ways to install it in Fedora and in Debian/Ubuntu

```
$ sudo dnf install htop -y
```

```
$ sudo apt-get install htop
```

TODO: add screenshot

To know more about htop, please read the man page.

```
$ man htop
```