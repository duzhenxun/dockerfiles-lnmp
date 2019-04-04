FROM mysql:5.7
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY sources.list /etc/apt/sources.list

RUN  apt-get -o Acquire::Check-Valid-Until=false update \
    && apt-get -y upgrade \
    && apt-get install -y --no-install-recommends cron vim curl \
    && apt-get clean \
    && apt-get autoremove --purge -y