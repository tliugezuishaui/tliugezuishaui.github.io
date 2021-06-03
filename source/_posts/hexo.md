---
title: Hexo + Next + Github 博客搭建
categories: Hexo
date: 2021-06-01 15:26:28
tags:
---
<meta name="referrer" content="no-referrer"/>

<!--more-->

## Hexo搭建步骤
  1. 安装Git
  2. 安装Node.js
  3. 安装Hexo
  4. GitHub创建个人仓库
  5. 生成SSH添加到GitHub
  6. 将hexo部署到GitHub
  7. 其余补充

### 1. 安装Git
  windows：到git官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个Git Bash的命令行工具

### 2. 安装nodejs
  Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。
  windows：[nodejs](https://nodejs.org/en/download/)选择LTS版本就行了。

### 3. 安装Hexo
  前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后cd到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

  输入命令：`npm install -g hexo-cli`

  接下来初始化Hexo
  `hexo init myblog`

  然后cd进入文件夹 进行安装依赖：`npm install`

  新建完成后，指定文件夹目录下有：
  * node_modules: 依赖包
  * public：存放生成的页面
  * scaffolds：生成文章的一些模板
  * source：用来存放你的文章
  * themes：主题
  * ** _config.yml: 博客的配置文件**

  接下来运行hexo
  ```
    hexo g
    hexo server
  ```
  Hexo基本就完成了...

### 4. GitHub创建个人仓库
  进入[github](https://github.com/)网站，在gitHub.com中看到一个New repository，新建仓库

  创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxxx.github.io，其中xxx就是你注册GitHub的用户名。

  点击create repository。

### 5. 生成SSH添加到GitHub
  在git bash中进行如下操作
  ```
  git config --global user.name "yourname"
  git config --global user.email "youremail"
  ```
  这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户

  可以用以下两条，检查一下你有没有输对
  ```
  git config user.name
  git config user.email
  ```

  然后创建SSH,一路回车
  `ssh-keygen -t rsa -C "youremail"`

  ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

  而后在GitHub的setting中，找到SSH keys的设置选项，点击New SSH key
  把你的id_rsa.pub里面的信息复制进去。

  在gitbash中，查看是否成功 `ssh -T git@github.com`

### 6.将hexo部署到GitHub
  这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 _config.yml，翻到最后，修改为
  YourgithubName就是你的GitHub账户
  ```
  deploy:
  type: git
  repo: git@github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
  ```

  这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。
  `npm install hexo-deployer-git --save`
  然后
  ```
  hexo clean
  hexo generate
  hexo deploy
  ```

  其中 hexo clean清除了你之前生成的东西，也可以不加。
  hexo generate 顾名思义，生成静态文章，可以用 hexo g缩写
  hexo deploy 部署文章，可以用hexo d缩写

  这样就可以在网站看到博客了

  另外  ！！！
  由于hexo d上传部署到github的其实是hexo编译后的文件，是用来生成网页的，不包含源文件。
  所以当我们在其他电脑进行操作写作的时候 需要将源文件上传到分支。将分支克隆下来，除了git文件外其他文件进行删除，将之前的源文件复制进来替换，除了.deploy_git不进行复制。复制过来的源文件应该有一个.gitignore，用来忽略一些不需要的文件，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：
  ```
  .DS_Store
  Thumbs.db
  db.json
  *.log
  node_modules/
  public/
  .deploy*/
  ```

### 7. 其余补充
  * 新建文章文件夹命令：`hexo new page title(新建文章文件夹名称)`
  * 新建文章命令：`hexo new title(新建文章名称)`
  * 设置文章更新时间：在next主题中找到_config.yml在里面查找 update 会找到一个 updated_at 的属性改为 true 即可。
  ```
  post_meta:
  updated_at: true
  ```