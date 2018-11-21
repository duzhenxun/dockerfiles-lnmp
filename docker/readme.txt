如果想手动运行。
1，先建文夹/data/docker-cnf 将docker-cnf 文件复制
2，安装镜像
docker pull mysql:5.6
docker pull redis
docker pull nginx
docker pull duzhenxun/php72
docker pull duzhenxun/php56
3,创建容器
docker run --name mysql  -p 3306:3306 -e MYSQL_ROOT_PASSWORD=qq5552123 -d \
-v /data/docker-data/mysql/:/var/lib/mysql/ \
-v /data/docker-logs/mysql/:/var/log/mysql/ \
-v /data/docker-conf/mysql/conf.d/:/etc/mysql/conf.d/ \
-v /data/docker-conf/mysql/my.cnf:/etc/mysql/my.cnf \
mysql:5.6

docker run --name redis -d -p 6379:6379 \
-v /data/docker-data/redis/:/usr/local/redis/data/ \
-v /data/docker-log/redis/:/usr/local/redis/logs/ \
-v /data/docker-conf/redis/redis.conf:/usr/local/redis/redis.conf \
redis redis-server /usr/local/redis/redis.conf

docker run --name php72  -d --link mysql --link redis \
-v /data/wwwroot:/data/wwwroot \
-v /data/docker-logs/php72/:/var/log/php-fpm \
-v /data/docker-conf/php72/php.ini:/usr/local/etc/php/php.ini \
-v /data/docker-conf/php72/php-fpm.conf:/usr/local/etc/php-fpm.conf \
duzhenxun/php72

docker run --name php56  -d --link mysql --link redis \
-v /data/wwwroot:/data/wwwroot \
-v /data/docker-logs/php56/:/var/log/php-fpm \
-v /data/docker-conf/php56/php.ini:/usr/local/etc/php/php.ini \
-v /data/docker-conf/php56/php-fpm.conf:/usr/local/etc/php-fpm.conf \
duzhenxun/php56

docker run --name nginx   --link php72 --link php56 -d -p 80:80 \
-v /data/wwwroot/:/data/wwwroot/ \
-v /data/docker-logs/nginx/:/var/log/nginx/ \
-v /data/docker-conf/nginx/conf.d/:/etc/nginx/conf.d/ \
-v /data/docker-conf/nginx/nginx.conf:/etc/nginx/nginx.conf \
-v /data/docker-conf/nginx/my_params:/etc/nginx/my_params \
nginx

设置数据可连接范围
grant all privileges on *.* to root@'172.17.0.%' identified by 'qq5552123' with grant option;
flush privileges;


