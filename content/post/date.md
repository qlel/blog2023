+++
author = "qlel"
title = "Linux_date"
date = "2023-08-13"
description = "Linux 的日期时间管理"
tags = [
"shell", "linux", "bash"
]
categories = [
"学习"
]
+++

Linux 的日期时间管理。

以给定的格式显示当前时间，或设置系统日期。

## date参数
参数|说明
-|-
`-d STRING`|显示由STRING描述的时间
`-I[FMT]`|显示ISO8601格式时间。FMT精度：<br>`date`(默认)、`hours`、`minutes`、`seconds`、`ns`
`-R`|以RFC5322格式输出日期和时间
`-r FILE`|显示文件最后修改时间
`-u`|显示UTC时间
`-s STRING`|设置STRING描述的时间

## 日期格式
```bash
%H 小时，24小时制（00~23）
%I 小时，12小时制（01~12）
%k 小时，24小时制（0~23）
%l 小时，12小时制（1~12）
%M 分（00～59）
%p 显示出上午或下午
%r 时间，12小时制
%s 从1970年1月1日0点到目前经历的秒数
%S 秒（00～59） 
%T 时间（24小时制）（hh:mm:ss）
%X 显示时间的格式（％H时％M分％S秒）
%Z 按字母表排序的时区缩写
%a 星期名缩写
%A 星期名全称
%b 月名缩写
%B 月名全称
%c 日期和时间
%d 按月计的日期（01～31）
%D 日期（mm/dd/yy） 
%h 和%b选项相同
%j 一年的第几天（001~366）
%m 月份（01～12）
%w 一个星期的第几天（0代表星期天）
%W 一年的第几个星期（00～53，星期一为第一天）
%x 显示日期的格式（mm/dd/yy）
%y 年份的最后两个数字（1999则是99）
%Y 年份（比如1970、1996等）
%C   世纪，通常为省略当前年份的后两位数字
%U  一年中的第几周，以周日为每星期第一天
%e   按月计的日期，添加空格，等于%_d
```

## 示例
默认显示时间：
```bash
$ date
Fri 26 Mar 2021 08:32:49 PM CST
```
格式化输出：
```bash
$ date +'%Y %m %d'
2021 03 26
$ date +'日期：%Y %m %d'
日期：2021 03 26
$ date +'%Y-%m-%d %H:%M:%S'
2021-03-26 20:46:30
```
显示自己指定的时间：
```bash
# 显示今天23点
$ date -d "23" +"%Y-%m-%d %H:%M:%S"
2021-03-26 23:00:00

# 显示1天前日期
$ date -d "1 day ago" +"%Y-%m-%d"
2021-03-25

# 显示2小时后日期
$ date -d "2 hour" +"%Y-%m-%d %H:%M:%S"
2021-03-26 22:59:02

# 显示2分钟后日期
$ date -d "2 min" +"%Y-%m-%d %H:%M:%S"
2021-03-26 21:03:15

# 显示指定时间
$ date -d "2020-12-05 12:12:02" +"%Y-%m-%d %H:%M:%S"
2020-12-05 12:12:02
```
加减操作：
```bash
# 明天
$ date -d '+1 day' +'%Y-%m-%d'
2021-03-27
# 昨天
$ date -d '-1 day' +'%Y-%m-%d'
2021-03-25

# 下个月
$ date -d '+1 month' +'%Y-%m-%d'
2021-04-26
# 上个月
$ date -d '-1 month' +'%Y-%m-%d'
2021-02-26

# 2年后
$ date -d '+2 year' +'%Y-%m-%d'
2023-03-26
# 2年前
$ date -d '-2 year' +'%Y-%m-%d'
2019-03-26
```
设置时间：
```bash
date -s 20120523               //设置成20120523，这样会把具体时间设置成空00:00:00
date -s 01:01:01               //设置具体时间，不会对日期做更改
date -s "01:01:01 2012-05-23"  //这样可以设置全部时间
date -s "01:01:01 20120523"    //这样可以设置全部时间
date -s "2012-05-23 01:01:01"  //这样可以设置全部时间
date -s "20120523 01:01:01"    //这样可以设置全部时间
```