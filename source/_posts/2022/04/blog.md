---
title: 搭建个人的Github Page Blog
tags:
  - web
abbrlink: 232
date: 2022-04-17 00:14:55
---


## 搭建个人Github Page博客


### 1. 技术选型


一个个人博客的功能这里我就不自己介绍了。在使用Github Page之前我使用过自己购买云服务器自己纯手写代码部署个人博客，但是在使用了一年之后因为价格比较贵，所以就没有继续使用。之后尝试使用旧手机刷Linux，通过内网穿透来部署个人应用，但是存在手机不能24小时在线、并且内网穿透流量带宽受限的问题。因此还是Github这种免费的静态网页服务适合我。

对于Github Page，有多种可选的技术，包括Hexo、VuePress等。总之我选择了Hexo+Yilia来搭建自己的Github Page。

不过最后要说的一点是，技术、模板等都不是我们搭建博客的最终目的，最终都是要立足到写这个动作上的，搭建网站只是手段而不是目的。

<!-- more -->

### 2. 博客搭建

第一步环境准备，需要的本地环境包括Git，node，npm等。在这里最好把npm源换成国内的，下载速度会更快些。  
```bash
npm config set registry https://registry.npm.taobao.org
npm install -g cnpm
```

第二步新建一个空文件夹，在命令行输出一下命令，会初始化一个hexo项目在此目录下输入`hexo -g`和`hexo s`命令会在本地生成一个测试服务器，通过浏览器访问`http://localhost:4000/`.
```
npm install hexo-cli -g
hexo -v
hexo init
```

第三步配置Gothub Page，登录你的Github账号，创建一个仓库，命名为`USERNAME.github.io`，在Description填写Github Page。将仓库clone到本地，将上边文件夹的内容复制到本地仓库中。然后在项目根目录下的`_config.yml`文件夹内配置以下代码。主要作用是可以使用`hexo d`命令将项目部署到你的Github Page上。
```
deploy: 
 type: git
 repo: https://github.com:username/username.github.io.git
 branch: master
```

第四步配置主题。Hexo 博客默认主题为 landscape，若想修改主题，在 github 上找到主题，克隆到 theme 文件夹下。

在[链接](https://github.com/litten/hexo-theme-yilia)下载yilia主题，解压后复制到项目文件夹，覆盖原有文件，即可使用该主题。


### 3. 博客配置

- 归档页面
就是博客可以按照时间来进行归档，这个功能是Yilia主题自带，只要开启即可。  
修改`_config.yml`文件下的`new_post_name :year/:month/:title.md`，之后再建立新的文章会自动按照年月的格式创建子目录，之后在yilia主题的`_config.yml`中配置`归档: /archives/index.html`

- 创建关于页面  
使用`hexo new page about`命令创建新的page，会出现一个about目录，修改里边index.md文件内容即可。之后需要在yilia主题的`_config.yml`中配置`关于: /about/`

- 相册  
我参考的是这个[链接](https://www.jianshu.com/p/a9f309aaa0e0)下的方法


### 4. 常用命令

```bash
hexo c # hexo clean
hexo g # hexo generate
hexo s # hexo server 本地调试服务器
hexo d # hexo deploy 部署到github上

hexo new page XXX # 创建页面
hexo new post XXX # 创建文章
hexo new draft XXX # 创建草稿
```

+++ **点击折叠**
针对markdown渲染引擎可参考[链接](https://blog.csdn.net/qq_42951560/article/details/123596899)
+++