

在系统中还有两个命令。
whatis：这个命令的作用和 man -f 是一致的。
apropos：这个命令的作用和 man -k 是一致的。
不过这两个命令和 man 基本一致，所以了解就好。不过 Linux 的命令很有意思，想知道这个命令是干什么的，可以执行 whatis 命令；想知道命令在哪里，可以执行 whereis 命令；想知道当前登录用户是谁，可以执行 whoami 命令。


```
表 1 man 命令的快捷键
快捷键	作 用
上箭头	向上移动一行
下箭头	向下移动一行
PgUp	向上翻一页
PgDn	向下翻一页
g	移动到第一页
G	移动到最后一页
q	退出
/字符串	从当前页向下搜索字符串
?字符串	从当前页向上搜索字符串
n	当搜索字符串时，可以使用n键找到下一个字符串
N	当搜索字符串时，使用N键反向查询字符串。也就是说，如果使用“/字符串”方式搜索， 则N键表示向上搜索字符串；如果使用“?字符串”方式搜索，则N键表示向下搜索字符串
```

```
man命令的帮助级别
级 别	作 用
1	普通用户可以执行的系统命令和可执行文件的帮助
2	内核可以调用的函数和工具的帮助
3	C语言函数的帮助
4	设备和特殊文件的帮助
5	配置文件的帮助
6	游戏的帮助（个人版的Linux中是有游戏的）
7	杂项的帮助
8	超级用户可以执行的系统命令的帮助
9	内核的帮助
```

```
main 是最常见的帮助命令，也是 Linux 最主要的帮助命令，其基本信息如下。
命令名称：man。
英文原意：format and display the on-line manual pages。
所在路径：/usr/bin/man。
执行权限：所有用户。
功能描述：显示联机帮助手册。
命令格式
[root@localhost ~]# man [选项]命令

选项：
-f：査看命令拥有哪个级别的帮助
-k: 査看和命令相关的所有帮助
```

http://c.biancheng.net/linux_tutorial/
https://explainshell.com/explain/1/ls
https://www.linuxcool.com/
http://c.biancheng.net/linux_tutorial/
https://wangchujiang.com/linux-command/list.html#!kw=ls