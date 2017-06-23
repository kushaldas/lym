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