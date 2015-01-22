---
layout: post
title : 常用 WinDBG 命令
category : Debug
tags : [WinDBG, Debug]
---
k
=

查看当前线程栈.

`kb` : 显示函数的前三个参数

`kp` : 显示比较全的信息,参数的名称与类型也会显示

~
=

显示所有线程信息,前面有个小黑点的是当前线程(以下所示的2号线程是当前活动的线程)

    0:002> ~
    0  Id: 5a0.1f8 Suspend: 1 Teb: 7ffdd000 Unfrozen
    1  Id: 5a0.158 Suspend: 1 Teb: 7ffdc000 Unfrozen
    .  2  Id: 5a0.b28 Suspend: 1 Teb: 7ffdb000 Unfrozen

`~*kb`(或`~*kp`)能显示所有线程的栈信息.

~xs
===

`s`的意思可能是 set current thread

该命令的意思是 切换到第x个线程.

也可以使用 `~~[TID]s` 来切换到对应线程, `TID` 是 ThreadId 的意思, 注意中括号不能少

g
=

`go` 的意思.[断点产生后执行该命令则会继续执行][].

!runaway
========

查看各线程的时间.

    0:004> !runaway
    User Mode Time
    Thread       Time
    2:7b0       0 days 0:00:20.203
    0:790       0 days 0:00:00.015
    4:eb8       0 days 0:00:00.000
    3:8a4       0 days 0:00:00.000
    1:648       0 days 0:00:00.000

!runaway 可以带参数.
`1`表示用户态时间,`2`表示内核态时间,`4`表示线程创建以来所经历的时间.这三个数可以或一下.

x
=

显示符号的地址 如 `printf` 函数的地址是 `77bd27c2`:

    x msvcrt!printf
    77bd27c2 msvcrt!printf = <no type information>

ln
==

`ln address` 尝试显示 address 附近的符号

dv
==

`dv` 观察栈上的局部变量 dv /i/t/V 表示格式

!for\_each\_local
=================

!for\_each\_local 枚举当前所有的局部变量

!for\_each\_frame dv
====================

!for\_each\_frame dv 切换到所有栈帧，并显示局部变量

u
=

反汇编 用法 u + 起始地址 + 长度 Length

    u 0040ff12 L100

dds
===

显示内存并尝试解析为符号. 用法 dds + 起始地址 + 长度 如:

    dds 0040ff12 L20

s
=

搜索字符串

    s -a 0040ff12 L999999 "the string"

.frame
======

切换栈帧, 先用 kb/knL 将栈帧都打印出来, 再用 .frame 切换之.

    .frame 5

dt
==

格式化显示, `dt pObject` 会显示pObject的类型以及该类的成员变量.

`dt address CMyClass`, 会将 address 当成 CMyClass 的对象来解析.

ba
==

内存访问断点.

对buffer的前4个字节进行读操作时会产生中断.

    ba r4 MyExe!buffer

对buffer的前4个字节进行写操作时会产生中断.

    ba w4 MyExe!buffer