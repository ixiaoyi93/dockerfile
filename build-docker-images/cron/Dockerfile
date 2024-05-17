FROM centos:centos7.4.1708
LABEL XiaoMuYi

RUN yum -y install cronie \
    && sed -i -e '/pam_loginuid.so/s/^/#/' /etc/pam.d/crond \
    && cat ../data-monitor.cron >> /var/spool/cron/root \
    && chmod 600 /var/spool/cron/root \
    && touch /var/log/cron.log
    
WORKDIR /data/app
ENTRYPOINT ["sh","-c","crond && tail -f /dev/null"]
