---
title: SVN
date: 2022/3/3 20:26:25
tags:
  - svn
  - 版本控制
categories:
  - 版本控制
cover: /img/svn/svn.jpg
---

# SVN

## 1.概述

1. 全称SubVersion
2. 操作简单，入门容易
3. 支持跨平台操作
4. 支持版本回退功能（时间机器）

> 软件网址：
>
> 服务端：http://www.visualsvn.com/
>
> 客户端：http://tortoisesvn.net/downloads

## 2.指令

### 1.Checkout(检出)

> 第一次与服务器连接都需要
>
> 更新数据到本地
>
> ![image-20220301171127817](/img/svn/image-20220301171127817.png)

### 2.Update(更新)

> 更新数据到本地
>
> ![image-20220301171254729](/img/svn/image-20220301171254729.png)

### 3.Commit(提交)

> 提交数据到服务器
>
> ![image-20220301171258476](/img/svn/image-20220301171258476.png)

## 3.创建版本仓库

> 1. svnadmin create Shop

![image-20220301145753875](/img/svn/image-20220301145753875.png)

> 2. 进行服务器端监管
>
> svn->svn://localhost或ip地址访问到相关数据仓库
>
> 基本语法：
>
> svnserve -d（后台运行） -r(监管目录) 版本仓库路径

![image-20220301150129792](/img/svn/image-20220301150129792.png)

> 3. 权限控制
>
>    默认情况下，SVN不允许匿名用户上传文件的，修改svnserve.conf

```conf
anon-access = write
```

> 4. SVN客户端软件安装与使用
> 5. 使用客户端软件连接SVN服务器（CheckOut)

## 4.使用详解

### 1. 检出操作

### 2. Commit操作

### 3. Update操作

## 5.使用详解（二）

### 1.图标集

> 1. 常规图标（客户端和服务器端文件一致）
>
> 2. 冲突图标（客户端和服务器端文件不一致）
>
> 3. 删除图标（服务器端数据已经删除）
>
> 4. 增加图标（当我们编写的文件已经到了提交队列）
>
> 5. 无版本控制图标（当我们编写的文件没有添加到上传对列，系统将自动提示以上图标）
>
> 6. 修改图标（客户端文件修改了，但是没有提交）
> 7. 只读图标（文件只读）
> 8. 锁定图标（当服务器端数据已锁定，客户端文件会自动显示锁定图标）
> 9. 忽略图标（客户端文件已忽略，不需要进行上传）

### 2.忽略功能

> ![image-20220301154239747](/img/svn/image-20220301154239747.png)

### 3. 版本回退



> 根据日志进行版本回退
>
> ![image-20220301154955931](/img/svn/image-20220301154955931.png)

### 4.版本冲突

> 在实际项目开发中，如果两个人同时修改某个文件，就会出现版本冲突

![image-20220301155832403](/img/svn/image-20220301155832403.png)

> 1. 合理分配项目开发时间
> 2. 合理分配项目开发模块
> 3. 通过SVN解决版本冲突问题
>    - 跟新服务器端到本地

![image-20220301160026726](/img/svn/image-20220301160026726.png)

> - 删除php以外的三个文件
> - 修改冲突文件
> - 提交文件

### 5. 配置多仓库与权限控制

> 1. 在实际项目开发中，我们可能会同时开发多个项目，如何进行多个项目监管呢
>
> svnserve 只能监管某个文件夹
>
> 使用WebApp来监管多个项目
>
> ![image-20220301160819098](/img/svn/image-20220301160819098.png)
>
> Shop项目： svn://localhost/Shop
>
> 2. 权限控制功能
>
>    - 开启权限
>
>      ![image-20220301160955177](/img/svn/image-20220301160955177.png)
>
>      - authz：告诉那些用户具有哪些权限
>
>      - passwd：认证文件：表示svn仓库中具有哪些用户和密码
>
>      - 默认情况以上两个文件都是禁用的
>
>      - 开启
>
>        #anon-access = write
>
>        ![image-20220301161228636](/img/svn/image-20220301161228636.png)
>
>      - 配置认证文件（编写密码）
>
>        ![image-20220301161346061](/img/svn/image-20220301161346061.png)
>
>      - 配置授权文件
>
>        ![image-20220301161552893](/img/svn/image-20220301161552893.png)

## 6. SVN配置和服务管理

### 1. 配置自启动服务

```cmd
sc create SVNService binpath= "D:\soft\svn\bin\svnserve.exe --service -r D:/soft/svn/WebApp" start= auto
```

### 2. 创建批处理文件

```bat
net start SVNService
net stop SVNService
sc delete SVNService
```

## 7. 模拟开发环境

### 1. 工作流程

### 2. 钩子程序

> `post-commit.tmpl`
>
> 默认可以采用批处理或shell指令来编写

### 3. 通过批处理指令编写钩子程序

> 指定服务器端SVN路径
>
> SET SVN = "D:\soft\svn\bin\svn.exe"
>
> 指定服务器工作目录
>
> SET DIR = "D:\code\shop"
>
> 通过update实时更新到dir目录中
>
> ![image-20220301164552339](/img/svn/image-20220301164552339.png)
>
> 具体使用步骤
>
> - 复制文件为post-commit.bat文件

### 4. SVN拓展程序

> 什么是BAE
>
> 百度推出的网络应用开发平台
>
> 
>
> 如何使用：
>
> BAE地址：http://bce.baidu.com/

