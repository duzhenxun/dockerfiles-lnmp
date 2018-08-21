FROM redis:3.2
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]