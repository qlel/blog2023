+++
author = "qlel"
title = "Linux_exec"
date = "2023-08-13"
description = "在shell脚本中使用exec命令，根据操作的对象不同会有不同的行为。"
tags = [
"shell", "linux", "bash"
]
categories = [
"学习"
]
+++
在shell脚本中使用`exec`命令，根据操作的对象不同会有不同的行为。

## 重定向文件操作符
主要用于将shell脚本的所有标准输出和标准错误都重定向到一个日志文件中。
```bash
exec >/home/qlel/my.log 2>&1
```
执行此命令不会退出shell脚本。

## 其他命令
无论何时我们在Bash shell中运行任何命令，默认情况下都会创建一个子shell，并生成一个新的子进程(fork)来执行该命令。但是，当使用`exec`时，`exec`后面的命令将替换当前shell。这意味着不创建子shell，当前进程被这个新命令取代。

系统调用`exec`是以新的进程去代替原来的进程，但进程的PID保持不变。因此，可以这样认为，`exec`系统调用并没有创建新的进程，只是替换了原来进程上下文的内容。原进程的代码段，数据段，堆栈段被新的进程所代替。

在shell脚本中使用`exec`，当`exec`后面的其它命令被执行完就会退出脚本，之后的shell命令不执行。

```bash
#!/usr/bin/env bash

echo "one 1"
exec echo "two 2"
echo "three 3"
```
结果：
```bash
one 1
two 2
```