---
title: 使用Hexo + Nginx 的博客搭建
categories:
  - [web,blog]
index_img: '\posts\21986\62463049.jpg'
abbrlink: 21986
date: 2019-11-22 07:20:37
tags:
  - [blog]
  - [hexo]
  - [nginx]
banner_img:
---

>一个使用 hexo生成静态文件，并利用 nginx 转发的简单博客搭建

<!-- more -->

#### 1.安装 nodejs、git、hexo

* Git
```
$ sudo apt-get install git-core
```
* Nodejs
```
$ curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
这里使用[NodeSource](https://github.com/nodesource/distributions#deb)安装，如果你使用的服务器在国内，那么你可能需要十分耐心（或者使用代理）

* Hexo（全局安装）
```
$ sudo npm install -g hexo-cli
```
在国内可以使用[淘宝镜像](https://npm.taobao.org/)加速
```
/* 安装cnpm */
$ sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
/* 并用cnpm替换npm命令 */
$ sudo cnpm install -g hexo-cli
```

#### 2.hexo的基本配置


首先说下配置文件，hexo的配置文件有两个

> _config.yml    //这个称为 网站配置

> themes/主题名/_config.yml    // 这个称为 主题配置



* 初始化 
```
/*生成网站，并放在 hexo-blog 目录下*/
$ hexo init hexo-blog
$ cd hexo-blog
$ npm install
``` 

* 新建文章
```
$ hexo new [layout] <title>
// 新建一篇文章
$ hexo new post "hello hexo!"
/*等价于*/
$ hexo new "hello hexo!"
/*编辑新建的文章*/
$ vim source/_posts/hello-hexo.md
```

* 生成静态文件
```
$ hexo generate
或者
$ hexo g
```

* 启动服务器
```
$ hexo server
或者
$ hexo s
```
然后就可以t通过 http://你的IP:4000 访问了

#### 3.主题

默认主题 landspace 不怎么好看，换主题是很有必要的。主题可以在hexo的[主题库](https://hexo.io/themes/)中找,下载并放到 *themes/* 目录下并修改 网站配置 中的 *theme* 选项即可修改主题。具体的主题配置需要依据该主题的文档，一般都很容易找到。

#### 4.Nginx
hexo为一个全静态文件的博客，所以可以全部用nginx转发。
* nginx 安装
``` 
$ sudo apt-get install -y nginx
$ sudo systemctl enable nginx
$ sudo systemctl start nginx
```

* nginx 配置
将nginx的网站根目录指向我们生成的文件目录(*public/*) 
```
$ sudo vim /etc/nginx/sites-available/hexo-blog
// 添加如下内容
server {
        listen 80;
        listen [::]:80;
        server_name your.site.com;

        location / { 
                root /home/用户名/hexo-blog/public;
                index index.html;        
        }

        error_page  404 403 500 502 503 504  /404.html;

        location = /404.html {

        # 放错误页面的目录路径。

            root /home/用户名/hexo-blog/public;
        }
}
// 应用 nginx 配置
sudo ln -s /etc/nginx/sites-available/hexo-blog /etc/nginx/sites-enabled/hexo-blog
// 然后重启 nginx
$ sudo systemctl restart nginx
```

这样，写好文章或者做完修改后，只需要
```
hexo g 
```
生成静态文件到 *public*目录，即可。

#### 以上！




