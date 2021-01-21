---
title: hexo的安装与配置
date: 2021-01-17 14:28:33
categories: hexo
tags:
- hexo
- blog
---

# Hexo的安装与初始配置
## 一、Hexo安装准备

安装Hexo的前置需求有：

+ **Node.js**：主要是使用其中的npm包管理工具（[Hexo官网](https://hexo.io/zh-cn/docs/)建议使用Node.js 12.0版本，最低Node.js 10.13） 
+ **Git**：版本控制工具，主要用于博客的版本控制、远端存储和同步

## 二、安装Hexo

安装好上述软件需求后，在shell终端输入以下指令，即可完成安装Hexo。

```
$ npm install -g hexo-cli
```

安装过程中可能遇到的问题：

1. 安装过程缓慢或卡在某个过程中不动弹。可以考虑修改npm的默认下载源，如**淘宝镜像源**。
2. Windows可以在**Git Bash**中进行类似Linux中的指令。

安装好hexo之后，如此使用：

```
$ npx hexo <command>
```

为了方便起见，我们更倾向于直接使用hexo指令

```
# hexo <command>
```

+ 方法一：可以考虑将hexo加入PATH
+ 方法二：设置alias hexo='npx hexo'
+ 方法三：(Windows系统)，使用**git bash**貌似可以不用设置环境PATH。但是存在一个小问题，直接运行**git bash**无法正确运行hexo指令，但从文件夹窗口中使用右键菜单**在此处打开git bash**可以直接使用`hexo <command>`
+ 方法三小问题的原因：`hexo <command>`需要在blog根目录下执行，因此其它路径会报错。

## 三、初始配置

自此开始，默认使用简化的形式

```
$ hero <command>
```

安装好Hexo之后，要初始化一个工作目录，类似于git中的

```
$ git init
```

执行指令

```
$ hexo init <directory>
$ cd <directory>
$ npm install
```

逐条解析：

1. `hexo init <directory>`：格式化指定路径为hexo工作目录。

```
$ ll <directory>
total 116
-rw-r--r-- 1  197609     0   _config.landscape.yml
-rw-r--r-- 1  197609  2545   _config.yml
drwxr-xr-x 1  197609     0   node_modules/
-rw-r--r-- 1  197609   641   package.json
-rw-r--r-- 1  197609 57736   package-lock.json
drwxr-xr-x 1  197609     0   scaffolds/
drwxr-xr-x 1  197609     0   source/
drwxr-xr-x 1  197609     0   themes/
```

2. `cd <directory>`：切换到工作目录
3. `npm install`：使用`npm`根据`<directory>`中的配置文件下载相关的依赖文件。

## 四、个性化设置

关于hexo博客的个性化设置，离不开`<hexo>/_config.yml`

### 1、设置博客标题、基本信息等

```
$ vim _config.yml
...
# Site
title: 设置博客标题
subtitle: ''
description: ''
keywords:
author: 作者姓名
language: 语言，中文设置为zh即可
timezone: 国内一般使用Asia/Shanghai即可
...
```

此处抛砖引玉，更多相关设置使用时实时查阅即可

### 2、设置博客主题(theme)

[Hexo官网](https://hexo.io/zh-cn/)共享了很多主题。

![Hexo官网主题](https://gitee.com/songz7026/image-pool/raw/master/note_hexo/hexo_theme.png)

任选一款进入，即可找到作者的github主页连接，在其中寻找hexo的主题仓库即可，一般在readme中都会详细介绍主题的安装方法。不过一般来说都是一样的。

```
$ cd <directory>
$ git clone <github_theme仓库的连接>
```

在此之后修改`_config.yml`中的`theme`即可

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: 这里这里这里 theme名称填上去就可以了
```

个别theme会有自己的安装方法，按照readme详细做就可以了。

## 五、查看博客

在shell中键入以下指令即可开启本地服务，从本地浏览器输入对应连接(下文对应链接为localhost:4000)即可查看
```
$ hexo server
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

![初始状态博客首页](https://gitee.com/songz7026/image-pool/raw/master/note_hexo/hexo_init_blog.png)

使用`-o`选项可以自动使用系统默认浏览器打开
```
$ hexo server -o
```