---

title: Hexo 常用命令
tags: [HEXO]
index_img: https://p.pstatp.com/origin/pgc-image/dec8c9c9821347e2ac3c441b44b936dd
banner_img: https://p.pstatp.com/origin/pgc-image/dec8c9c9821347e2ac3c441b44b936dd
categories:
- [技术文档]
date: 2021-09-16 10:00:00

---

## 常用命令

- `hexo n "title"` 创建新文章；

- `hexo g -d` 生成静态文件（gnerate），且文件生成后立即部署网站（deploy）；

- `hexo publish "title"` 发表草稿；

- `hexo s` 启动服务器。默认情况下，访问网址为： `http://localhost:4000/`；

- `hexo clean` 清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)；

  在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令；

- `hexo version` 显示 Hexo 版本；

- `npm list` 查看插件；

- `npm uninstall 插件名称` 卸载插件；

## 备份博客

使用 **[hexo-git-backup](https://github.com/coneycode/hexo-git-backup)** 插件进行备份操作。

### 安装插件

如果 Hexo 版本是 3.x.x，则应按如下方式安装：

```
$ npm install hexo-git-backup --saveCopy
```

### 插件升级

如果使用 –save 安装，则在更新时必须先删除。

```
$ npm remove hexo-git-backup
$ npm install hexo-git-backup --saveCopy
```

### 插件配置

在博客目录根的 _config.yml 中增加相应配置。

```
backup:
    type: git
    theme: coney,landscape,xxx
    repository:
       github: git@github.com:xxx/xxx.git,branchName
       gitcafe: git@github.com:xxx/xxx.git,branchNameCopy
```

其中 branchName 为在 GitHub 中博客仓库中自行创建的分支名称。若需要备份主题则在 theme 处添加主题名称。

> 插件更多配置细则请参考官方文档。

### 插件使用

`hexo b` 备份博客到 GitHub 上对应的 backup 分支。

**建议每次发布博客 `hexo d` 的时候都同时 `hexo b` 对博客进行备份更新。**

## 恢复博客

### 安装 Hexo

在新环境下根据 [Hexo官方文档](https://hexo.io/zh-cn/) 安装配置好Hexo环境，

安装博客部署到 GitHub 所需要的插件：

```
npm install --save hexo-deployer-gitCopy
```

### 覆盖本地文件

从之前插件备份到 GitHub 分支中下载博客文件到本地，并覆盖本地博客文件。

此外，可以只下载其中的 `config.yml，theme/，source/，scaffolds/，package.json，.gitignore` 这六个文件覆盖。

此时运行如下三连进行测试博客是否迁移成功。

```
hexo clean
hexo g
hexo s
```