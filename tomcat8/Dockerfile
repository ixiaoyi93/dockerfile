FROM alpine:3.7

MAINTAINER https://github.com/XiaoMuYi

# 定义jdk相关信息
ARG JAVA_VERSION=8
ARG JAVA_UPDATE=181
ARG JAVA_BUILD=13
ARG JAVA_HOME=/usr/local/java
ARG JAVA_DOWNLOAD_URL=http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/96a7b8442fe848ef90c96a2fad6ed6d1/server-jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz

# 指定tomcat版本
ARG TOMCAT_MAJOR=8
ARG TOMCAT_VERSION=8.0.33
ARG TOMCAT_HOME=/usr/local/tomcat

ENV LANG=C.UTF-8

RUN ALPINE_GLIBC_BASE_URL="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" && \
    ALPINE_GLIBC_PACKAGE_VERSION="2.28-r0" && \
    ALPINE_GLIBC_BASE_PACKAGE_FILENAME="glibc-$ALPINE_GLIBC_PACKAGE_VERSION.apk" && \
    ALPINE_GLIBC_BIN_PACKAGE_FILENAME="glibc-bin-$ALPINE_GLIBC_PACKAGE_VERSION.apk" && \
    ALPINE_GLIBC_I18N_PACKAGE_FILENAME="glibc-i18n-$ALPINE_GLIBC_PACKAGE_VERSION.apk" && \
    apk add --no-cache --virtual=.build-dependencies wget ca-certificates && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" && \
    apk add --no-cache \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" && \
    \
    rm "/etc/apk/keys/sgerrand.rsa.pub" && \
    /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 C.UTF-8 || true && \
    echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh && \
    \
    apk del glibc-i18n && \
    \
    rm "/root/.wget-hsts" && \
    apk del .build-dependencies && \
    rm \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" \
    && apk add --no-cache  --virtual .fetch-deps ca-certificates coreutils bash tar wget unzip tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && mkdir -p ${JAVA_HOME} ${TOMCAT_HOME} \
    && wget -P /tmp --no-cookies --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" ${JAVA_DOWNLOAD_URL} \
    && wget -P /tmp http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz \
    && cd /tmp \
    && tar -zxf apache-tomcat-${TOMCAT_VERSION}.tar.gz \
    && tar -zxf server-jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz -C ${JAVA_HOME} --strip-components=1 \
    && cp -rf  apache-tomcat-${TOMCAT_VERSION}/* ${TOMCAT_HOME}/ \
    && apk del tar wget ca-certificates unzip tar \
    && rm -rf server-jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz  apache-tomcat-${TOMCAT_VERSION}.tar.gz \
           ${JAVA_HOME}/COPYRIGHT ${JAVA_HOME}/LICENSE  ${JAVA_HOME}/README.html  ${JAVA_HOME}/THIRDPARTYLICENSEREADME.txt \
           ${TOMCAT_HOME}/LICENSE ${TOMCAT_HOME}/NOTICE ${TOMCAT_HOME}/RELEASE-NOTES ${TOMCAT_HOME}/RUNNING.txt \
           /tmp/* ${TOMCAT_HOME}/webapps/* \
           /var/cache/apk/* \
    && echo "CATALINA_OPTS=\"\$CATALINA_OPTS -\$Xms -\$Xmx -Xss512k  -XX:+AggressiveOpts \
          -XX:+UseBiasedLocking -XX:+DisableExplicitGC -XX:MaxTenuringThreshold=15 \
          -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled \ 
          -XX:ReservedCodeCacheSize=32m -XX:+UseCMSCompactAtFullCollection\ 
          -XX:LargePageSizeInBytes=128m -XX:+UseFastAccessorMethods \
          -XX:+UseCMSInitiatingOccupancyOnly -Djava.awt.headless=true\"" | \
    tee -a $TOMCAT_HOME/bin/setenv.sh \
    && chmod 755 $TOMCAT_HOME/bin/setenv.sh 

# 指定环境变量
ENV JAVA_HOME=/usr/local/java
ENV TOMCAT_HOME=/usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$JAVA_HOME/bin:$PATH

EXPOSE 8080
CMD ["/usr/local/tomcat/bin/catalina.sh","run"]
