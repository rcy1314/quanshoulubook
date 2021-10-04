---

title: HEXO零代码快速搭建
tags: [HEXO, 博客搭建]
index_img: https://p.pstatp.com/origin/pgc-image/0198ac9747c84ae59f992bda50118bbc
banner_img: https://p.pstatp.com/origin/pgc-image/0198ac9747c84ae59f992bda50118bbc
categories:
- [技术文档]
date: 2021-09-16 10:00:00

---

HEXO是一套快速、简洁且高效的博客框架，官方包括网上已有各种搭建教程，这里仅介绍我的博客站搭建过程，中间不需要输入任何代码。

# 前提

你需要有一个Github账户，需要用这个账户登录Vercel，实现连通。

# 第一步

用github账户登录[Vercel官网](https://vercel.com/)

![](https://p.pstatp.com/origin/pgc-image/a164e6197cfd485b8d01f28c95749649)

# 第二步

新建模版文件

![](https://p.pstatp.com/origin/pgc-image/41de82c6488644108277814460d394bb)

导入官方已准备好的模版库

![](https://p.pstatp.com/origin/pgc-image/4d7ea6d607074d05a790576244e09826)

选择HEXO模版

![](https://p.pstatp.com/origin/pgc-image/d8941bfd99134596b142c874aaca1e0d)

导入同步到自己的Git仓库，名字新建即可

![](https://p.pstatp.com/origin/pgc-image/1d2760f683624f63b21b64622e14d847)

# 第三步

转到刚刚新建的仓库，更改管理访问权限，如果想仓库一直是私有的，则需要建立SSH公匙（公共的不需要）

![](https://p.pstatp.com/origin/pgc-image/212e5577754347c48e7b23fb536c51ad)

# 第四步

这时Vercel已经构建好Hexo并提供了一个永久域名，

我们还可以到Vercel的HEXO仓库下添加一个属于自己的解析域名

![](https://p.pstatp.com/origin/pgc-image/dcf196e778814fcba83ff7003dad1306)

# 最后

到Github官方下载客户端或使用其他GIT工具，把github下Hexo仓库git到本地，这时你已经可以新建md文章及下载其他主题更改博客站配置了。

下图为我用的的fluid主题

![](https://p.pstatp.com/origin/pgc-image/55eae005deed4c5ca3d746f5eb547017)