# dockerfiles-lnmp 说明
Docker搭建php5.6，php7.2，nginx1.12，redis4.0，mysql5.7，memcached1.5，swoole，一键搞定！本人第一次使用docker，开发中需要使用PHP5.6与PHP7.2两个版本，如有配置不对请指正。QQ:5552123(Ado)

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
--- docker | 最原始的手动创建容器
--- docker-compose | docker compose创建镜像
--- --- mysql | mysql配置及安装文件
--- --- nginx | nginx配置及安装文件
--- --- redis | redis配置及安装文件
--- --- php56 | php5.6配置及安装文件
--- --- php72 | php7.2配置及安装文件
--- --- docker-composer.yml | docker配置执行文件
--- data | 数据目录
--- --- www | web目录
--- --- db | 数据库目录
--- --- logs | 日志目录

## 使用

#### 下载dockerfiles-lnmp
1,直接clone：
```
git clone git@github.com:duzhenxun/dockerfiles-lnmp.git
chmod -R 777 ./dockerfiles-lnmp/data/logs
cd dockerfiles-lnmp/docker-compose
```
2,进行docker-compose.yml所在文件夹执行命令即可：
```
docker-compose up  

```  
3,修改本地host 查看不同版本php-fpm
```  
127.0.0.1 php72.com
127.0.0.1 php56.com 
``` 

# 其它 dokcer 常用命令

docker info

docker pull nginx  下载镜像

docker run -p 8080:80 -d nginx 后台启动容器

docker exec  -it d6d933711bc2 /bin/bash  进入

ctrl+p  ctrl+q 退出，保持运行

docker ps 查看运行的容器

docker ps -a 查看所有容器

docker stop d6d933711bc2 停指定容器

docker rm d6d933711bc2 删指定容器

docker stop $(docker ps -q) 停止所有容器

docker rm $(docker ps -a -q) 删除所有容器

docker image 查看所有镜像

docker rmi c82521676580 删镜像

docker system df 查看占用空间

docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq) 使用显示所有容器IP地址

docker rename old_name new_name  重命名一个容器

docker tag 4cbf48630b46 duzhenxun/centos-test 打包镜像

docker run --rm -it alpine  time dd if=/dev/zero of=test.dat bs=1024 count=1000000

docker inspect web1 ///查看

自己做镜像

1,使用commit

docker commit

docker commit -p du1 // -p 暂停内部操作

docker tag imgid duzhenxun/nginx:v1.0

docker tag duzhenxun/nginx:v1.0 duzhenxun/nginx:latest

docker image rm /duzhenxun/nginx:latest

docket commit -a "5552123@qq.com" -p -c 'CMD ["/bin/nginx"]' nginx duzhenxun/nginx-web:v1.0

docker login

docker push duzhenxun/nginx-web:v1.0

2,使用 save

docker save -o dzximages.gz duzhenxun/nginx:v1.0 duzhenxun/nginx:v1.2  打包

docker load -i dzximages.gz  导入镜像

网络方面

docker network create --subnet=10.10.10.0/16  --gateway "10.10.10.1" ado

docker run -d --name es1 --net ado --ip 10.10.10.61 elasticsearch:6.4.0

ifconfig  查看网络

ip linkset dev br-xxxx name docker1

启动容器加入到新加的网络中

docker run --name du1 -it --network  ado --ip 10.10.10.101 busybox

docker run --name du2 -it --network ado --ip 10.10.10.102 busybox

cat /pro/sys/net/ipv4/ip_forward  开启net转发

日志

sudo docker logs   --tail 10 es61

远程连接docker

/etc/docker/daemon.json中

"hosts":["tcp://0.0.0.0:2375","unix:///var/run/docker.sock"]

修改docker网络IP段

  "registry-mirrors" : [
    "https://hub-mirror.c.163.com"
  ],
  "debug" : true,
  "experimental" : true,
  "bip" : "10.0.0.1/16" ,
"hosts":["tcp://0.0.0.0:2375","unix:///var/run/docker.sock"],
"insecure-registries":["0532888.cn:5000"] //http协议

快速查看容器信息

docker inspect -f {{.Monnts}} du01

docker inspect -f {{.NetworkSettings.IPAddress}} du01

docker run -it --name box1 -v /data/www:/data busybox

docker run -it --name box2 --volumes-from box1 busybox //和box1挂载一样的卷

做镜像 dockerfile

docker build -t dzxweb:v0.1 ./ dockerfile在当前目录

docker run --rm dzxweb:v0.1 cat xxx  查看文件后退出删除容器

WORKDIR /usr/local/src/

//下面ADD 会将文件复制到上面的工作目录中

ADD http://xxx.com/a.tar.gz /. 不会解

ADD a.tar.gz /. 自动解压

ENV DOC_ROOT=/data/web/html/ 变量

COPY index.html ${DOC_ROOT:-/data/web/html/}


FROM nginx

CMD["/bin/httpd","-f","-h ${WEB_DOC_ROOT}"]

ENTRYPOINT ["/bin/sh","-c"] 不可覆盖后面参数]

ENV NGX_DOC_ROOT='/data/web/html'

ADD index.html ${NGX_DOC_ROOT}

ADD entrypoint.sh /bin/

CMD ['/usr/sbin/nginx','-g','daemon off;']

ENTRYPOINT ['/bin/entrypoint.sh']


#!/bin/sh

#

cat >/etc/nginx/conf.d/www.conf << EOF
server{
sever_name ${HOSTNAME};
listen ${IP:-0.0.0.0}:${PORT:-80};
root ${NGX_DOC_ROOT:-/usr/share/nginx/html};
}
EOF
exec "$@"

docker build -t dzx-nginx:v0.1

docker run --name dzxweb --rm -e "PORT=8080" dzx-nginx:v0.1

HEALTHCHECK  健康检测

vmware/harhor 图形化仓库管理

资源分配
hub.docker.com/r/lorel/docker-stress-ng

docker pull lorel/docker-stress-ng

docker run --name stress -it --rm -m 256m --cpus 2 /lorel/docker-stress-ng:latest --cpu 8 --vm 2

docker run --name stress -it --rm -m 256m --cpu--shares  1024 /lorel/docker-stress-ng:latest --cpu 8 --vm 2

CPU是可压缩资源,内存不是
