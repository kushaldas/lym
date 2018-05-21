File system mounting
=====================

In this chapter, we’ll learn how to mount file systems.  If you type
*mount* in the shell, it will tell you about various file systems, and
how are they mounted (as a directory) in the system.


.. index:: mount

    $ mount
    sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime,seclabel)
    proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
    devtmpfs on /dev type devtmpfs (rw,nosuid,seclabel,size=2012852k,nr_inodes=503213,mode=755)
    securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
    tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,seclabel)
    devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,seclabel,gid=5,mode=620,ptmxmode=000)
    tmpfs on /run type tmpfs (rw,nosuid,nodev,seclabel,mode=755)
    tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,seclabel,mode=755)
    cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd)
    pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime,seclabel)
    cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
    cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
    cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
    cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
    cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpu,cpuacct)
    cgroup on /sys/fs/cgroup/net_cls,net_prio type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls,net_prio)
    cgroup on /sys/fs/cgroup/pids type cgroup (rw,nosuid,nodev,noexec,relatime,pids)
    cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
    cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
    cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
    configfs on /sys/kernel/config type configfs (rw,relatime)
    /dev/vda1 on / type ext4 (rw,relatime,seclabel,data=ordered)
    selinuxfs on /sys/fs/selinux type selinuxfs (rw,relatime)
    systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=23,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=11175)
    mqueue on /dev/mqueue type mqueue (rw,relatime,seclabel)
    debugfs on /sys/kernel/debug type debugfs (rw,relatime,seclabel)
    hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,seclabel)
    tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=404680k,mode=700,uid=1000,gid=1000)


If you look carefully at the output above, you’ll find that
*/dev/vda1* is mounted as root */* in the system. This is actually the
primary hard drive in this system. The device can be different based
on the system.

- /dev/vd*  For virtual machines
- /dev/sd*  For physical machines

The number at the end of the device name is the partition number.


.. index:: NTFS

Connecting USB drives to your system
-------------------------------------

If you connect vfat partitioned USB drives (the normal pendrives),
they will auto mount under the */run/media/username/* directory.  But,
for NTFS based drives, you will have to install the driver to mount
those partitions.

::

    $ sudo dnf install ntfs-3g -y

.. index:: mount

Mounting a device
-----------------

We can use the *mount* command to mount a file system on an existing
directory. The syntax to do that is, *mount device /path/to/mount/at*.

::

    $ sudo mount /dev/sdb1 /mnt


In the example above, we mounted */dev/sdb1* on the */mnt* directory.


.. index:: umount

Unmounting
-----------

We use the *umount* command on a given directory to unmount the file system.

Do not remove any drive from the system before unmounting them.  Just to be on
the safe side, you can execute the *sync* command, which will write any existing
cache to the drives.  That will make sure that your chances of losing data is
marginal.

Encryptding drives with LUKS
-----------------------------

Follow `this
link <https://kushaldas.in/posts/encrypting-drives-with-luks.html>`_ to
learn about how to encrypt your drives with LUKS. This is a simple way
to make sure that even if you loose your USB drive, the data inside
can still be safe (relatively). 