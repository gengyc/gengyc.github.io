---
title: 使用Hexo和GitHub搭建博客教程，支持在新电脑同步管理项目和主题
date: 2016-11-11 10:59:32
tags: hexo
---

使用 GitHubPages + hexo 可以很便捷地搭建个人博客，githubpages提供 300M 的免费空间，做博客够用了，hexo 是一个静态页面发布框架。
下面介绍采用 gitHub + hexo 搭建个人博客，项目代码托管在GitHub，可以在不同电脑管理自己的博客。并且，自己下载的hexo主题也可以同步管理。

<!-- more -->

## 安装环境
本地安装git, node

## 安装hexo
使用npm安装hexo，官方提供的详细安装方法：
[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)

安装一些必要的插件：

```bash
$  sudo  npm install hexo-deployer-git --save    //安装Git部署器
$  sudo npm install hexo-generator-index --save 
$  sudo npm install hexo-generator-archive --save 
$  sudo npm install hexo-generator-category --save 
$  sudo npm install hexo-generator-tag --save 
$  sudo npm install hexo-server --save 
$  sudo npm install hexo-deployer-heroku --save 
$  sudo npm install hexo-deployer-rsync --save 
$  sudo npm install hexo-deployer-openshift --save 
$  sudo npm install hexo-renderer-marked@0.2 --save 
$  sudo npm install hexo-renderer-stylus@0.2 --save 
$  sudo npm install hexo-generator-feed@1 --save 
$  sudo npm install hexo-generator-sitemap@1 --save
```

## GitHub准备工作

1.在github上新建一个仓库，命名规则为 `git用户名.github.io`。仓库默认有个master分支。

2.另新建一个分支 gh_pages，并将此分支设置为 default_branch（默认分支）。
该分支用来存放和管理整个开发项目的代码，我们只需要手动管理gh_pages分支。
通过 `hexo d` 部署命令会将生成的文件自动提交到master分支。

3.将GitHub的空项目仓库clone到本地。本地新建一个分支 gh_pages, 切换到这个分支。
然后一直在此分支下开发，推送远程的时候也是推送到gh_pages同名远程分支。

```bash
$  git clone https://github.com/gengyc/gengyc.github.io
$  cd gengyc.github.io
$  git checkout -b gh_pages
```

## hexo流程

#### 1. 新建本地站点
安装完hexo后，在项目根目录(/gengyc.github.io)下，运行以下命令，会自动生成项目所需文件。
```bash
$  hexo init
$  npm install
```
完成后，文件夹中目录如下：
```
├── _config.yml
├── package.json
├── scaffolds
├── source
|    └── _posts
|
└── themes

因为要将此项目推送到远程GitHub，我们需要添加一个 .gitignore 文件，忽略掉安装的插件，因为其没必要提交到远程：

// .gitignore
node_modules
```

然后，运行 `hexo server `启动站点。

```bash
$  hexo server   // 可简写为 hexo s
```

站点默认链接为 [http://0.0.0.0:4000](http://0.0.0.0:4000)  或 [http://localhost:4000](http://localhost:4000) ，在浏览器打开此地址即可看到站点了。
保存编辑文章的时候，hexo server 会监听项目文件的改动，而且会自动刷新本地站点页面。

#### 2. 新建文章

```bash
$  hexo new '文章标题'
```

会在 source/_posts/ 目录下生成一个新的 markdown 格式的文件。
在此文件编辑和保存自己的新文章。

#### 3. 生成 html  js 站点文件

```bash
$  hexo generate    // 可简写为 hexo g
```

会在 public 文件夹中生成页面需要的完整的html js等文件。

#### 4. 发布

发布之前，先需要在根目录的 _config.yml 配置文件里关联上你自己的 github 地址。


```
// _config.yml

deploy:
  type: git
  repository: https://github.com/gengyc/gengyc.github.io
  branch: master

// 然后在命令行运行 hexo deploy，首次运行会按要求填写GitHub账号和密码
$  hexo deploy     // 可简写为 hexo d
```

这样，会把 public 文件夹里面的文件，推送到github仓库里。
然后，在浏览器中打开 [https://gengyc.github.io/](https://gengyc.github.io/) 就可以看到所搭建的线上的静态页面了。



## 安装主题
在网上找到自己喜欢的主题，clone到 themes 文件夹下。
以安装[even](https://github.com/ahonn/hexo-theme-even)主题为例，在根目录下运行如下命令：


```bash
$  git  clone  https://github.com/ahonn/hexo-theme-even  themes/even
```

修改根目录下的 _config.yml 文件，将主题名字改为 even


```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: even
```

运行以下命令就可以看到新主题的效果了：

```bash
$  hexo g    // 生成
$  hexo s    // 开启本地服务预览
$  hexo d    // 发布
```

一定要把主题文件也提交到远程仓库，可以自己自定义主题样式，换电脑后还可以同步自己配置的主题。
此时本地的git仓库跟踪不到 /themes 文件下新加的主题文件。我们只需要把 /themes/even 文件目录下的 `.git` 文件夹删除就可以了。


```bash
$  rm -rf .git
```
然后 /themes/even 下的文件就可以被 git 跟踪到了。


## 推送到远程


文章发布完后，我们把本地仓库推送到远程仓库的 gh_pages 分支。


```bash
$  git commit -am 'add hexo'
$  git push origin gh_pages
```


## 在新电脑上同步这个项目(包括主题)，步骤：


$  git clone https://github.com/gengyc/gengyc.github.io
$  cd gengyc.github.io      // 默认分支为 gh_pages
$  npm install hexo -g
// 此时直接运行npm install，不需要在项目根目录运行hexo init命令，否则会将所有文件初始化掉
$  npm install
$  hexo s        // 看是否能成功开启本地服务了
$  hexo new 'new article title'
$  hexo g
$  hexo d
$  git commit -am 'add hexo files'
$  git push origin gh_pages


end.



