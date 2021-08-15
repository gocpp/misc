FROM centos:7.8.2003
# 解决中文乱码
# ENV LANG C.UTF-8
RUN echo "export LC_ALL=en_US.UTF-8"  >>  /etc/profile
RUN yum -y update && \
yum -y install openssh-server && \
yum -y install curl wget git rsync telnet nc zsh zlib-devel net-tools && \
yum -y install make gcc gcc-c++ gdb&& \
yum -y install curl git zsh && \
yum clean all && \
rm -rf /tmp/* /var/tmp/*
# sshd
RUN mkdir /var/run/sshd && \
    mkdir /root/.ssh && \
    echo 'root:cynking'| chpasswd && \
    sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config &&\
    ssh-keygen -q -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N '' &&\
    ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N '' &&\
    ssh-keygen -t dsa -f /etc/ssh/ssh_host_ed25519_key -N ''
# ohmyzsh
RUN git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh &&\
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc &&\
    chsh -s /bin/zsh
# mysql connect and log4cplus
RUN mkdir /usr/local/mysql && mkdir /usr/local/include/log4cplus
ADD mysql.tar /usr/local
ADD log4cplus.tar /usr/local/include
COPY liblog4cplus.a /usr/local/lib/liblog4cplus.a
COPY lang.sh /etc/profile.d/lang.sh
# 启动sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]