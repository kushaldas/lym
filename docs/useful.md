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

```eval_rst
.. index:: tar
```
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


```eval_rst
.. index:: vim
```
### Vim editor

Text editors are the tool to edit files. This can be a configuration file, or source code, or an email, or any other kind of text file.
Which editor to use is generally a personal choice, and a lot good energy have been to wasted to tell which one is the best editor. In this book we will just learn about *Vim* editor. It is also known as *vi enhanced* editor. In the Fedora Linux distribution, the *vi* command is actually an alias to the the *vim* itself.

If we just type vim, and press enter, we will see the following screen.

![](/img/vim1.png)

#### :q to exit vim

Press Escape and then type *:q* to exit the vim.

![](/img/vim2.png)

#### Open a new file or edit an existing file

*vim filename* is the command to open an existing file, if the file is not there, it will open a new empty file for editing.

#### Different modes of vim

Vim editor starts with command mode, every time you open any file, this is the default mode of the editor. You can press the *Escape* key in any other mode to come to the command mode.

You can press *i* to go into insert mode, we edit documents in the insert mode. Here if you press *Escape* you will get back to the command mode.

![](/img/vim3.png)

#### :w to save a file

In the command mode typing *:w* saves a file, if you want to save
and quit the editor, then type either *:wq* or *:x*.

#### :q! to quit without saving

In the command typing *:q!* will allow us to quit without saving
the current file.

Vim is a powerful editor, and we learned about only a very basic steps into it. It will a complete book to explain different features of vim. But, the steps above are sufficient for this book's scope. 

```eval_rst
.. index:: su
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

```eval_rst
.. index:: sudo
```
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

```eval_rst
.. index:: Environment variable
```
### Environment variables

Environment variables are a way to pass data to the applications. We can set values of different variables, which then any of the application can access. There are various variables which decides how
the shell will behave. To see all the variables use the *printenv* command.

```
$ printenv
... long output
```

You can execute the same command once as normal user, and once as *root*, and then check for the differences between the output. That is because the variables can be user specific.

```eval_rst
.. index:: export
```
### Setting up environment variable values

 We can use the *export* command to create a new environment variable or we can change an existing one. We can use the *echo* command to print one particular environment variable value.

```
$ export NAME="Kushal Das"
$ echo $NAME
Kushal Das
$ export NAME="Babai Das"
$ echo $NAME
Babai Das
```

In the above example we first created a new variable called *name* and then we changed the value of the variable.

```eval_rst
.. index:: locate
```
### locate command

*locate* is a very useful tool to find files in the system. This is part of the *mlocate* package. For example, the following command will
search all the files with firewalld in the name.

```
$ locate firewalld
/etc/firewalld
/etc/sysconfig/firewalld
/etc/systemd/system/basic.target.wants/firewalld.service
/home/kdas/.local/share/Zeal/Zeal/docsets/Ansible.docset/Contents/Resources/Documents/docs.ansible.com/ansible/firewalld_module.html
/home/kdas/Downloads/ansible-devel/lib/ansible/modules/system/firewalld.py
/home/kdas/Downloads/ansible-fail-on-github-zipfile/lib/ansible/modules/system/firewalld.py
/home/kdas/code/git/ansible/lib/ansible/modules/system/firewalld.py
... long output
```

You can update the search database by using *updatedb* command as root.

```eval_rst
.. index:: updatedb
```
```
$ sudo updatedb
```

This may take some time as it will index all the files in your computer.

 