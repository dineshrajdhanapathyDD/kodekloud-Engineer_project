

## Questions:
The `Nautilus` security team performed an audit on all servers present in `Stratos DC`. During the audit some critical data/files were identified which were having the wrong permissions as per security standards. Once the report was shared with the production support team, they started fixing the issues. It has been identified that one of the files named `/etc/resolv.conf` on `Nautilus App 1` server has wrong permissions, so that needs to be fixed and the correct ACLs needs to be set.


1. The user owner and group owner of the file should be `root `user.

2. `Others` must have `read only` permissions on the file.

3. User `jim` must not have any permission on the file.

4. User `rod` should have `read only` permission on the file.


## Solution:  

**1. Login on   App server as per the task**

```
thor@jump_host /$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2.  Check the existing file permission**

```
[root@stapp01 ~]# getfacl /etc/resolv.conf
getfacl: Removing leading '/' from absolute path names
# file: etc/resolv.conf
# owner: root
# group: root
user::rw-
group::r--
other::r--
```

**3.  As per the task check users are already existing  or not**

```
[root@stapp01 ~]# id jim
uid=1002(jim) gid=1002(jim) groups=1002(jim)
[root@stapp01 ~]# id rod
uid=1003(rod) gid=1003(rod) groups=1003(rod)
```

**4.  Set the ACL permissoin as per the task**

```
[root@stapp01 ~]# setfacl -m u:jim:-,rod:r /etc/resolv.conf

(The setfacl utility sets ACLs (Access Control Lists) of files and directories. On the command line, a sequence of commands is followed by a sequence of files (which in turn can be followed by another sequence of commands, and so on).

The options -m and -x expect an ACL on the command line. Multiple ACL entries are separated by commas (","). The options -M and -X read an ACL from a file or from standard input. The ACL entry format is described in the ACL Entries section, below.

The --set and --set-file options set the ACL of a file or a directory. The previous ACL is replaced. ACL entries for this operation must include permissions.

The -m (--modify) and -M (--modify-file) options modify the ACL of a file or directory. ACL entries for this operation must include permissions.

The -x (--remove) and -X (--remove-file) options remove ACL entries. It is not an error to remove an entry which does not exist. Only ACL entries without the perms field are accepted as parameters, unless the POSIXLY_CORRECT environment variable is defined.

The perms field is a combination of characters that indicate the permissions: read ("r"), write ("w"), execute ("x"), or "execute only if the file is a directory or already has execute permission for some user" (capital "X"). Alternatively, the perms field is an octal digit ("0"-"7").)
```

**5.  Validate the task check all user have correct ACL permission set**

```
[root@stapp01 ~]# getfacl /etc/resolv.conf
getfacl: Removing leading '/' from absolute path names
# file: etc/resolv.conf
# owner: root
# group: root
user::rw-
user:jim:---
user:rod:r--
group::r--
mask::r--
other::r--
```

**6. Click on `Finish` & `Confirm` to complete the task successful.**