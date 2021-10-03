---

title: 宝塔面板安装Flarum论坛
tags: [Flarum论坛, 宝塔面板]
cover: https://p.pstatp.com/origin/pgc-image/00cdb4c712ea44b7a30833075b0c637d
categories:
- [技术文档]
layout: page
aplayer: true
dplayer: true
date: 2021-09-16 10:00:00

---

`Flarum`是一款新式的论坛，虽然还处于不太稳定的测试版，但是它非常好看，这就够了

一些链接：[官方手册(英语)](https://flarum.org/docs/) | [官方论坛(英语)](https://discuss.flarum.org/) | [中文论坛](https://discuss.flarum.org.cn/)

> 本文使用的是Flarum 0.1.0-beta.12

> 官方不建议将Flarum用于生产环境，测试版软件有潜在的不稳定风险。

# **安装步骤**

### **环境要求**

- PHP版本 >= 7.2

  > 部分插件不支持7.4，推荐使用7.3或者7.2记得解禁putenv函数

- 内存 >= 2G

  > 论坛和插件使用composer进行安装，内存不够的话composer进程会被杀死。如果物理内存不够的话可以加2G虚拟内存。

### **宝塔面板新建站点**

创建完以后先把防跨站关了：

![](https://p.pstatp.com/origin/pgc-image/8b78747ca99e45c6acfc06a2d36ed719)



然后去网站目录删除`index.html` `404.html` `.htaccess`这几个文件。

### **命令行安装**

登陆SSH，切换到网站目录

```
#首先要确保目录是空的
ls -la
#使用composer安装
composer create-project flarum/flarum . --stability=beta
```



![](https://p.pstatp.com/origin/pgc-image/c7cf7b5559f64a7080a4dfeecdc9efbb)



如果出现红框说明环境有问题：

- 安装目录非空：

  ![](https://p.pstatp.com/origin/pgc-image/a99717b0e8c24e7aa76967eab0daaa25)

- `putenv`函数被禁用：

  ![](https://p.pstatp.com/origin/pgc-image/a5db49dee4534efea6a8c84c899dba70)

- 显示`killed`或者`已杀死`：系统内存不够，使用`free -h`查看所有内存，确保可用内存在2G以上，不够的话加点虚拟内存。

### **检查是否安装成功**

```
#更改文件所有者
chown www:www . -R
#查看文件列表
ls -la
```



如果安装没有问题，文件结构应该跟图中类似

![](https://p.pstatp.com/origin/pgc-image/a2987e56887547a3b813b1ce94fa59f3)



### **网站目录设置**

打开防跨站保护，把运行目录改成`/public`

![](https://p.pstatp.com/origin/pgc-image/289e1f9136c247419d4471f7ea4cdc52)



### **`Nginx`伪静态设置，`Apache`无需配置**

设置成下面的内容

```
location / {
  try_files $uri $uri/ /index.php?$query_string;
}
```



### **访问网站完成安装**

![](https://p.pstatp.com/origin/pgc-image/9ff8cd9872a840df927871197a082a8f)



> 表前缀可为空

### **安装完成**

![](https://p.pstatp.com/origin/pgc-image/d2e97d6cf34b4975a14c795d7c4f9574)



# **后续配置**

### **配置文件说明**

配置文件叫`config.php`：

```
<?php return array (
  'debug' => false,  /*调试模式开关*/
  'database' =>
  array (
    'driver' => 'mysql',
    'host' => 'localhost',
    'port' => 3306,  
    'database' => '数据库名',
    'username' => '数据库用户名',
    'password' => '数据库密码',
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '', 
    'strict' => false,
    'engine' => 'InnoDB',
    'prefix_indexes' => true,
  ),
  'url' => '<https://chrxw.com>',  /*论坛网址*/
  'paths' =>
  array (
    'api' => 'api',
    'admin' => 'admin',    /*管理员后台地址，强烈建议修改*/
  ),
);
```



### **升级论坛**

```
composer update --prefer-dist --no-dev -a --with-all-dependencies
php flarum migrate
php flarum cache:clear
```



### **配置插件**

官方的插件社区：[链接](https://discuss.flarum.org/t/extensions)

也是用`composer`进行配置：

```
#安装插件
composer require 插件包名
#卸载插件
composer remove 插件包名
#升级插件（升级前最好先在后台禁用）
composer update 插件包名
```

> 插件包名可以从官方论坛里获取

例如安装简体中文语言包：

```
composer require littlegolden/flarum-lang-simplified-chinese
```



> 如果遇到报错可能是某个函数被禁用，解禁以后再安装即可。

安装好以后在`https://域名/admin#/extensions`里启用插件

刷新就能看到效果了：

![](https://p.pstatp.com/origin/pgc-image/71824c15bffc4c73a4f2a8061cd01500)



> 更多插件请去官方插件社区获取