---
title: Linux 文件隐藏属性
author: Jinzhong Xu
mathjax: true
date: 2020-07-18 10:30:20
tags:
- linux
- chattr
- lasttr
categories: technology
---

Linux 隐藏属性对于系统安全非常重要，特别是chattr命令。所谓隐藏属性就是使用标准的 ls -la命令无法查看的属性，不过需要知道的是chattr命令只能在Ext2/Ext3/Ext4的Linux传统文件系统上完整生效，其他文件系统可能无法完整支持这个命令。如xfs仅仅支持chattr命令的部分参数而已。

<!--more-->

- chattr 设置文件隐藏属性

  基本命令方法：

  ```shell
  [root@centos ~]# chattr  [+-=] [AaSscdiu] 文件或目录名称
  选项与参数：
  +：增加某一个特殊参数，其他原本存在参数不动
  -：移除某一个特殊参数，其他原本存在参数不动
  =：设置一定，且仅有后面接的参数
  A：存取文件或目录时，存取时间atime不会被修改，可避免I/O较慢机器过度存取磁盘
  a：文件只能增加数据，不能删除不能修改数据，只有root可以设置该属性
  S：一般的文件是使用非同步的方式写入磁盘，设置该属性后可保证文件修改后同步写入磁盘
  s：文件被删除，则将会被完全的移除出硬盘，误删将完全无法挽回
  c：将自动“压缩”文件，读取时将自动解压缩，存储时将压缩后再存储
  d：当dump程序被执行的时候，设置d属性将可使该文件或目录不会被dump备份
  i：可使得文件不能被删除、改名、设置链接、写入或新增数据，只有root可以设置该属性
  u：与s相反，如果文件被删除，则数据内容还存在磁盘中，可以使用该方法保证文件内容在磁盘上
  ```

  更多更详细(man chattr)：

  ```shell
  CHATTR(1)                             General Commands Manual                             CHATTR(1)
  
  NAME
         chattr - change file attributes on a Linux file system
  
  SYNOPSIS
         chattr [ -RVf ] [ -v version ] [ -p project ] [ mode ] files...
  
  DESCRIPTION
         chattr changes the file attributes on a Linux file system.
  
         The format of a symbolic mode is +-=[aAcCdDeijPsStTu].
  
         The  operator  '+'  causes the selected attributes to be added to the existing attributes of
         the files; '-' causes them to be removed; and '=' causes them to be the only attributes that
         the files have.
  
         The  letters 'aAcCdDeFijPsStTu' select the new attributes for the files: append only (a), no
         atime updates (A), compressed (c), no copy on write (C), no dump (d), synchronous  directory
         updates  (D), extent format (e), case-insensitive directory lookups (F), immutable (i), data
         journalling (j), project hierarchy (P), secure deletion (s),  synchronous  updates  (S),  no
         tail-merging (t), top of directory hierarchy (T), and undeletable (u).
  
         The  following  attributes are read-only, and may be listed by lsattr(1) but not modified by
         chattr: encrypted (E), indexed directory (I), and inline data (N).
  
         Not all flags are supported or utilized by all filesystems; refer to filesystem-specific man
         pages such as btrfs(5), ext4(5), and xfs(5) for more filesystem-specific details.
  
  OPTIONS
         -R     Recursively change attributes of directories and their contents.
  
         -V     Be verbose with chattr's output and print the program version.
  
         -f     Suppress most error messages.
  
         -v version
                Set the file's version/generation number.
  
         -p project
                Set the file's project number.
  
  ATTRIBUTES
         A  file  with  the  'a' attribute set can only be open in append mode for writing.  Only the
         superuser or a process possessing the CAP_LINUX_IMMUTABLE capability can set or  clear  this
         attribute.
  
         When  a file with the 'A' attribute set is accessed, its atime record is not modified.  This
         avoids a certain amount of disk I/O for laptop systems.
  
         A file with the 'c' attribute set is automatically compressed on the disk by the kernel.   A
         read  from this file returns uncompressed data.  A write to this file compresses data before
         storing them on the disk.  Note: please make sure to read the bugs and  limitations  section
         at the end of this document.
  
         A  file  with the 'C' attribute set will not be subject to copy-on-write updates.  This flag
         is only supported on file systems which perform copy-on-write.  (Note: For  btrfs,  the  'C'
         flag  should  be  set  on new or empty files.  If it is set on a file which already has data
         blocks, it is undefined when the blocks assigned to the file will be fully stable.   If  the
         'C'  flag is set on a directory, it will have no effect on the directory, but new files cre‐
         ated in that directory will have the No_COW attribute set.)
  
         A file with the 'd' attribute set is not candidate for backup when the  dump(8)  program  is
         run.
  
         When  a  directory  with  the  'D'  attribute  set is modified, the changes are written syn‐
         chronously on the disk; this is equivalent to the 'dirsync' mount option applied to a subset
         of the files.
  
         The  'e'  attribute indicates that the file is using extents for mapping the blocks on disk.
         It may not be removed using chattr(1).
  
         The 'E' attribute is used by the experimental encryption patches to indicate that  the  file
         has  been  encrypted.   It  may not be set or reset using chattr(1), although it can be dis‐
         played by lsattr(1).
  
         A directory with the 'F' attribute set indicates that  all  the  path  lookups  inside  that
         directory  are  made  in  a case-insensitive fashion.  This attribute can only be changed in
         empty directories on file systems with the casefold feature enabled.
  
         A file with the 'i' attribute cannot be modified: it cannot be deleted or renamed,  no  link
         can  be  created to this file, most of the file's metadata can not be modified, and the file
         can not be  opened  in  write  mode.   Only  the  superuser  or  a  process  possessing  the
         CAP_LINUX_IMMUTABLE capability can set or clear this attribute.
  
         The  'I'  attribute  is used by the htree code to indicate that a directory is being indexed
         using hashed trees.  It may not be set or reset using chattr(1), although  it  can  be  dis‐
         played by lsattr(1).
  
         A file with the 'j' attribute has all of its data written to the ext3 or ext4 journal before
         being written to the file itself, if the file system is mounted with the  "data=ordered"  or
         "data=writeback"  options and the file system has a journal.  When the filesystem is mounted
         with the "data=journal" option all file data is already journalled and this attribute has no
         effect.   Only the superuser or a process possessing the CAP_SYS_RESOURCE capability can set
         or clear this attribute.
  
         A file with the 'N' attribute set indicates that the file has data stored inline, within the
         inode  itself.  It  may not be set or reset using chattr(1), although it can be displayed by
         lsattr(1).
  
         A directory with the 'P' attribute set will enforce a  hierarchical  structure  for  project
         id's.  This means that files and directory created in the directory will inherit the project
         id of the directory, rename operations are constrained so when a file or directory is  moved
         into  another directory, that the project id's much match.  In addition, a hard link to file
         can only be created when the project id for the file and the destination directory match.
  
         When a file with the 's' attribute set is deleted, its blocks are zeroed and written back to
         the  disk.   Note:  please  make sure to read the bugs and limitations section at the end of
         this document.
  
         When a file with the 'S' attribute set is modified, the changes are written synchronously on
         the disk; this is equivalent to the 'sync' mount option applied to a subset of the files.
  
         A  file with the 't' attribute will not have a partial block fragment at the end of the file
         merged with other files (for those filesystems which support tail-merging).  This is  neces‐
         sary  for  applications  such  as  LILO  which read the filesystem directly, and which don't
         understand tail-merged files.  Note: As of this writing, the ext2 or ext3 filesystems do not
         (yet, except in very experimental patches) support tail-merging.
  
         A directory with the 'T' attribute will be deemed to be the top of directory hierarchies for
         the purposes of the Orlov block allocator.  This is a hint to the block  allocator  used  by
         ext3  and ext4 that the subdirectories under this directory are not related, and thus should
         be spread apart for allocation purposes.   For example it is a very good idea to set the 'T'
         attribute on the /home directory, so that /home/john and /home/mary are placed into separate
         block groups.  For directories where this attribute is not set, the  Orlov  block  allocator
         will try to group subdirectories closer together where possible.
  
         When  a file with the 'u' attribute set is deleted, its contents are saved.  This allows the
         user to ask for its undeletion.  Note: please make sure to read  the  bugs  and  limitations
         section at the end of this document.
  ```
  注意1：属性设置常见的是 a 与 i 的设置值，而且很多设置值必须要身为 root 才能设置 

  注意2：xfs 文件系统仅支持 AadiS 而已 

  ```shell
  范例：请尝试到/tmp下面创建文件，并加入 i 的参数，尝试删除看看。 
  [root@study ~]# cd /tmp 
  [root@study tmp]# touch attrtest <==创建一个空文件 
  [root@study tmp]# chattr +i attrtest <==给予 i 的属性 
  [root@study tmp]# rm attrtest <==尝试删除看看 
  
  rm: remove regular empty file `attrtest'? y
  rm: cannot remove `attrtest': Operation not permitted 
  
  看到了吗？呼呼！连 root 也没有办法将这个文件删除呢！赶紧解除设置！ 
  
  范例：请将该文件的 i 属性取消！
  [root@study tmp]# chattr -i attrtest
  ```

- lsattr 显示文件隐藏属性

  ```shell
  [root@study ~]# lsattr [-adR] 文件或目录
  选项与参数：
  -a ：将隐藏文件的属性也秀出来；
  -d ：如果接的是目录，仅列出目录本身的属性而非目录内的文件名；
  -R ：连同子目录的数据也一并列出来！
  [root@study tmp]# chattr +aiS attrtest
  [root@study tmp]# lsattr attrtest
  --S-ia---------- attrtest
  
  ```

  

设置chattr后，可以使用lsattr查看隐藏属性，请小心使用这两个命令，特别是chattr。一个例子，

```bash
chattr +i /etc/shadow
```

则将无法增加新用户。

该篇参考：鸟哥的Linux私房菜 基础学习篇 第四版 p322