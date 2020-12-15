# auto-server
  docker-compose 使用 nginx-proxy 在一台小鸡下部署多个项目 支持ssl。
  此项目的目的是为了自用时候方便,顺便做记录,*不保证所有参数都调优且高可用*。

## 目录
   use_ssl：支持docker-compose一键搭建并使用ssl
   normal：一些人不喜欢https或者如果套用cf再用 Let’s Encrypt 生成证书会出问题。这两种情况用这个目录下的
   tool: 一些工具类的软件,直接使用ip访问或者直接使用docker跑的东西

## 核心
   use_ssl/nginx-proxy 或者 normal/nginx-proxy 目录下的docker-compose，好奇可以自行google image后面的内容
   如果只是使用可以直接运行

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
 ####  https://blog.csdn.net/jiangyu1013/article/details/80881097
 nginx-proxy 是使用的docker的数据卷没有自定义挂载，需要操作数据卷的朋友可以参照这篇

