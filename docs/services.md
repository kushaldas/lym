## Linux Services

This is also a chapter related to the *systemd* tool.

### What is a service?

A service is a process or application which is running in the background, either doing some predefined tasks,or waiting for some events. If you remember our process chapter, we learned about *systemd* for the first time there. It is the first process to run in our sysytem, it
then start all the required processes and services. To know about how the system boots up,
read the *bootup* man page. [Click here](https://www.freedesktop.org/software/systemd/man/bootup.html) to read it online.

```
$ man bootup
```

### What is a daemon?

Daemon is the actual term for those long running backgroud processes. A service actually consists of one or more daemons.


### What is init system?

If you look at the Unix/Linux history, you will find the first process which stars up, is also known as *init process*. This process used to start other processes by using the rc files from */etc/rc.d* directory. In the modern Linux systems, *systemd* replaced the init system.


### Units in systemd

Units are a standardized way for the systemd to manage various parts of a system. There are different kinds of units, *.service* is for system services, *.path* for path based ones. There is also *.socket* which are socket based systemd units. There are various other types, we can
learn about those later.

### .service units in systemd

These are service units, which explains how to manage a particular service in the system. In our
daily life, we generally only have to work with these unit files.


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