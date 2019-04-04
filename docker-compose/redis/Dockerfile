FROM redis:4.0
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
COPY redis.conf /usr/local/redis/etc/redis.conf
CMD [ "redis-server","/usr/local/redis/etc/redis.conf"]