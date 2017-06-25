## Useful commands

In  this chapter we will learn about more commands which we may have to use in daily life.


### Creating soft link to a file

Soft link or symbolic links are the special kind of files, which actually points to some other file using either related or absolute path. We can create soft links using *ln -s* command.

```
$ ln -s /etc/hostname name
$ ls -l
total 12
-rw-rw-r--. 1 fedora fedora   13 Jun 23 11:14 hello.txt
lrwxrwxrwx. 1 fedora fedora   13 Jun 23 12:32 name -> /etc/hostname
$ cat name
kushal-test.novalocal
```
In the above example, we created a soft link called *name* to the */etc/hostname* file. You can see the details about the soft link files by using the *ls -l* command. You can create links to any directory in the same way.

If you remove the original file the soft link is pointing to, then
the soft link will become useless, and will point to a file which does not exists. Soft links can point to file which is in a different file system.

### Creating hard links

```
$ echo "Hello World!" > hello.txt
$ ln hello.txt bye.txt
$ ls -l
total 16
-rw-rw-r--. 2 fedora fedora   13 Jun 23 11:14 bye.txt
-rw-rw-r--. 2 fedora fedora   13 Jun 23 11:14 hello.txt
lrwxrwxrwx. 1 fedora fedora   13 Jun 23 12:32 name -> /etc/hostname
$ cat hello.txt 
Hello World!
$ cat bye.txt 
Hello World!
$ echo "1234" > hello.txt 
$ cat bye.txt 
1234
$ cat hello.txt 
1234
$ rm hello.txt 
$ cat bye.txt 
1234
$ ls -l
total 12
-rw-rw-r--. 1 fedora fedora    5 Jun 23 12:39 bye.txt
lrwxrwxrwx. 1 fedora fedora   13 Jun 23 12:32 name -> /etc/hostname
```

If you look carefully, in the above example, we created a hard link using the *ln* command. When we made a change to the original *hello.txt* file, that also reflects in the *bye.txt* file. But, because *bye.txt* is a hard link, even if I deleted the *hello.txt*, the hard link still exists, and also have the original content.


### Extracting a tar file

*tar* is a tool to create and extract archive files. Many times we will have to download and then extract a tar file in our regular computer usage.

```
$ tar -xzvf files.tar.gz 
hello.c
bye.txt
```

*files.tar.gz* file is compressed with gzip, if the file name ends with
*.tar.bz2*, then it is compressed wth bzip2.

```
$ tar -xjvf files.tar.bz2 
hello.c
bye.txt
```

### Creating a tar file

We can use the same *tar* command to create a tar file.

```
$ tar -czvf files.tar.gz hello.c bye.txt 
hello.c
bye.txt
$ ls
bye.txt  files.tar.gz  hello.c
```

### Becoming root user

*root* is the superuser. It has power to make changes in various parts
of a Linux system, that also means if you by mistake make any dangerous change (say deleting your user account) as root, that cause real problem. The general rule is, only when you need superuser power, use the *sudo* command to get it done, and keep using your normal user account for everything else. The *su -* command will help you to become the *root* user, use this carefully.

```
$ su -
Password:
# 
``` 

Notice how the command prompt changed to *#* from *$*, *#* shows that you are using the *root*, that is another visible indication to think
about every command you give as *root*. You can press *Ctrl+d* to log out from the *root* account.

### Using sudo command

You can add *sudo* command in front of any command to execute them as
*root*. For example:

```
$ less /var/log/secure
/var/log/secure: Permission denied
$ sudo less /var/log/secure
[sudo] password for fedora:
... long output
```

### Environment variables

Environment variables are a way to pass data to the applications. We can set values of different variables, which then any of the application can access. There are various variables which decides how
the shell will behave. To see all the variables use the *printenv* command.

```
$ printenv
... long output
```

You can execute the same command once as normal user, and once as *root*, and then check for the differences between the output. That is because the variables can be user specific.

### Setting up environment variable values

 We can use the *export* and *set* commands to create a new environment variable or we can change an existing one. We can use the *echo* command to print one particular environment variable value.

```
$ export NAME="Kushal Das"
$ echo $NAME
Kushal Das
$ export NAME="Babai Das"
$ echo $NAME
Babai Das
````

In the above example we first created a new variable called *name* and then we changed the value of the variable.

 