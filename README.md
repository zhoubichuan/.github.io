## Hexo 使用指南

npm install hexo -g #安装

npm update hexo -g #更新

hexo init #初始化

## 草稿

hexo publish [layout] <title> #发表草稿。

## 写作 hexo

hexo n "name" #新建文章

hexo g #生成静态网页

hexo p #发表草稿。

hexo s #启动服务

hexo d #部署网站 参数：-g 部署之前先生成静态文件。

## 服务器

Hexo 3.0 把服务器独立成了个别模块，您必须先安装才能使用。

npm install hexo-server --save #安装服务

hexo s #启动服务

hexo server -p 5000 #更改端口

hexo s -s #静态模式

hexo s -i 192.168.1.1 #自定义 ip

hexo clean #清除缓存

## 部署

hexo d -g

hexo g -d

两者作用完全相同。
