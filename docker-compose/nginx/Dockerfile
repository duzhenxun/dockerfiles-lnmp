FROM nginx
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
#ADD http://nginx.org/download/nginx-1.15.2.tar.gz /usr/local/src/
#docker build -t n:v0.1 ./
#docker run --name n1 --rm  n:v0.1 ls /usr/local/src