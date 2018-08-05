## Linux常用命令

- 系统安全：sudo, su, chmod, setfacl

- 进程管理： w, top, ps, kill, pstree, killall

- 用户管理： id, usermod, useradd, groupadd, userdel

- 文件系统： mount, umount, fsck, df, du

- 系统关机和重启： shutdown, reboot

- 网络应用： curl, telnet, mail, elinks

- 网络测试： ping, netstate, host

- 网络配置： hostname, ifconfig

- 常用工具： ssh, screen, clear, who, date

- 软件包管理： yum, rpm, apt-get

- 文件查找和比较： locate, find

- 文件内容查看： head, tail, less, more

- 文件处理： touch, unlink, rename, ln, cat

- 目录操作： cd, mv, rm, pwd, tree, cp, ls

- 文件权限属性： setfacl, chmod, chown, chgrp

- 压缩/解压： bzip2/bunzip2, gzip/gunzip, zip/unzip, tar

- 文件传输： ftp, scp



## Linux系统定时任务

- crontab命令    crontab -e    `*****命令(分 时 日 月 周)`

- at命令，一次性执行    `at 2:00 tomorrow    at>/home/Jason/do_job`    ctrl + D 结束




## vi/vim编辑器

- 一般模式：删除、复制和粘贴

    - 切换编辑模式：i, I, o, O, a, A, r, R
    
    - 切换命令行模式： :, /, ?
    
- 移动光标：ctrl + f, ctrl + b, 0或者功能键Home, $或者功能键End, G, gg, N + Enter