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
--- conf | docker原始安装方法配置文件
--- data | 数据文件夹
--- logs | 程序运行日志文件夹
--- docker-compose | docker compose创建镜像一键安装
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
