FROM alpine:3.8

MAINTAINER https://github.com/XiaoMuYi

# 定义jdk相关信息
ENV JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=191 \
    JAVA_VERSION_BUILD=12 \
    JAVA_HOME=/usr/local/java \
    JAVA_SHA256=2787e4a523244c269598db4e85c51e0c \
    LANG=C.UTF-8 \
    GLIBC_URL=https://github.com/sgerrand/alpine-pkg-glibc \
    GLIBC_VERSION=2.28-r0 \
    PATH=${PATH}:/usr/local/java/bin

RUN set -ex && \
    apk add libstdc++ ca-certificates shadow curl bash java-cacerts unzip tzdata && \
    curl -o /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    for pkg in glibc-${GLIBC_VERSION} glibc-bin-${GLIBC_VERSION} glibc-i18n-${GLIBC_VERSION}; do curl -sSL ${GLIBC_URL}/releases/download/${GLIBC_VERSION}/${pkg}.apk -o /tmp/${pkg}.apk; done && \
    apk add --allow-untrusted /tmp/*.apk && \
    /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 C.UTF-8 || true && \
    echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh && \
    mkdir -p ${JAVA_HOME} && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    cd /tmp && \
    curl -jksSLH "Cookie:oraclelicense=accept-securebackup-cookie" \
         -O http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_SHA256}/server-jre-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
         -O http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION_MAJOR}/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
    tar -zxvf server-jre-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz -C ${JAVA_HOME} --strip-components=1 && \
    find /usr/local/java -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
    unzip jce_policy-${JAVA_VERSION_MAJOR}.zip && \
    cp -v UnlimitedJCEPolicyJDK8/*.jar ${JAVA_HOME}/jre/lib/security/ && \
    cd /usr/local/java && ln -s ./jre/bin ./bin && \
    sed -i s/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/ ${JAVA_HOME}/jre/lib/security/java.security && \
    ln -sf /etc/ssl/certs/java/cacerts ${JAVA_HOME}/jre/lib/security/cacerts && \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf && \
    apk del glibc-i18n curl ca-certificates unzip tar && \
    rm -rf /etc/apk/keys/sgerrand.rsa.pub \
           /usr/local/java/jre/plugin \
           /usr/local/java/jre/bin/javaws \
           /usr/local/java/jre/bin/jjs \
           /usr/local/java/jre/bin/orbd \
           /usr/local/java/jre/bin/pack200 \
           /usr/local/java/jre/bin/policytool \
           /usr/local/java/jre/bin/rmid \
           /usr/local/java/jre/bin/rmiregistry \
           /usr/local/java/jre/bin/servertool \
           /usr/local/java/jre/bin/tnameserv \
           /usr/local/java/jre/bin/unpack200 \
           /usr/local/java/jre/lib/javaws.jar \
           /usr/local/java/jre/lib/deploy* \
           /usr/local/java/jre/lib/desktop \
           /usr/local/java/jre/lib/*javafx* \
           /usr/local/java/jre/lib/*jfx* \
           /usr/local/java/jre/lib/amd64/libdecora_sse.so \
           /usr/local/java/jre/lib/amd64/libprism_*.so \
           /usr/local/java/jre/lib/amd64/libfxplugins.so \
           /usr/local/java/jre/lib/amd64/libglass.so \
           /usr/local/java/jre/lib/amd64/libgstreamer-lite.so \
           /usr/local/java/jre/lib/amd64/libjavafx*.so \
           /usr/local/java/jre/lib/amd64/libjfx*.so \
           /usr/local/java/jre/lib/ext/jfxrt.jar \
           /usr/local/java/jre/lib/ext/nashorn.jar \
           /usr/local/java/jre/lib/oblique-fonts \
           /usr/local/java/jre/lib/plugin.jar \
           /var/cache/apk/* \
           /tmp/*
