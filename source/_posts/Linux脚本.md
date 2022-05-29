---
title: Linux脚本
date: 2022/5/25 11:21:25
tags:
  - Linux
  - sh
categories:
  - Linux
cover: /img/sh/sh.jpg
---



# Linux脚本

## shebang

```shell
#!/bin/bash
```

## 变量

###### 规则

- 命名中只能使用英文字母，数字和下划线，首个字符不能用数字开头
- 中间不能又空格，可以使用下划线
- 先声明后使用

```shell
#!/bin/bash
# $0表示运行文件名 $1表示传递的参数
echo $0
echo $1
```

```shell
sh ./test.sh 张郑旭
```

```shell
./test.sh
张郑旭
```

## 运算符

```shell
#! /bin/bash

a=10
b=20
c=张郑旭
d=黄莉惠

echo `expr $a \* $b` #一定要\
echo `expr $a / $b`
echo `expr $a + $b`
echo `expr $a - $b`

# 算数关系
[ $a == $b ]
[ $a != $b ]
[ $a eq $b ]
[ $a ne $b ]
[ $a gt $b ]
[ $a lt $b ]
[ $a ge $b ]
[ $a le $b ]

# 布尔运算
[ ! $a eq $b ]
[ $a eq $b -o $a le $b ]
[ $a eq $b -a $a le $b ]

# 字符串运算符
[ $c = $d ]
[ $c != $d ]
[ -z $c ]
[ -n $d ]
[ str $c ] # 字符串是否为空
```

###### 文件测试运算符

```shell
[ -e $file ] #文件是否存在
[ -d $file ] #是否是目录
[ -f $file ] #是否是普通文件
[ -s $file ] #是否不为空
[ -r -w -x ] #是否可读可写可执行
```

###### 通配符

- *：代表匹配任意字符

- ？：一个？代表一个字符

- ;：连续不同命令的分隔符

- [[0-9\]]: 匹配0-9之间的10个数字

###### 转义字符

- \ : 使反斜杠后边的一个变量变成一个单纯的字符串
- '' : 转义其中所以变量保存为单纯的字符串
- " " : 保留属性，不进行转义处理
- `` ： 把其中命令执行后返回结果



###### 正则表达式（用于处理文件内容，而不是文件）

- ^

  ```shell
  grep '^1'
  ```

- $

  ```shell
  grep 'host%'
  ```

- ^$

  ```shell
  grep -n '^%' 
  ```

- .

  ```shell
  grep '.' #匹配任意一个字符
  ```

- \ : 转移字符

- *

  ```shell
  grep '0*'  #1个或多个前面一个字符
  ```

- .*

  ```shell
  # 匹配所有字符，包括空格
  ```

- a\\{3\\}

  ```shell
  # 重复三次a
  ```

  

## 三剑客

###### grep

> 用于搜索，并打印

- 参数
  - -i : 忽略大小写
  - -v : 取反
  - -n : 列出行号
  - -c ：只输出数量
  - -o ：只输出匹配的内容

###### awk

awk + option(-F) + pattern(条件，写在引号内) + action（{print $1})  

> https://blog.csdn.net/stpeace/article/details/46848873?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165346582616782248561796%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165346582616782248561796&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~top_positive~default-1-46848873-null-null.nonecase&utm_term=awk&spm=1018.2226.3001.4450

sed

> 