---
title: "浅析 URL"
date: 2020-05-15T17:48:49+08:00
draft: false
---

## URL的组成

### URL也叫统一资源定位符,由 `协议 + 域名 + 端口号 + 路径 + 查询参数 + 锚点` 组成

* 协议
  
    HTTP(基于TPC和IP两个协议)协议,还有`FTP协议`等等。

* 域名

    * 域名是对`IP`的别称
    * 一个域名可以对应不同的`IP`,这叫`均衡负载`,防止一台机器扛不住
    * 一个`IP`可以对应不同的域名,这叫`共享主机`,资金不够的开发者会这么做.

* 端口号

    一台机器可以提供很多服务,每个服务的号码就是端口

    * `HTTP` 服务最好使用 `80端口`
    * `HTTPS` 服务最好使用 `443端口`
    * `FTP` 服务最好使用 `21端口`
    * 一共有`65535`个端口(基本够用)
    * 0-1023号端口是留给系统使用的,你只有拥有了管理权限,才能使用这1024个端口
    * 其他端口都可以给普通用户使用
    * 端口若被占用,则只能换一个端口

* 路径

    是服务器上的一个目录或地址,以这个地址访问不同的页面,不是URL必须的部分

* 查询参数

    在问号后面拼接的一串字符,可以查询相关信息,可以在同一个页面,查看不同的内容

* 锚点
  
    定位页面的位置

    * 锚点看起来有中文,实际上不支持中文
    * 锚点无法在`Network`上看到的,因为锚点不会传给服务器

## DNS

### Domain Name System 域名系统

作用:可以将域名解析为`IP`,浏览器通过该`IP`的`80/443`端口发送请求,访问页面.

nslookup 可以查询`IP`地址和`DNS`记录,查看域名解析是否正常.

![nslookup](/images/nslookup.png)

## IP

### 当你买了一个路由器,并连接了租用的宽带,你就有了一个属于自己的`IP`

  * 路由器没有固定的`外网IP`,如果你重启了路由器,你有可能会被重新分配一个`外网IP`
  * 路由器会在你家里创建一个`内网`,内网中的设备使用`内网IP`

### 路由器的功能

  1. 内网设备可以相互访问,不能直接访问外网,必须经过路由器中转
  2. 外网设备可以相互访问,无法直接访问你的内网,外网设备想把内容发送到内网,必须经过路由器
  3. 因此路由器也叫`网关`

### 特殊的IP

 *  127.0.0.1  表示自己
 *  localhost  表示本地
 *  0.0.0.0  不表示任何设备

### ping

在命令行中`ping IP`,可以检查你的网络状况.

![ping](/images/ping.png)


## 域名

  ### 域名是对`IP`的别称,分为顶级域名,二级域名和三级域名

 * `com`是顶级域名
 * `baidu.com` 是二级域名(俗称一级域名)
 * `www.baidu.com`是三级域名(俗称二级域名)
 * 它们是父子关系
 * `baidu.com`和`www.baidu.com`是两个不同的域名
 * `www`是多余的