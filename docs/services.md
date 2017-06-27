## Linux Services

This is also a chapter related to the *systemd* tool.

```eval_rst
.. index:: bootup
```

### What is a service?

A service is a process or application which is running in the background, either doing some predefined tasks or waiting for some events. If you remember our process chapter, we learned about *systemd* for the first time there. It is the first process to run in our sysytem, it
then starts all the required processes and services. To know about how the system boots up,
read the *bootup* man page. [Click here](https://www.freedesktop.org/software/systemd/man/bootup.html) to read it online.

```
$ man bootup
```

```eval_rst
.. index:: daemon
```

### What is a daemon?

Daemon is the actual term for those long-running background processes. A service actually consists of one or more daemons.


### What is init system?

If you look at the Unix/Linux history, you will find the first process which stars up, is also known as *init process*. This process used to start other processes by using the rc files from */etc/rc.d* directory. In the modern Linux systems, *systemd* replaced the init system.


### Units in systemd

Units are a standardized way for the systemd to manage various parts of a system. There are different kinds of units, *.service* is for system services, *.path* for path based ones. There is also *.socket* which are socket based systemd units. There are various other types, we can
learn about those later.

```eval_rst
.. index:: services
```

### .service units in systemd

These are service units, which explains how to manage a particular service in the system. In our
daily life, we generally only have to work with these unit files.

```eval_rst
.. index:: systemctl
```

### How to find about all the systemd units in the system?

```
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
```

In the output of the *systemctl* command, you should be able to see all the different kinds of
units in the system. If you want to see only the service units, then use the following command.

```
$ systemctl --type=service
```

### Working with a particular service

Let us take the *sshd.service* as an example. The service controls the sshd daemon, which allows
us to remotely login to a system using the *ssh* command.

To know the current status of the service, I can execute the following command.

```
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
```

To start the service, I will use the following command, and then I can use the *status* argument to the *systemctl* to check the service status once again.

```
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
```

In the same way, we can use either *stop* or *restart* arguments to the *systemctl* command.


### Enabling or disabling a service

Even if you start a service, after you reboot the computer, you will find that the service
did not start at the time of boot up. To do so, you will have to enable the service, or stop a service to start at the time of boot, you will have to disable the service.

```
$ sudo systemctl enable sshd.service
Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/lib/systemd/system/sshd.service.
$ sudo systemctl disable sshd.service
Removed /etc/systemd/system/multi-user.target.wants/sshd.service.
```

### Shutdown or reboot the system using systemctl

We can also reboot or shutdown the system using the systemctl command.

```
$ sudo systemctl reboot
$ sudo systemctl shutdown
```

### Finding the logs of a service

We can use the *journalctl* command to find the log of a given service.
The general format is *journalctl -u servicename". Like below is the log for *sshd* service.

```
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
```

```eval_rst
.. index:: journalctl
```

### Continuous stream of logs

In case you want to monitor the logs of any service, that is keep reading the logs in real time, you can use *-f* flag with the *journalctl* command.

```
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
```

I can see that someone was trying to break into this VM by trying random ports :)

### Listing of previous boots

In systems like Fedora, *journalctl* by default keeps the history from
old boots. To know about all the available boot history, type the following command.

```
$ sudo journalctl --list-boots
[sudo] password for fedora: 
-112 7a88e13a76434a1199f82ad90441ae7f Tue 2014-12-09 03:41:08 IST—Tue 2014-12-09 03:41:08 IST
-111 b86086ed59b84b228e74f91ab08a66b3 Sun 2015-06-28 23:54:26 IST—Sun 2015-07-12 07:27:48 IST
-110 71d3f6024f514653bfd2574243d096d1 Sun 2016-06-05 01:51:05 IST—Sun 2016-06-05 01:51:16 IST
-109 b7721878a5144d009418cf269b5eea71 Fri 2016-08-19 19:47:57 IST—Sat 2016-08-20 01:16:07 IST
-108 6102102fc7804379b888d83cea66838b Sat 2016-08-20 01:21:36 IST—Sun 2016-08-21 00:05:38 IST
... long output
```

To know about any particular boot log, you can use the hash along with *-b* flag to the *journalctl* command.

```
$ sudo journalctl -b 7a88e13a76434a1199f82ad90441ae7f
-- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 13:40:49 IST. --
Dec 09 03:41:08 localhost.localdomain systemd[1344]: Stopping Default.
Dec 09 03:41:08 localhost.localdomain systemd[1344]: Stopped target Default.
Dec 09 03:41:08 localhost.localdomain systemd[1344]: Starting Shutdown.
Dec 09 03:41:08 localhost.localdomain systemd[1344]: Reached target Shutdown.
Dec 09 03:41:08 localhost.localdomain systemd[1344]: Starting Exit the Session..
```

### Time-based log viewing

We can also use *journalctl* to view logs for a certain time. For example, if we want to see all the logs since yesterday, we can use the following command.

```
sudo journalctl --since yesterday
[sudo] password for fedora: 
-- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 15:21:54 IST. --
Jun 23 00:00:00 kushal-test.novalocal /usr/libexec/gdm-x-session[28622]: (evolution-alarm-notify:11609): evolution-alarm-notify-WARNING **: alarm.c:253: Reques
Jun 23 00:01:01 kushal-test.novalocal CROND[22327]: (root) CMD (run-parts /etc/cron.hourly)
... long output
```

You can also use date time following *YYYY-MM-DD HH:MM:SS* format.

```
$ sudo journalctl --since "2015-11-10 14:00:00"
-- Logs begin at Tue 2014-12-09 03:41:08 IST, end at Sat 2017-06-24 15:25:30 IST. --
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Timers.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Paths.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Starting D-Bus User Message Bus Socket.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Listening on D-Bus User Message Bus Socket.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Sockets.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Basic System.
Jun 05 01:51:05 kushal-test.novalocal systemd[5674]: Reached target Default.
```