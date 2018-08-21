# dockerfiles-lnmp 说明
Docker搭建php5.6，php7.2，nginx1.12，redis4.0，mysql5.7，memcached1.5，swoole，一键搞定！本人第一次使用docker，开发中需要使用PHP5.6与PHP7.2两个版本，如有配置不对请指正。QQ:5552123

相关软件版本：
- PHP 7.2
- PHP 5.6
- MySQL 5.7 (root账号:root;密码123456)
- Nginx 1.12
- memcher
- 用到的PHP扩展
-- bcmath
Core
ctype
curl
date
dom
fileinfo
filter
ftp
gd
gettext
gmagick
hash
iconv
json
ldap
libxml
mbstring
mcrypt
memcached
mongodb
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
posix
readline
redis
swoole
ssh2
yaf
yaconf
zip
zookeeper
（注：gmagick 和 imagick 扩展冲突，默认只启用了gmagick扩展）
#### 目录说明
目录 | 说明
---|---
--- data | 数据目录
--- --- www | web目录
--- --- db | 数据库目录
--- --- logs | 日志目录
--- dockerfiles | docker镜像配置
--- --- mysql | mysql配置及安装文件
--- --- nginx | nginx配置及安装文件
--- --- redis | redis配置及安装文件
--- --- php56 | php5.6配置及安装文件
--- --- php72 | php7.2配置及安装文件
--- --- docker-composer.yml | docker配置执行文件

## 使用

#### 下载dockerfiles-lnmp
1,直接clone：
```
git clone git@github.com:duzhenxun/dockerfiles-lnmp.git
chmod -R 777 ./dockerfiles-lnmp/data/logs
cd dockerfiles-lnmp/dockerfiles
```
2,进行docker-compose.yml所在文件夹执行命令即可：
```
docker-compose up  

```  
3,修改本地host 查看不到版本php
```  
127.0.0.1 php72.com
127.0.0.1 php56.com 
``` 

# 其它 dokcer 常用命令

所有容器将后台运行：  
```
docker-compose up -d
```  
可以这样关闭容器并删除服务：
```
docker-compose down
```
下载镜像
```
docker pull nginx  
```
后台启动容器
```
docker run -p 8080:80 -d nginx 
```
进入进入容器
```
docker exec  -it d6d933711bc2 /bin/bash  
```
进入容器
```
docker-compose exec [images] bash # 例如 docker-compose exec php-fpm bash
```
退出，保持运行
```
ctrl+p  ctrl+q 
```
查看运行的容器
```
docker ps 
```
查看所有容器
```
docker ps -a 
```

停指定容器
```
docker stop d6d933711bc2 
```
删指定容器
```
docker rm d6d933711bc2 
```
停止所有容器
```
docker stop $(docker ps -q) 
```
删除所有容器
```
docker rm $(docker ps -a -q) 
```

查看所有镜像
```
docker image 
```
删镜像
```
docker rmi c82521676580 
```
查看占用空间
```
docker system df 
```

使用显示所有容器IP地址
```
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
```
重命名一个容器 
```
docker rename old_name new_name 
```
进入容器
```
docker-compose exec [images] bash # 例如 docker-compose exec php-fpm bash
```
创建并启动一个容器，在run后面加上-d参数，则会创建一个守护式容器在后台运行
```
docker run 
```
查看已经创建的容器

```
docker ps -a 
```
查看已经启动的容器

```
docker ps -s 
```
启动容器名为con_name的容器
```
docker start con_name 
```
停止容器名为con_name的容器
```
docker stop con_name 
```
删除容器名为con_name的容器
```
docker rm con_name 
```
重命名一个容器 
```
docker rename old_name new_name 
```
将终端附着到正在运行的容器名为con_name的容器的终端上面去，前提是创建该容器时指定了相应的sh
```
docker attach con_name 
```

