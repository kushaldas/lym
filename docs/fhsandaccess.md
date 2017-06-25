## File system

Now you know few very basic Linux commands. Before we can learn anything else, we should look into how the files and directories are structured inside of a Linux system.

### FHS

```
$ ls /
bin  boot  dev  etc  home  lib  lib64  lost+found  mc  media  mnt  opt  output  proc  root  run  sbin  srv  sys  tmp  usr  var
```

*/* is the root directory of your file system, under this directory, all the other files
and directories reside. There is a [Filesystem Hierarchy Standard(FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html), which talks
about these different directories, and what kinds of files are located in which directory.

