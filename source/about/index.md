---
title: 关于
top_img: /img/50.png
date: 2022-03-05 14:28:35
type: about
comments: false
---

## 项目地址

> github地址: https://github.com/Wthah/Wthah.github.io
>
> 因为要在不同的电脑上边操作我的博客，所以我创建了两个分支
>
> `hexo分支`：主要用于存储文件、静态资源、配置
>
> `master分支`：用于存储编译后的静态页面

## 安装本项目

> `安装前提`：安装git+Typora(最好，编写文章用)+安装nvm（管理npm版本，以及npm和nodejs版本对应问题）
>
> `git安装`：https://blog.csdn.net/mukes/article/details/115693833?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165707528516782246472064%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165707528516782246472064&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-115693833-null-null.142^v31^pc_rank_34,185^v2^tag_show&utm_term=%E5%AE%89%E8%A3%85git&spm=1018.2226.3001.4187
>
> `nvm安装`：https://blog.csdn.net/qq_30376375/article/details/115877446?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165707763516782388096749%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165707763516782388096749&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-115877446-null-null.142^v31^pc_rank_34,185^v2^tag_show&utm_term=nvm%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187
>
> `Typora安装`：https://typoraio.cn/
>
> Typora好像收费了，博主是在淘宝买了一个激活码，太难找了激活码，不知道你在看的时候有没有成熟的激活方法了
>
> 建议：最好是有一定编程基础的人进行安装

```shell
# clone后切换到hexo分支，用于操作静态文件
git clone https://github.com/Wthah/Wthah.github.io
git branch -a
git checkout hexo
# 补充因为.gitignore忽略的组件
npm install hexo
npm install
npm install hexo-deployer-git --save
# ！！！删除.git目录！！！
# 在github建库 命名为用户名.github.io
# 在clone下来的目录执行
git init
git remote add origin "你的git仓库地址"
# 创建hexo分支,并上传
git branch hexo
git add .
git commit -m "msg"
git push origin hexo
# 可以试试看现在本地能不能跑起页面，登录localhost:4000
hexo s
# 修改根目录下的_config.yml,修改deploy: repository为你的仓库地址，保存
vim _config.yml
# 编译，并上传静态页面到master分支
hexo g -d
```

## 申请Gitee Pages服务（Gitee仓库）

![image-20220706144551235](/img/about/image-20220706144551235.png)

> 最后访问路径：开启服务后会给page地址

## Github仓库

> 命名的时候命名为 用户名.github.io

![image-20220706144716251](/img/about/image-20220706144716251.png)

> 最后访问路径： https://wthah.github.io/

## 日常更新

```shell
# 日常更新指令
git pull origin hexo
# 推送到hexo分支
git add .
git commit -m "msg"
git push origin hexo
# 最后编译部署到master分支
hexo g -d
```
