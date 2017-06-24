## Users and Groups

In this chapter we will learn about user and group management on your
system, and also about basic access control.

In Linux everything is associated to an user and a group. Based on these values, the system who can access what part of the system. That includes files, directories, network ports etc.

### Finding the owner of file

We can use *ls -l* command to find the owner, and group of a file or directory.

![](img/lsl.png)

In the above example, fedora is the name of the owner and group both. The first value talks about who all can access this file (we will learn about it in some time).

### /etc/passwd file

*/etc/passwd* contains all the user available in the system. This is a plain text file, means you can view the information by using *cat* command.

```
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-timesync:x:999:998:systemd Time Synchronization:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
chrony:x:998:995::/var/lib/chrony:/sbin/nologin
systemd-coredump:x:994:994:systemd Core Dumper:/:/sbin/nologin
fedora:x:1000:1000:Fedora:/home/fedora:/bin/bash
polkitd:x:993:993:User for polkitd:/:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
```

Each line has seven entries separated by *:*. 

```
username:password:uid:gid:gecos:/home/dirname:shell

|---------------|---------------------------------|
|field          | meaning                         |
|---------------|---------------------------------|
| username      | the username                    |
|---------------|---------------------------------|
| password      | the password of the user        |
|---------------|---------------------------------|
| uid           | Numeric user id                 |
|---------------|---------------------------------|
| gid           | Numeric group id of user        |
|---------------|---------------------------------|
| gecos         | arbitary field                  |
|---------------|---------------------------------|
| /home/dirname | Home directory of the user      |
|---------------|---------------------------------|
| shell         | Which shell to use for the user |
|---------------|---------------------------------|
```

There are accounts with */sbin/nologin* as shell, these are generally
accounts for various services, which are not supposed to be used by a normal human user, that is why they don't have a shell to be used.

The actual user passwords are stored in */etc/shadow* file, only the
root user can access this file.

```
$ ls -l /etc/shadow
----------. 1 root root 2213 Jun 22 15:20 /etc/shadow
```

### Details about the groups

Group details are stored inside of */etc/group* file. Each user has
one primary group, and zero or more supplementary groups.

### wheel group

If your user is part of the *wheel* group, then it has sudo access. If you remember Fedora Installer, it actually gives an option to mark the new user to be part of wheel group during installation.

### Adding a new user

*useradd* command adds a new user to the system. You can guess that this command has to execute as root, otherwise anyone can add random user accounts in the system. The following command adds a new user
*babai* to the system.

```
$ sudo useradd babai
```

In Fedora, the initial user you create gets the uid 1000.

### Changing user password

*passwd* command helps to change any user password.

```
$ sudo passwd babai
Changing password for user babai.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

### Modifying existing user details

*usermod* command can help to modify any existing user.
You can use the same command to lock any account in the system.

```
$ sudo usermod -L babai
$ su - babai
Password: 
su: Authentication failure 
```

### Deleting an user

We can use *userdel* command to delete an user from the system.


