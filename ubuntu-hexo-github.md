title: ubuntu14.04搭建hexo+github博客环境
date: 2015-07-23 16:59:13
categories: blog
tags: [hexo]
---
## **安装**
[Hexo](https://hexo.io/) 是一个快速、简洁、高效的博客框架。Hexo使用Markdown（或者其他渲染引擎）解析文章，在几秒内即可使用靓丽的主体生成静态网页。
在安装hexo之前，需要安装如下两个应用程序：
 * [Node.js](https://nodejs.org/)
 * [Git](http://git-scm.com/)

### **安装Git**
直接使用命令：sudo apt-get install git-core
### **安装Node.js**
推荐使用nvm安装，下载方式如下：
```bash
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```
安装成功后，**重启终端**后运行下面命令安装Node.js
 ```bash
nvm install 0.12（可能版本会更新，我安装时的版本是0.12）
 ```

### **安装hexo**
```bash
npm install -g hexo-cli
```
## **github设置**
1. 申请github账号，这个就不介绍了。
2. 建立一个仓库，仓库名字为<username>.github.io,比如我的为zhangg.github.io。
3. 配置SSH keys。具体的参考github官网给出的[步骤](https://help.github.com/articles/generating-ssh-keys/)。

## **配置hexo**
创建一个自己的hexo文件夹并初始化：
```bash
hexo init myhexo
cd myhexo
apt-get install npm
npm install
```
随后会在myhexo下生成一些文件，
```bash 
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
其中__config.yml 是配置文件，打开该文件在最下方的*deploy*部分修改如下内容：
```bash
type：git
repository: git@github.com:xxx/xxx.github.io.git (即上面建的repo的地址）
branch: master
```
最后，在自己的hexo文件下运行：
```bash
hexo g
hexo d
```
hexo d时有可能遇到错误，遇到错误是运行如下命令：
```bash
npm install hexo-deployer-git --save
```

完成了这些，就可以在 https://xxx.github.io 查看自己的blog网站效果了。
