# auto-server
  docker-compose 使用 nginx-proxy 在一台小鸡下部署多个项目 使用域名区分 支持ssl。
  此项目的目的是为了自用时候方便,顺便做记录,*不保证所有参数都调优且高可用*。

 补充：

  本项目的初衷是为了给自己折腾留下记录，所以此项目之后方向可能会转变不仅仅是使用docker搭建

  项目如此简单:因此会逐渐增加各种对数据的管理、使用golang实现一键脚本(面板)等功能

## 目录
   use_ssl：支持docker-compose一键搭建并使用ssl
   normal：一些人不喜欢https或者如果套用cf再用 Let’s Encrypt 生成证书会出问题。这两种情况用这个目录下的
   tool: 一些工具类的软件,直接使用ip访问或者直接使用docker跑的东西

   data: 所有目录的数据集中到这里管理，方便数据备份/转移

## 核心
jwilder/nginx-proxy 负责通过你填写的域名自动生成nginx的配置文件，不需要关心细节，只需要运行起来就可以
jrcs/letsencrypt-nginx-proxy-companion 负责通过letsencrypt生成免费https证书并且自动重新认证
不套用cf的情况下理论上是可以一次搭建永久有效。套用cf也可以,不过过了60天后会不会自动重新认证https我还没有测试

## 使用步骤
   #### 1.下载docker 和 docker-compose 并启动
具体参照: [docker手册](https:://yeasy.gitbook.io/docker_practice/install "Markdown")。
   #### 2.创建一个 Docker network
    docker network create nginx-proxy
   #### 3.到对应目录启动 nginx-proxy
    cd auto-server\use_ssl\nginx-proxy
    or
    cd auto-server\normal\nginx
    docker-compose up -d
   #### 4.修改并启动需要启动的服务的docker-compose中的参数
    如bitwarden下的
      - VIRTUAL_HOST=example.test.com
      - LETSENCRYPT_HOST=example.test.com
      - DEFAULT_EMAIL=xxx@gmail.com
    将对应的域名和邮箱修改,不使用ssl的情况下 只需要修改VIRTUAL_HOST
    一些特殊的软件可能需要额外配置，请留意文件中的注释

## 已知的问题
在套用cloudflare并且https启用严格的情况下,再使用use_ssl中的方法运行可能出现ssl检验不过的情况,实测可以先使用灵活模式，再运行docker-compose。
再改成严格模式/直接使用normal方式启动cf设置成灵活模式/cloudflare使用严格模式并且dns中配置足够久的情况下仍然可以直接使用use_ssl启动，这也是为什么一开始我没有发现这个问题的原因。

## 问题排查和一些基本操作
当发现启动项目后运行不正常，先使用docker ps -a
查询到对应docker容器的id 再使用 docker logs -f 容器id的方式可以查询到该容器的启动日志。多数情况下是jrcs/letsencrypt-nginx-proxy-companion没有签发成功，详细情况可以添加issue。或者直接使用normal方式
nginx-proxy中的数据是通过 docker volume管理的,某些情况下你可能会想将他删除重新生成 先使用docker-compose down  再使用 docker volume prune 清理

## 已经支持的项目
	bitwarden
	chevereto
	gitlab
	file_server(https://files.photo.gallery/demo)
	wordpress
	shadowsoks
	resilio
	syncthing
	transmission

## 参照
 #### http://einverne.github.io/post/2017/02/docker-nginx-host-multiple-websites.html
 看见这篇文章开始折腾的
 ####  https://yeasy.gitbook.io/docker_practice/data_management/volume
 nginx-proxy 是使用的docker的数据卷没有自定义挂载，需要操作数据卷的朋友可以参照这篇

## 或许你有兴趣看我的博客
 #### https://blog.hideo.site
