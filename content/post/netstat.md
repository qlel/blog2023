+++
author = "qlel"
title = "netstat"
date = "2023-08-13"
description = "netstat命令用于显示与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。netstat是在内核中访问网络及相关信息的程序，它能提供TCP连接，TCP和UDP监听，进程内存管理的相关报告。"
tags = [
"shell", "linux", "bash"
]
categories = [
"学习"
]
+++
netstat命令用于显示与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。netstat是在内核中访问网络及相关信息的程序，它能提供TCP连接，TCP和UDP监听，进程内存管理的相关报告。

```c
$ netstat --help
usage: netstat [-vWeenNcCF] [<Af>] -r         netstat {-V|--version|-h|--help}
       netstat [-vWnNcaeol] [<Socket> ...]
       netstat { [-vWeenNac] -i | [-cnNe] -M | -s [-6tuw] }

        -r, --route              display routing table
        -i, --interfaces         display interface table
        -g, --groups             display multicast group memberships
        -s, --statistics         display networking statistics (like SNMP)
        -M, --masquerade         display masqueraded connections

        -v, --verbose            be verbose
        -W, --wide               don't truncate IP addresses
        -n, --numeric            don't resolve names
        --numeric-hosts          don't resolve host names
        --numeric-ports          don't resolve port names
        --numeric-users          don't resolve user names
        -N, --symbolic           resolve hardware names
        -e, --extend             display other/more information
        -p, --programs           display PID/Program name for sockets
        -o, --timers             display timers
        -c, --continuous         continuous listing

        -l, --listening          display listening server sockets
        -a, --all                display all sockets (default: connected)
        -F, --fib                display Forwarding Information Base (default)
        -C, --cache              display routing cache instead of FIB

  <Socket>={-t|--tcp} {-u|--udp} {-U|--udplite} {-S|--sctp} {-w|--raw}
           {-x|--unix} --ax25 --ipx --netrom
  <AF>=Use '-6|-4' or '-A <af>' or '--<af>'; default: inet
  List of possible address families (which support routing):
    inet (DARPA Internet) inet6 (IPv6) ax25 (AMPR AX.25)
    netrom (AMPR NET/ROM) rose (AMPR ROSE) ipx (Novell IPX)
    ddp (Appletalk DDP) x25 (CCITT X.25)
```

选项|说明
-|-
`-h`,`--help`|显示帮助信息
`-V`,`--version`|显示版本信息
`-r`,`--route`|显示路由表
`-i`,`--interfaces`|显示接口/网卡表
`-g`,`--groups`|显示多播组成员关系
`-s`,`--statistics`|显示网络统计信息
`-M`,`--masquerade`|显示伪装的连接
`-v`,`--verbose`|显示指令执行详细过程
`-W`,`--wide`|显示伪装的连接
`-n`,`--numeric`|显示数字地址，而不是尝试确定符号主机，端口或用户名。
`--numeric-hosts`|显示数字主机地址，但不影响端口或用户名的解析。
`--numeric-ports`|显示数字端口号，但不影响主机名或用户名的解析。
`--numeric-users`|显示数字用户ID，但不影响主机名或端口名的解析。
`-N`,`--symbolic`|解析硬件名。显示网络硬件外围设备的符号连接名称。
`-e`,`--extend`|显示网络其他更多相关信息。显示扩展信息。
`-p`,`--programs`|显示正在使用Socket的程序PID和程序名称。
`-o`,`--timers`|显示计时器。
`-c`,`--continuous`|列出所有连接。每隔1秒就重新显示一遍，直到用户中断它。
`-l`,`--listening`|显示处于监听中服务的套接字。
`-a`,`--all`|显示所有套接字 (默认: connected)。包括正在监听的。
`-F`,`--fib`|显示转发信息库FIP
`-C`,`--cache`|显示路由缓存
`-t`,`--tcp`|显示TCP协议的连接情况
`-u`,`--udp`|显示UDP协议的连接情况。
`-x`,`--unix`|显示unix协议的连接情况。
`-w`,`--raw`|显示RAW传输协议的连线状况。。
`--ip`,`--inet`|显示...

示例:
```bash
etstat -a     列出所有端口
netstat -n     显示所有已建立的有效连接 ( 用点分四段式的形式显示ip )。
netstat -at    列出 所有 TCP 端口
netstat -au    列出 所有 UDP 端口
netstat -nu    列出 所有 UDP 端口
netstat -apu   显示UDP端口号的使用情况。
netstat -ax    列出 所有 UNIX 端口
netstat -g     显示组播组的关系。

netstat -l    显示监听端口。即 显示监听的套接字。
netstat -lt   显示监听的 TCP 端口
netstat -lu   显示监听的 UDP 端口
netstat -lx   显示监听的 UNIX 端口

netstat -s    显示所有端口的协议统计信息
netstat -st   或者  -su    显示 TCP 或者 UDP 端口统计信息
netstat -p    -p 开关可以与其他开关一起使用，就可以添加 “PID/进程名称” 到netstat的输出中
netstat -pt   
netstat -an   以数字形式显示(不使用主机名)
netstat -a --numeric-ports
netstat -a --numeric-hosts
netstat -a --numeric-users

netstat -c     每隔一秒输出网络信息
netstat --verbose    显示系统不支持的地址族
netstat -r     显示核心路由信息
netstat -rn    显示数字格式，不查询主机名

netstat -ap | grep ssh    找出程序运行端口
netstat -an | grep ':80'    找出运行在指定端口的进程
netstat -i      显示网络接口列表。即 显示网卡列表。
netstat -ie     显示详细信息。跟 ifconfig 很像。

netstat -ntl    用来查看linux的端口使用情况
netstat -natp
netstat -ntlp
netstat -anp | grep 3306
netstat -an
netstat -ae |grep mysql

netstat -e     显示关于以太网的统计数据。  
netstat -i -e  显示主机上每个网络接口的配置和状态
netstat -lp    标识正在监听的网络服务
netstat -rn    检查路由表
 
Linux查看端口及服务 
　　# netstat -tulpn 
　　或者是
　　# netstat -npl


查看TIME_WAIT连接数
netstat -ae|grep "TIME_WAIT" |wc -l
```