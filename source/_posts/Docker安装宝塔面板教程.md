---

title: Docker安装宝塔面板教程
tags: [Docker, 宝塔面板]
cover：https://www.nbmao.com/wp-content/uploads/2020/11/20201104232251-76e82.jpeg
categories:
- [技术文档]
layout：page
aplayer：true
dplayer：true
---



宝塔界面建站非常方便，但是没有官方的docker版，pch18大神移植到docker了，正好可以用在unraid上面，不过有一些坑需要注意，下面试安装步骤

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232251-76e82.jpeg)

1.首先docker设置里先启用主机访问自定义网络和保留用户定义的网络

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232252-2c8f7.jpeg)

2.打开 unRAID 的 Docker 页面，并选择添加容器

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232253-a0e90.jpeg)

3.填写配置容器参数

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232253-553e9.jpeg)

版本命名说明（根据自己的需求替换镜像地址）

1. pch18/baota或pch18/baota:latest 等同pch18/baota:lnmp
2. pch18/baota:lnmp 为最新版本的官方纯净安装的基础上安装nginx,mysql,php
3. pch18/baota:lnp 为官方版本纯净安装的基础上安装nginx,php(不内置mysql,用于外置数据库的环境)
4. pch18/baota:lamp 为官方版本纯净安装的基础上安装apache,php
5. pch18/baota:lap 为官方版本纯净安装的基础上安装apache,php(不内置mysql,用于外置数据库的环境)
6. pch18/baota:clear 为官方版本纯净安装, 不默认安装nginx,mysql,php等程序

4.映射路径

第一个路径首次配置，容器路径不要直接指定www，换成别的，比如我的other,后面再改回来

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232253-2f92e.jpeg)

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232254-5b75c.jpeg)

5.设置好后，点击 APPLY，这里会根据你选择的镜像，自动进行下载

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232255-df636.jpeg)

安装成功后，打开浏览器输入上面设置的ip再带上端口号8888就可以访问了，比如我的：192.168.1.25:8888默认的用户名是：username ，默认密码是：password

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232256-89f1b.jpeg)

但是，这里还没彻底完成，因为到这里，虽然能进行正常运行了，但是比如你添加了一些软件或者做了宝塔面板的升级，然后你去重新编辑该 Docker，它是不会保存这些设置的，我们还需要进行一些操作。

6.登入进去后点击宝塔面板左侧的文件，在右侧的路径，选择 根目录-www，然后选定除去 wwwroot 文件夹以外的所有文件夹，点击右上角的复制

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232256-3b11d.jpeg)

然后选择根目录 - other 文件夹，右上角点击 粘贴所有，最终结果如图

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232257-b765d.jpeg)

5、接下来，我们重新编辑下 宝塔 Docker的配置，只需要修改文件夹路径即可

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232257-eb144.jpeg)

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232257-d1c30.jpeg)

修改完成，点击 APPLY 保存，到此，宝塔已经可以完整正常使用了，并且每次做了宝塔内的升级、安装等操作后，配置都会进行保存。

其他优化设置

1.这里你折腾了几次，宝塔面板的机制是每一次都会在docker 的img文件里面 新建一个卷 volume每次 大概4G 容量，而docker映像默认是20G，如果你没有修改的话。

所以这里我们需要删除无用的卷。调用unraid的终端 ，命令如下

docker volume prune

回车之后输入y 确认删除

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232258-a9d7e.jpeg)

2.宝塔面板的webui链接设置，高级选项在webui项填入值：

http://[IP]:[PORT:8888]

![](https://www.nbmao.com/wp-content/uploads/2020/11/20201104232258-cada8.jpeg)

3.图标美化修改

设置上图中Icon URL地址即可

最后列出本项目的源地址

github issue传送门: [https://github.com/pch18-docker/baota](https://www.ruoyer.com/wp-content/themes/begin/go.php?url=aHR0cHM6Ly9naXRodWIuY29tL3BjaDE4LWRvY2tlci9iYW90YQ==)dockerHub传送门: [https://hub.docker.com/r/pch18/baota/](https://www.ruoyer.com/wp-content/themes/begin/go.php?url=aHR0cHM6Ly9odWIuZG9ja2VyLmNvbS9yL3BjaDE4L2Jhb3RhLw==)作者主页传送门: [https://pch18.cn/archives/docker-baota.html](https://www.ruoyer.com/wp-content/themes/begin/go.php?url=aHR0cHM6Ly9wY2gxOC5jbi9hcmNoaXZlcy9kb2NrZXItYmFvdGEuaHRtbA==)