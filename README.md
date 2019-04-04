# 项目介绍
此项目是使用Docker搭建php5.6，php7.2，nginx1.12，redis4.0，mysql5.6，elasticsearch，swoole等环境
开发中需要使用PHP5.6与PHP7.2两个版本，如有配置不对请指正。QQ:5552123(Ado)
#### 目录说明
目录 | 说明
---|---
--- conf | docker手动安装配置文件
--- --- nginx | nginx配置
--- --- redis | redis配置
--- --- mysql | redis配置
--- --- php56 | php5.6配置
--- --- php72 | php7.2配置
--- --- es | elasticsearch配置
--- data | 数据文件夹
--- logs | 程序运行日志文件夹
--- docker-compose | docker compose创建镜像一键安装
--- --- docker-composer.yml | docker配置执行文件

# 一 、手动安装容器 （推荐使用此方法）
````
一,设置docker环境相关信息
1，创建目录文件夹，如果你是windows系统请将/data/ 换成你的电脑硬盘路径即可
/data/wwwroot
/data/docker
2，下载到本地
git clone https://github.com/duzhenxun/dockerfiles-lnmp.git /data/docker

二，新建网络
如果有固定ip，我们不需要再使用link容器。这样会方便很多，我们这里以10.10.10.0段的网络举例子
docker network create --subnet=10.10.10.0/16  --gateway=10.10.10.1 ado

三，拉取镜像创建容器
1，NGINX容器
docker run -d --name nginx  \
--net ado --ip 10.10.10.11 \
-p 80:80 -p 81:81 \
-v /data/wwwroot/:/data/wwwroot/ \
-v /data/docker/logs/nginx/:/var/log/nginx/ \
-v /data/docker/conf/nginx/certs/:/etc/nginx/certs/ \
-v /data/docker/conf/nginx/conf.d/:/etc/nginx/conf.d/ \
-v /data/docker/conf/nginx/nginx.conf:/etc/nginx/nginx.conf \
-v /data/docker/conf/nginx/my_params:/etc/nginx/my_params \
nginx

2，php7.2容器
docker run -d --name php   \
--net ado --ip 10.10.10.21 -p 9000:9000 -p 8080:8080 \
-v /data/wwwroot/:/data/wwwroot/ \
-v /data/docker/logs/php72/:/var/log/php-fpm/ \
-v /data/docker/conf/php72/php.ini:/usr/local/etc/php/php.ini \
-v /data/docker/conf/php72/php-fpm.conf:/usr/local/etc/php-fpm.conf \
duzhenxun/php72

3，php5.6容器
docker run -d --name php56 \
--net ado --ip 10.10.10.22 -p 9056:9000 \
-v /data/wwwroot:/data/wwwroot \
-v /data/docker/logs/php56/:/var/log/php-fpm/ \
-v /data/docker/conf/php56/php.ini:/usr/local/etc/php/php.ini \
-v /data/docker/conf/php56/php-fpm.conf:/usr/local/etc/php-fpm.conf \
duzhenxun/php56

4，mysql:5.6容器
docker run -d --name mysql  \
--net ado --ip 10.10.10.31 -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=123456   \
-v /data/docker/data/mysql/:/var/lib/mysql/ \
-v /data/docker/logs/mysql/:/var/log/mysql/ \
-v /data/docker/conf/mysql/conf.d/:/etc/mysql/conf.d/ \
-v /data/docker/conf/mysql/my.cnf:/etc/mysql/my.cnf \
mysql:5.6

让其它容器可以连接
docker exec -it mysql bash
mysql -uroot -p123456;
grant all privileges on *.* to admin@'10.10.10.%' identified by 'adminadmin' with grant option;
flush privileges;

5，redis 容器
docker run -d  --name  redis \
--net ado --ip 10.10.10.41 -p 6379:6379 \
-v /data/docker/data/redis/:/usr/local/redis/data/ \
-v /data/docker/logs/redis/:/usr/local/redis/logs/ \
-v /data/docker/conf/redis/redis.conf:/usr/local/redis/redis.conf  \
redis redis-server /usr/local/redis/redis.conf

6，elasticsearch容器
docker run -d --name es \
--net ado --ip 10.10.10.61 -p 9200:9200 -p 9300:9300 \
-v /data/docker/conf/es/elasticsearch_1.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /data/docker/logs/es/:/usr/share/elasticsearch/logs/ \
elasticsearch:6.4.0

docker run -d --name es2 \
--net ado --ip 10.10.10.62  \
-v /data/docker/logs/es2/:/usr/share/elasticsearch/logs/ \
-v /data/docker/conf/es/elasticsearch_2.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
elasticsearch:6.4.0

平时可以用以下命令批量启动与关闭你的容器
docker start nginx php php56 mysql redis es es2
docker stop nginx php php56 mysql redis es es2

结束~

````


# 二、使用docker-compose 安装
可以一键搞定，但有时并不灵活！（本人工作中已放弃这种方案）
#### 下载dockerfiles-lnmp
```
1,下载源码 源码到data/docker下面
git clone git@github.com:duzhenxun/dockerfiles-lnmp.git /data/docker

2,进行docker-compose.yml所在文件夹执行命令即可：
cd docker-compose 
docker-compose up  
3,修改本地host 查看不同版本php-fpm
127.0.0.1 php72.com
127.0.0.1 php56.com 
```

#三、常用的命令
````
systemctl start docker 启动docker
systemctl daemon-reload
systemctl restart docker
chkconfig --list
service mysqld start/stop
/etc/init.d/mysqld start/stop
mysqladmin -p -u root shutdown
ifconfig  查看网络
cat /pro/sys/net/ipv4/ip_forward  开启net转发
apt-get update
apt-get install -y vim

docker info
docker pull nginx  下载镜像
docker run -p 8080:80 -d nginx 后台启动容器
docker exec  -it d6d933711bc2 /bin/bash  进入  ctrl+p  ctrl+q 退出，保持运行
docker ps 查看运行的容器
docker ps -a 查看所有容器
docker stop d6d933711bc2 停指定容器
docker rm d6d933711bc2 删指定容器
docker stop $(docker ps -q) 停止所有容器
docker rm $(docker ps -a -q) 删除所有容器
docker image 查看所有镜像
docker rmi c82521676580 删镜像
docker system df 查看占用空间
docker rename old_name new_name  重命名一个容器
docker tag 4cbf48630b46 duzhenxun/centos-test 打包镜像
sudo docker logs  -f --tail 10 es61  日志查看

docker run --rm -it alpine  time dd if=/dev/zero of=test.dat bs=1024 count=1000000

创建一个网络
docker network create --subnet=10.10.10.0/16  --gateway "10.10.10.1" ado
启动容器加入到新加的网络中
docker run -d --name es1 --net ado --ip 10.10.10.61 elasticsearch:6.4.0
快速查看容器信息
docker inspect -f {{.Monnts}} du01
docker inspect -f {{.NetworkSettings.IPAddress}} du01
使用显示所有容器IP地址
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
挂载卷
docker run -it --name box1 -v /data/www:/data busybox
docker run -it --name box2 --volumes-from box1 busybox //和box1挂载一样的卷

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


3,做镜像 dockerfile
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

远程连接docker
/etc/docker/daemon.json中
  "registry-mirrors" : [
    "https://hub-mirror.c.163.com"
  ],
  "debug" : true,
  "experimental" : true,
  "bip" : "192.168.50.1/16" ,
"hosts":["tcp://0.0.0.0:2375","unix:///var/run/docker.sock"],#远程连接docker
"insecure-registries":["0532888.cn:5000"] //http协议


