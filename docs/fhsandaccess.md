## File system

Now you know a few really basic, Linux commands.
Before we can learn anything else, we should look into how files and directories are structured inside a Linux system.

```eval_rst
.. index:: fhs
```
### FHS

```
$ ls /
bin  boot  dev  etc  home  lib  lib64  lost+found  mc  media  mnt  opt  output  proc  root  run  sbin  srv  sys  tmp  usr  var
```

```/``` is the root directory of your file system.
It’s under this directory, that all the other files and directories reside. There’s a [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html), which talks
about these different directories, and what kinds of files are located in which directory.
