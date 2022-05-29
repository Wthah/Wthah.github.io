---
title: Nginx
date: 2022/5/5 11:21:25
tags:
  - Nginx
  - 项目部署
categories:
  - 项目部署
cover: /img/nginx/nginx.jpg
---

# Nginx

## 1. Nginx概述

###### 高并发:

Nginx是Apache服务器不错的替代品: Nginx在美国是做虚拟主机生意的老板们经常选择的软件平台之一. 能够支持高达 50,000 个并发连接数的响应

###### 负载均衡：

Nginx 既可以在内部直接支持 [Rails](https://www.w3cschool.cn/nginxsysc/kdgmwy.html) 和 [PHP](https://www.w3cschool.cn/nginxsysc/kdgmwy.html) 程序对外进行服务, 也可以支持作为 [HTTP代理](https://www.w3cschool.cn/nginxsysc/kdgmwy.html) 服务器对外进行服务. Nginx采用C进行编写, 不论是系统资源开销还是CPU使用效率都比 [Perlbal](https://www.w3cschool.cn/nginxsysc/kdgmwy.html) 要好很多.

###### 邮件代理服务器：

Nginx同时是一个非常优秀的邮件代理服务器

###### 安装简单，配置简单

###### 反向代理

一个ip，代理多个ip，代理服务网站，多个端口

![image-20220517232829330](/img/nginx/image-20220517232829330.png)

![image-20220521084105465](D:\code\BLog\source\_posts\image-20220521084105465.png)

###### 正向代理

在浏览器中配置代理服务器，通过代理服务器进行互联网访问之前访问不到的网站，代理客户

###### 负载均衡

和反向代理同理，多个端口互相协调均衡，解决并发问题。

- 轮询（默认）

- 权重

  ![image-20220521084855551](D:\code\BLog\source\_posts\image-20220521084855551.png)

- ip_hash（按ip得hash结果分配）

- fair (按响应时间来分配)

![image-20220521084357289](D:\code\BLog\source\_posts\image-20220521084357289.png)



###### 动静平衡

将动态页面和静态页面放在不同的服务器上，加快解析的速度

![image-20220517233240040](/img/nginx/image-20220517233240040.png)

###### nginx高可用

![image-20220524141141125](/img/nginx/image-20220524141141125.png)

- 安装keepalived

  ```shell
  yum install keepalived -y
  ```

- 配置文件在`/etc/keepalived`里边

- ```conf
  global_defs {
     notification_email {
       acassen@firewall.loc
       failover@firewall.loc
       sysadmin@firewall.loc
     }
     notification_email_from Alexandre.Cassen@firewall.loc
     smtp_server 192.168.200.1
     smtp_connect_timeout 30
     router_id LVS_DEVEL   //!!这个最重要
     vrrp_skip_check_adv_addr
     vrrp_strict
     vrrp_garp_interval 0
     vrrp_gna_interval 0
  }
  
  // 虚拟ip
  vrrp_instance VI_1 {
      state MASTER //MASTER or BACKUP
      interface eth0 // 网卡名字
      virtual_router_id 51 //主备的id必须相同
      priority 100 //主机较大
      advert_int 1
      authentication {
          auth_type PASS
          auth_pass 1111
      }
      // 虚拟ip地址
      virtual_ipaddress {
          192.168.200.16
          192.168.200.17
          192.168.200.18
      }
  }
  
  virtual_server 192.168.200.100 443 {
      delay_loop 6
      lb_algo rr
      lb_kind NAT
      persistence_timeout 50
      protocol TCP
  
      real_server 192.168.201.100 443 {
          weight 1
          SSL_GET {
              url {
                path /
                digest ff20ad2481f97b1754ef3e12ecd3a9cc
              }
              url {
                path /mrtg/
                digest 9b3a0c85a887a256d6939da88aabd8cd
              }
              connect_timeout 3
              nb_get_retry 3
              delay_before_retry 3
          }
      }
  }
  
  virtual_server 10.10.10.2 1358 {
      delay_loop 6
      lb_algo rr
      lb_kind NAT
      persistence_timeout 50
      protocol TCP
  
      sorry_server 192.168.200.200 1358
  
      real_server 192.168.200.2 1358 {
          weight 1
          HTTP_GET {
              url {
                path /testurl/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl2/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl3/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              connect_timeout 3
              nb_get_retry 3
              delay_before_retry 3
          }
      }
  
      real_server 192.168.200.3 1358 {
          weight 1
          HTTP_GET {
              url {
                path /testurl/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334c
              }
              url {
                path /testurl2/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334c
              }
              connect_timeout 3
              nb_get_retry 3
              delay_before_retry 3
          }
      }
  }
  
  virtual_server 10.10.10.3 1358 {
      delay_loop 3
      lb_algo rr
      lb_kind NAT
      persistence_timeout 50
      protocol TCP
  
      real_server 192.168.200.4 1358 {
          weight 1
          HTTP_GET {
              url {
                path /testurl/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl2/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl3/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              connect_timeout 3
              nb_get_retry 3
              delay_before_retry 3
          }
      }
  
      real_server 192.168.200.5 1358 {
          weight 1
          HTTP_GET {
              url {
                path /testurl/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl2/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              url {
                path /testurl3/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334d
              }
              connect_timeout 3
              nb_get_retry 3
              delay_before_retry 3
          }
      }
  }
  
  ```

  

## 2. 安装nginx

###### 依赖库文件

```shell
yum -y install make pcre pcre-devel zlib zlib-devel gcc gcc-c++ automake autoconf libtool  openssl openssl-devel
```

###### 关闭防火墙

```shell
systemctl stop firewalld.service
```

###### 关闭selinux

```shell
setenforce 0 
```

###### 下载编译

```shell
cd /usr/local/src
#下载
wget https://nginx.org/download/nginx-1.19.10.tar.gz
tar -xvf nginx-1.19.10.tar.gz
cd nginx-1.19.10
#编译安装
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module 
make && make install
#查看版本
/usr/local/nginx/sbin/nginx -v
```

###### nginx命令行参数

```shell
# 为nginx指定一个配置文件，来代替缺省
-c 
# 不运行，只用于测试配置文件
-t
# 显示nginx版本
-v
# 显示版本并且显示配置参数
-V
```

###### nginx控制信号

可以使用信号系统来控制主程序

默认将主进程pid写入`/usr/local/nginx/nginx.pid`文件中

![image-20220517183650154](/img/nginx/image-20220517183650154.png)

###### nginx启动、停止、重启命令

`nginx启动`

```shell
sudo /usr/local/nginx/nginx
```

`从容停止`

```shell
kill -quit nginx pid
```

`快速停止`

```shell
kill -term nginx pid
# 强制停止
kill -9 nginx pid
```

嫌麻烦可以不查看pid直接在`nginx.conf`中设置`pid`

```shell
kill -quit `cat /usr/local/nginx/nignx.pid`
```

`重启`

- 简单型：关闭进程，修改配置后重启进程
- 重新加载配置文件，不重启进程，不会停止处理请求 使用`-HUP`的参数配置
- 平滑更新nginx二进制，不停止处理请求 -USR2

## 3. nginx优化

###### hash max size && hash bucket size

###### 事件模型

## 4. 核心模块

### 主模块--控制Nginx的基本功能的指令

- daemon

- master_process 和deamon一样只用于开发调试老死不相往来

- debug_points 调试断点

- error_log

- include 用于配置文件的包含

  ```conf
  include vhosts/*.conf;
  ```

- pid

  ```shell
  kill -HUP `cat /var/log/nginx.pid`
  ```

- user

  指定Nginx Worker进程运行用户



## 5. 基本入门

###### 配置文件结构

- 简单指令

  由名称和参数组成以分号结尾

- 块指令

  由名称和参数组成不以分号结尾而是{} 例如events、http、server、location

###### 提供静态内容

```conf
http{
	server{
		location / {
			root /data/www
		}
		location /images/ {
			root /data
		}
	}
}
```

###### log

> /usr/local/nginx/logs

###### 简单的代理服务器

```
http{
	server{
		listen 8080;
		root /data/upl; !!!!location没有时才使用这个root
		location /{
			proxy_pass http://localhost:8080;
		}
		location /images/{
			root /data;
		}
		// 第二个可以修改为
		location ~ \.(gif|jpg|png)${
			root /data/images
		}
	}
}
```

###### 设置快速通用网关接口代理FastCGI

使用FastCGI服务器最基本的nginx配置包括使用 [fastcgi_pass](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html#fastcgi_pass) 指令替代 `proxy_pass`指令，以及使用 [fastcgi_param](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html#fastcgi_param) 指令来设置传递给FastCGI服务器的参数。假设FastCGI服务器可以在 `localhost:9000` 上访问。以上一节中的代理配置为基础，将 `proxy_pass` 指令替换为 `fastcgi_pass` 指令，并将参数更改为 `localhost:9000` 。在PHP中， `SCRIPT_FILENAME` 参数用于确定脚本名称，  `QUERY_STRING` 参数用于传递请求参数。得到的配置将是:

```
server {
     location / {
         fastcgi_pass  localhost:9000;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_param QUERY_STRING    $query_string;
     }
     location ~ \.(gif|jpg|png)$ {
         root /data/images;
     }
 } 
```

