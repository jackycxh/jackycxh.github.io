---
title: 如何使用Hexo在Github上搭建静态博客
date: 2017-02-17 12:00:19
tags:
 - Hexo
 - Github
featuredImage: "/images/how-to-blog-with-hexo-over-github.png"
---

Hexo 是一个快速，简单的静态博客框架。使用者在本地使用markdown格式编辑博客文章，然后使用Hexo在几秒时间内生成静态HTML文件，并发布到博客网站上。Hexo基于Node.js；功能不如 Wordpress 强大，但对于大部份人来说足够了。

Github 是一个开源代码管理、分享的网站平台。Github提供了一个Pages功能，有300M免费空间做为静态网站展示。本用于介绍托管在Github上的项目，也可以用来搭建个人静态博客。

本文详细介绍了如何安装配置Hexo博客框架，并发布到Github Pages上。


##  本地系统环境配置

本文所有本地设置都是基于ArchLinux操作系统。

Hexo是基于Node.js的。而Github和Hexo都需要git的支持。本地系统环境主要就是安装Node.js和git客户端；Hexo是在安装node.js后使用npm进行安装的。

### 安装Node.js

采用ArchLinux系统带的软件包管理器pacman安装

```bash
sudo pacman -S nodejs npm
```

其它系统可以参考[下载、安装Node.js](https://nodejs.org/en/download/package-manager)

### 安装git客户端

```bash
sudo pacman -S git
```

其它系统可到[Git-Downloads](https://git-scm.com/download)下载安装

### 安装Hexo

使用npm安装hexo-cli时需要全程root权限，使用sudo还是会有权限不足的提示，必须使用su进入root用户再安装才能成功。

```bash
su
npm install hexo-cli -g
exit
```

## 本地博客搭建

### 新建博客文件夹

在本地建立一个文件夹用来存放博客系统。

```bash
mkdir blog
```

### 初始化博客

```bash
cd blog
hexo init
npm install
```

命令 *hexo init* 会在当前文件夹建立博客所需的所有文件。

### 生成博客静态HTML文件

```bash
hexo generate
```

此命令(可缩写为 *hexo g* ）会在当前文件夹下建立public子文件夹，并在里面生成博客所有的静态HTML文件。

### 本地测试

```bash
hexo server
```

可缩写为 *hexo s* ，启动本地测试web服务器。

用浏览器打开http://localhost:4000 就可以预览博客，其中包含一篇Hello World。

## 部署到Github

现在博客系统已经在本地成功运行。下一步就是发布到远徎WEB服务器中。发布的本质实际上就是把博客文件夹中public子文件夹里面所有文件传到WEB服务器的目录中。可以手工以自己喜欢的任何方式上传，也可以用Hexo的部署插件(deployer)。因为我们的博客要发布到Github Page中，我们可以使用Hexo的git部署插件(hexo-deployer-git)。

### 首先要有一个github帐号和Pages仓库

进入http://github.com, 点sign up，按提示一步步注册自己的帐号。

注册好帐号并登录进去后，在github首页右上角**+** -> **New Repository**，项目名称（Project Name）里面填<username>.github.io。<username>为你的github帐号名。其它信息可以不填。点击**Create Repository**完成。

### 设定本地git客户端的用户信息

```bash
git config --global user.name "User Name" # 此用户名为真名，不需要与github里面的相同。
git config --global user.email "user@abc.com" # 此处所填邮箱地址需与github帐号的邮箱相同。
```

### 安装git部署插件

使用npm安装

```bash
npm install hexo-deployer-git --save
```

### 配置github部署

github部署支持两种连接方式。一种是SSH，还有一种是HTTPS。

#### SSH 配置

采用SSH进行部署的优点是配置好之后每次进行部署时不需要输入用户名、密码。缺点是初始配置比较复杂，而且无法通过代理服务器进行部署。

##### 生成SSH密钥

```bash
ssh-keygen -t rsa -C "user@abc.com" # 此处输入的邮箱地址需与github帐号的邮箱相同
```

系统提示输入保存密钥的文件名，保持默认就行。

再回车后，系统会再提示输入密码。建议留空，否则后面每次更新部署博客时都需要输入密码。只要<span id="ks">[保证密钥文件的安全](#keysafe)</span>就行了。

##### 添加公钥到github帐户中

本地SSH密钥生成后，需要把公钥添加到github帐户设置中。

1. 复制本地主目录$HOME/.ssh/id_rsa.pub文件中所有的内容
2. 登录github, 右上角选择**Settings**，-> **SSH and GPG Keys** -> **New SSH key**
3. Title里面输一个描述，Key里面把is_rsa.pub文件里面的内容粘贴进去
4. 点**Add SSH key**

##### 测试SSH连接

输入下面命令测试设置是否成功

```bash
ssh -T ssh@github.com
```

第一次连接时会提示

```
Are you sure you want to continue connecting (yes/no)?
```

输入yes,然后会看到

```
You've successfully authenticated, but GitHub does not provide shell access.
```

说明你已经可以成功通过SSH连接到Github了。下面配置Hexo通过SSH部署博客到Github Pages中。

##### SSH部署设置

编辑博客文件夹中的_config.yml。光标移到最后deployment段落

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:username/username.github.io.git  # 替换username为你的github用户名
  branch: master
```

#### HTTPS配置

采用HTTPS进行部署的话每次发布时都需要输入github的用户名和密码。优点是初始配置简单，可以<span id="ps">[通过代理服务器](#proxy)</span>进行部署。比如我们公司的网络是只允许HTTP、HTTPS访问Internet，而且还要通过代理服务器。

初始配置只需要编辑博客文件夹中的_config.yml。光标移到最后deplyment段落

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git  # 替换username为你的github用户名
  branch: master
```

### 部署博客到Github Pages中

使用hexo git部署插件把博客部署到github pages中。

```bash
hexo deploy # 可缩写为 hexo d
# 如果是采用HTTPS方式，每次部署时会提示要用户名和密码。
```

然后就可以用浏览器打开 http://username.github.io 浏览你的博客了。

## 博客文章管理

Hexe里面所有博客文章都是采用Markdown格式编写，源文件都保存在source/_posts文件夹里。文章管理实际上就是管理此文件夹里面的文件。

新建一篇博客文章可以在此文件夹里新建一个.md文件。或在博客文件夹里面执行

```bash
hexo new <title> # 文章标题
```

删除、修改一篇博文也就是删除、修改对应的.md文件。

做任何改动后都需要重新生成，部署

```bash
hexo generate # 可缩写为 hexo g 
hexo deploy   # 可缩写为 hexo d
# 以上两行可以一起缩写为 hexo g -d 或 hexo d -g
```

## 附录

<span id="keysafe"></span>

### SSH密钥文件的权限设置

设置SSH密钥文件及所在文件夹只有本人可读写，其它任何人无权限。

```bash
cd ~
chmod 700 .ssh
cd .ssh
chmod 600 *
```

[返回](#ks)

<span id="proxy"></span>

### Git代理服务器设置

```bash
git config --global http.proxy "proxyserver:port"   # 填入代理服务器地址及端口 
git config --global https.proxy "proxyserver:port"  # 填入代理服务器地址及端口
```

[返回](#ps)
