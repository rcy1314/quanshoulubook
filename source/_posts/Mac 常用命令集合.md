---

title: Mac 常用命令集合
tags: [Mac命令]
cover: https://p.pstatp.com/origin/pgc-image/00cdb4c712ea44b7a30833075b0c637d
categories:
- [技术文档]
layout: page
aplayer: true
dplayer: true
date: 2021-09-15 10:00:00

---

开始 记住命令多用 Tab 键，命令自动补齐，提高开发效率！ 由于 Mac OS 是基于 Unix 的内核，所以与各种 Linux 系统命令大同小异。

su

# 一般使用sudo su，切换root管理员身份

$ sudo su

# 同样是切换用户，su - 用户，切换指定用户身份

$ su - root clear

# 没啥好说的，清屏命令

$ clear mkdir & touch

# 创建目录

$ mkdir 目录名

# 创建文件

$ touch 文件 微拓展

# 创建多级目录

$ mkdir -p 目录/目录/... 删除

# 删除目录下所有的文件及目录

# r 表示递归，f表示强制删除

$ rm -rf 目录/*

# 删除目录下以.java结尾的文件

$ rm -rf 目录/*.java

# 强制删除目录及目录下所有的文件及目录

$ rm -rf 目录 cd

# 进入某个目录

$ cd 目录

# 返回上一级目录

$ cd .. ls

# 列出目录下的文件和目录

# 什么都不写默认是当前目录

$ ls 目录 微拓展

# 隐藏文件默认是不显示的 -a 表示列出所有文件和目录

$ ls -a

# 查看文件详细信息

$ ls -l 等同于 ll $ ls -al pwd

# 没啥好说的，显示当前目录位置

$ pwd 解压缩命令 注：tar 是打包，不是压缩！

# tar 解包

$ tar xvf FileName.tar

# tar 打包

$ tar cvf FileName.tar DirName

# .gz 解压方式一

$ gunzip FileName.gz

# 解压方式二

$ gzip -d FileName.gz

# 压缩

$ gzip FileName

# .tar.gz 和 .tgz 解压

$ tar zxvf FileName.tar.gz

# 压缩

$ tar zcvf FileName.tar.gz DirName 移动 & 拷贝

# 将文件或目录移动到另一目录下

$ mv 文件或目录 目录 ifconfig

# 显示网卡和网络的信息

$ ifconfig

# 显示当前机器的ip地址

$  ifconfig en0 | grep "\<inet\>" | awk "{print \$2}" 修改文件权限

# R 表示递归，whoami表示当前用户，后面是需要修改权限的目录或文件

$ sudo chown -R $(whoami) /usr/local/sbin vi & vim

# 文本编辑器

$ vi 需要编辑的文件 微拓展 进入 vi 或 vim 编辑界面，默认是阅读模式模式，键盘按 i 才会进入编辑模式，才能对文件进行编辑操作。 在阅读模式同样可以快捷对文本进行操作 o 在光标所在行的下一行进行编辑 yy 拷贝一行 p 粘贴 dd 删除一行 /搜索内容模糊查询，搜索内容后 n 表示下一个，p 表示查找上一个 文本编辑完成后按 ESC 退出编辑模式，:wq 保存并退出，q! 强制退出并不保存。

ssh

# 远程登陆，默认自动送到远程主机的22端口

$ ssh 用户名@ip地址

# 指定远程登陆的端口号

$ ssh -p 8088 用户名@ip地址 免密登陆

# 生成密钥对，-t表示类型选项，这里采用rsa加密算法

$ ssh-keygen -t rsa 此时目录下生成了.ssh 隐藏文件夹，我们 cd 到目录，可以看到默认生成三个文件 id_rsa,id_rsa.pub,known_hosts。将 id_rsa.pub 公钥拷贝到远程登陆的机器，此后远程登陆就不需要再输入密码。

ssh-copy-id <username>@<远端服务器IP地址> 如果想要快捷登陆可以直接在.bash_profile 文件中添加别名

alias xx='ssh user@ip-address'

远程拷贝命令 scp可以实现服务器与服务器之间的数据拷贝 基本语法： scp     -r      $pdir/$fname  $user@hadoop$host:$pdir/$fname 命令    递归    要拷贝的文件路径/名称   目标用户@主机:目的路径/名称

例子 scp -r /opt/moudle root@slave:/opt/moudle 或者从目标主机拉取文件 scp -r root@master:/opt/moudle /opt/moudle 查看进程 $ ps          # 查看正在运行中的进程 $ ps -ef      # 查看所有进程的详细信息 $ ps -ef|grep nmon    #搜索nmon相关的进程 $ kill -9 进程号   #杀死进程 查找文件 $ find 文件路径 参数 $ find . -name "test"        # 查看当前目录下(包含子目录)搜索文件包含test的文件 $ find ~/Project/ \! -name "*.py" -print        # 搜索文件后缀不是.py的文件并打印出来，这里的！用了\转义符 $ find .-newer demo -user hudu -print    #当前目录下搜索属于用户hudu，且比文件demo的时间新的文件 $ find . \! \( -newer demo -user hudu \) -print    #是搜索既不属于用户hudu也不比文件demo的时间新的文件。 $ find . -type f -exec echo {} \    # 打印当目录下所有的文件。 查看被占用的端口号 $ lsof -i tpc:1099 $ kill -9 99178