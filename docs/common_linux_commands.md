鸟哥的 Linux 私房菜
===============

* `ls -l --full-time` 能够显示出完整的时间格式，包括年月日时间。

* `uname -r`: Check kernel version

* `uname -m`: Print the machine hardware name, like 'x86_64'

* `lsb_release -a`: Print version related to Linux Standard Base

---

**ls**

* `ls -a`: list all file, including hidden ones.

* `ls -d`: only list directory, not the files in it.

* `ls -l`: list the detail information of file, including the attributes and permissions.

---

**cp**

* `cp -a`: equal to -dr --preserve=all

* `cp -d`: 

* `cp -i`: if dest file is exists, then ask before overwrite.

* `cp -p`: copy with attributes (permission, user, time), often use in backup.

* `cp -r`: copy recursively.

---

**cat**

* `cat -n`: print line number, include blank line.

* `cat -b`: print line number without blank line.

---

**chattr**: set hidden attribute of file

* `chattr +a`: file only allow to add content, no delete or modify, only root can change this attribute.

* `chattr +i`: file can not be delete, change name, set link or modify, only root can set this attribute.

---

**get filename and dirname**

looks like the `os.path.basename()` and `os.path.dirname()` in python.

```
[jonathang@study tmp]$ basename /etc/sysconfig/network
network
[jonathang@study tmp]$ dirname /etc/sysconfig/network
/etc/sysconfig
```