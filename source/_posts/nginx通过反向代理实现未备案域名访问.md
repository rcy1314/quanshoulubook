---

title: nginx通过反向代理实现未备案域名访问
tags: [nginx, 宝塔面板,反向代理]
cover: https://p.pstatp.com/origin/pgc-image/00cdb4c712ea44b7a30833075b0c637d
categories:
- [技术文档]
layout: page
aplayer: true
dplayer: true
date: 2021-09-09 10:00:00

---

本方法实现前提是8123端口（也可以是其他端口）面对互联网打开。server里面监听80端口，然后反向代理8123端口。1.其中server_name部分是我的域名可以替换成其他想要的域名2.8123是我指定的端口可以改成其他端口，最后访问http://你的域名.com   浏览器直接跳转成http://你的域名:81233.                  

本机8123端口业务一定要先配置通。下面给出server部分代码。

> ```
> server {
> listen 80;
> server_name www.你的域名 你的域名.cn;
> proxy_set_header X-Real-IP $remote_addr;
> proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
> access_log off;
> location / {
> proxy_pass <http://localhost:8123>;
> }
> }
> ```