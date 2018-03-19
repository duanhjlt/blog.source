---
title: Hexo搭建个人博客
---

使用Hexo搭建个人博客的说明，可以在网上搜到一堆一堆的，这里只是记录个人搭建的过程，毕竟按照网上的说明，不是一气呵成就搭建好的，也算是踩了一些坑了。  
## 1.Hexo介绍
Hexo是一个简单、轻量、基于Node的一个静态博客框架。通过Hexo可以快速创建自己的博客，只需要几条命令就可以完成，非常的方便。  

发布时，Hexo可以部署到自己的Node服务器上（我没有😄😄），也可以部署到github上面（必须滴）。

Hexo的官方网站：http://hexo.io
## 2.安装nvm
使用的OS X, 各种基础配置都是齐全的，就不多说了。

安装**nvm**（Node Version Manager）,在Terminal中运行  

```bash
$ curl https://raw.githubusercontent.com/creationix/nvm/v0.7.0/install.sh | sh
```

安装成功提示如下：  
`=> Close and reopen your terminal to start using NVM`  
重启Terminal后nvm命令才会生效。  
> 注意：  
> 1. 如果update失败的话，请检查~/.bash_profile是否存在，如果没有的话，新建一个文件，重新执行上面的命令  
> 2. 如果是zsh用户，写入到.bash_profile中是不生效的，要写入到.zshrc文件中,内容如下： 
>
``` zshrc
[ -s "/Users/duanhongjin/.nvm/nvm.sh" ] && . "/Users/duanhongjin/.nvm/nvm.sh" # This loads nvm
```

## 3.安装Node.js
使用nvm安装node.js  

```bash
$ nvm install 0.10
```

## 2.Hexo安装
Hexo安装要用全局安装，加-g参数  

```bash
$ npm install -g hexo
$ hexo init ~/Documents/blog
$ cd ~/Documents/blog
```
安装之后看一下版本号  

```bash
$ hexo version
=> hexo： 3.2.2
```
如果是Hexo2.6以后的版本还需要执行：

```bash
$ npm install hexo-renderer-ejs --save
$ npm install hexo-renderer-stylus --save
$ npm install hexo-renderer-marked --save
```
### 主题
我使用的jacman，准备修改一下，所以fork了一份 

```bash 
$ git clone https://github.com/duanhjlt/jacman.git themes/jacman
```
在*_config.yml*配置文件中配置：  

```
theme: jacman
```
### 新建文章

```bash
$ hexo new Hexo搭建个人博客
```
会在**source/_posts/**下生成文件**Hexo搭建个人博客.md**， 编辑此文件后生成静态html 

```bash  
$ hexo generate
```
> 在进行deploy前先生成html

### deploy
在github上建一个respository,名字随意，不用相信必须是**name.github.io**的话，在*__config.yml*中：

```config
deploy:
  type: git
  repo: https://github.com/duanhjlt/blog.git
```
> 注意： type要用git，别用github，否则会报错

然后执行:

```bash
$ npm install hexo-deployer-git --save
$ Hexo deploy
```
### 本地预览

```bash
$ hexo server  
=> [info] Hexo is running at localhost:4000/. Press Ctrl+C to stop.
```
## 4.绑定域名
我是在万网申请的域名**starship.pub**（这个比较便宜），可以在**阿里云**上进行域名解析，也可以在**dnspod**上进行解析，我使用的是万网。
### 主域名绑定
首先ping一下记住ip  

```bash
$ ping https://duanhjlt.github.io
=> 64 bytes from 151.101.36.133: icmp_seq=0 ttl=44 time=426.403 ms
```
> ip可能会有变化，以ping为准

在github上托管，首先建一个CNAME文件，里面写**最终指向**的域名

```bash
$ echo starship.pub > public/CNAME
```
![主域名](/images/20160730110501.jpeg)

### 子域名绑定
首先要记录github的ip,同样要写CNAME文件  

```bash
$ ping github.com
```
![子域名](/images/20160730110601.jpeg)

经过漫长的等待，终于能看到了！！！  
等有时间了在修改页面吧。。。。

PS:  
将blog本地迁移到另一台电脑后，重新发布就无法打开了。折腾了好几天，发现ip和原来也不一样了。各种google，各种baidu后，找到另一个方式

1. CNAME文件修改为**blog.starship.pub**
![CNAME](/images/20170408171301.jpeg)
2. 域名解析添加 **CNAME** , 主机名为： **blog**, 记录值这： **duanhjlt.github.io.**（注意后面的 **.** ）
![子域名](/images/20170408171302.jpeg)

祝君好运