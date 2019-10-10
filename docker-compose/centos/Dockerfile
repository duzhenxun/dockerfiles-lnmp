FROM centos:7
MAINTAINER duzhenxun<5552123@qq.com>
# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# change sources
COPY CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo
#升级系统
RUN yum -y update
#安装openssh-server
RUN yum -y install openssh-server
#修改/etc/ssh/sshd_config
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
#将密钥文件复制到/etc/ssh/目录中
ADD ssh_host_rsa_key /etc/ssh/ssh_host_rsa_key
ADD ssh_host_rsa_key.pub /etc/ssh/ssh_host_rsa_key.pub
RUN mkdir -p /root/.ssh
ADD authorized_keys /root/.ssh/authorized_keys
#将ssh服务启动脚本复制到/usr/local/sbin目录中，并改变权限为755
ADD run.sh /usr/local/sbin/run.sh
RUN chmod 755 /usr/local/sbin/run.sh
#变更root密码为test
RUN echo "root:5552123"|chpasswd
#开放窗口的22端口
EXPOSE 22
#运行脚本，启动sshd服务
CMD ["/usr/local/sbin/run.sh"]

#创建容器
#docker run -d --name centos1 -p 10022:22 centos_sshd:v1 /usr/local/sbin/run.sh
#ssh root@127.0.0.1 -p 10022