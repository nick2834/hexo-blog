---
title: 搭建个人博客-hexo+github详细完整步骤
date: 2018-02-24 13:03:59
type: "tags"
tags: [ github ]
---
原本是用wordpress搭建的博客网站，但因个人原因暂时没有太多时间来写主题，就偷个懒利用hexo+github快速搭建一个简单的博客程序，下面分享一下安装步骤

### 安装node.js
此步骤忽略

安装步骤: [Node安装教程](http://www.runoob.com/nodejs/nodejs-install-setup.html)
### 安装hexo
电脑打开终端 输入以下命令
``` bash
npm install -g hexo
```
![安装hexo][1]

### 初始化Hexo
在电脑任意位置创建搭建博客的文件夹
![初始化Hexo][2]
``` bash
hexo init
```
初始化完成后是这个样子的
![初始完成截图][3]
相信到这一步应该是非常简单了

### 配置Hexo 选择自己喜欢的主题

 - 配置博客基础信息，打开文件夹根目录_config.yml
```javascript
# Site
title: Nickの博客 # 博客名
subtitle: Stay Hungry, Stay Foolish # 副标题
description: 青春就是疯狂的奔跑，然后华丽的跌倒，站起来，再跑…… # 站点描述
author: Nick-XU # 作者名
language: zh-Hans # 语言设置
timezone: Asia/Shanghai
```
![基础配置][4]

主题配置

![主题配置][5]
以next主题为例
```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
这样next主题就搭配好了

终端输入
```bash
hexo g
hexo s
```
启动后，浏览器输入localhost:4000 就能在本地预览刚刚搭建好的博客了

 - github配置
 ![github配置][6]

配置完成后，再打开博客根目录，打开_config.yml

![github配置][7]
```javascript
    deploy:
      type: git
      repo: 你刚刚创建的那个GitHub地址
      branch: master
```
注意冒号后面都有个空格

配置完成后，再输入
```bash
hexo d -g
```
完成后打开你刚刚的你的git地址，比如我的是`https://nick2834.github.io`，即可打开你现在在github上面搭建的博客了

### 域名绑定（*重点*）
域名注册购买不赘述，请自行解决
增加两个GitHub官方解析地址`192.30.252.154`，`192.30.252.153`
然后添加CNAME域名指向解析，看截图第三条，如下图
![域名解析][8]
然后打开你创建的博客目录，/source/目录下面创建一个CNAME文件，不要后缀，然后用文本打开，上面写你的域名地址，不要http，如下图
![CNAME][9]

然后再输入
```bash
hexo clean
hexo generate
hexo deploy
```
或者
```bash
hexo d -g
```
然后打开github上面的项目，打开设置
![此处输入图片的描述][10]

配置完成后，你的博客应该已经搭建完毕了


  [1]: http://p0mejgdza.bkt.clouddn.com//blog/20180224-131148.png
  [2]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-131708.png
  [3]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-132716.png
  [4]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-133943.png
  [5]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-134129.png
  [6]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-135127.png
  [7]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-135257.png
  [8]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-140357.png
  [9]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-141232.png
  [10]: http://p0mejgdza.bkt.clouddn.com//blog/QQ20180224-141658.png