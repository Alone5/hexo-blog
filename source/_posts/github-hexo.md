---
title: GitHub+Hexo 搭建博客
date: 2019-04-20 22:11:33
tags: github
---

# Windows GitHub+Hexo 搭建个人网站教程

## 搭建步骤：

1. GitHub创建个人仓库
1. 安装Git
1. 配置和使用Github
1. 安装Node.js
1. 安装Hexo
1. 推送网站

### 创建GitHub个人仓库

登录到GitHub,如果没有GitHub帐号，使用你的邮箱注册GitHub帐号注册完成后
{% asset_img new-repository.jpg 创建仓库 %}
{% asset_img repository-name.jpg  新建 %}
我们来New repository创建新仓库，仓库名为：你的用户名.github.io
这个**用户名**使用你的GitHub帐号名称代替  这是标准写法

### 安装 Git
简单了解一下Git：git的作用就是对文件进行版本管理，方便在不同版本进行切换修改，类似文件分不同时间备份让后需要时找回其中一份代替，不过更方便使用。
下载并安装：网址 <https://git-scm.com/download/win>

### 配置和使用GitHub
在桌面 右键 找到:
{% asset_img git-bash.jpg 命令符 %}
生成新的 SSH Key：
```
ssh-keygen -t rsa -C "输入你注册的邮箱"
```
提示： -C    “C”是大写
然后  敲三下回车就可以了
#### 添加SSH Key到GitHub
找到id_rsa.pub文件   如图：
{% asset_img id_rsa.jpg 找文件 %}
用记事本打开  复制里面的全部数据     之后打开你的GitHub    右上角头像位置如图：
{% asset_img settings.jpg Settings %}
点击第一个  New SSH key 
{% asset_img ssh-key.jpg key %}
第一行 标题 随便填（最好填上） 然后把你复制的数据粘贴到Key中     之后点Add SSH key
接下来 测试    
还在 git bash 里     输入命令  （就是这个命令不要更改 原样打上）  然后输入yes 回车    如图：
```
ssh -T git@github.com
```
{% asset_img test.jpg 测试 %}
接下来可以通过 SSH 链接到GitHub 了。 输入：
```
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```
### 安装 Node.js
Node.js下载地址：[Download | Node.js](https://link.zhihu.com/?target=https%3A//nodejs.org/en/download/) 下载安装包，注意安装Node.js会包含环境变量及npm的安装，安装后，检测Node.js是否安装成功   在命令符中输入 :
```
node -v
```

检测npm是否安装成功，在命令符中输入 :
```
npm -v
```
#### 安装 Hexo
在安装hexo之前 保证Git和Node.js 安装好之后 来安装hexo

Hexo就是我们的个人博客网站的框架， 接下来在电脑盘里创建一个文件夹，可以命名为Blog，Hexo框架与以后你自己发布的网页都在这个文件夹中。创建好后，进入文件夹中，按住shift键，右击鼠标点击命令行：

使用命令来安装 Hexo：
```
npm install -g hexo-cli 
```
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```bash
hexo init <folder> # 就是你创建的文件夹
cd <folder>
npm install
```

##### 补充执行`npm install`命令进度条停止不动问题！
安装angular cli工具时，，发现进度条一直卡住不动，原因应该是国内的网络连接npm速度较慢，甚至很多东西都无法下载安装。那么如何解决这个问题呢？
在这里我选择使用代理registry
```bash
 在执行`npm install`命令前先执行代理命令
 npm config set registry https://registry.npm.taobao.org
```

#### 创建一篇文章
```bash
hexo new 文章名
```
#### 检测我们的网站雏形  输入：
```bash
hexo serve
```
执行完命令后就可以到浏览器输入 localhost:4000 查看到了 (上面也提示了  )

#### 推送网站

上面只是在本地预览，接下来要做的就是就是推送网站，也就是发布网站，让我们的网站可以被更多的人访问。

将我们的Hexo与GitHub关联起来，打开站点的配置文件_config.yml，翻到最后修改为：
```
deploy:
  type： git
  repo： git@github.com:Alone5/alone5.github.io.git
  branch： master
```

安装Git部署插件,输入命令：

```bash
npm install hexo-deployer-git --save
```

然后  分别输入三个命令：

```bash
hexo clean 
hexo g 
hexo d
```

第三条的 hexo d 就是部署网站命令，d是deploy的缩写。完成后，打开浏览器，在地址栏输入你的放置个人网站的仓库路径，即 [http://xxxx.github.io](https://link.zhihu.com/?target=http%3A//xxxx.github.io)   例如我的 ：  alone5.github.io

这样你的博客就已经上线了，可以在网络上被访问了。

## 博客源码使用GitHub维护

1. 新建仓库 hexo-blog
2. 将本地的blog文件夹与远程仓库关联
   ```bash
   $ cd hexo-blog
   $ git init
   $ git add .
   $ git commit -m "first commit"
   $ git remote add origin git@github.com:Alone5/hexo-blog.git
   $ git push -u origin master
   ```
3. 每次本地编辑完成之后，上传到github
	```bash
	$ git add .
	$ git commit -m '注释'
	$ git push origin master 
	```
4. 将github上的文件同步到本地
	```bash
	$ git pull
	```






