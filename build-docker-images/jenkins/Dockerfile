FROM jenkins/jnlp-slave:alpine

MAINTAINER YangSheng <yang972711021@gmail.com>

USER root

##################### 安装glibc,执行docker #####################
ARG GLIBC_VERSION='2.27-r0'
RUN apk update \
  && apk add --no-cache shadow sudo curl tar bash ca-certificates wget libltdl \
  && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub \
  && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
  && apk add glibc-${GLIBC_VERSION}.apk \
  && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-bin-${GLIBC_VERSION}.apk \
  && apk add glibc-bin-${GLIBC_VERSION}.apk \
  && rm -rf glibc-${GLIBC_VERSION}.apk glibc-bin-${GLIBC_VERSION}.apk


#################### 安装 maven 版本为 3.5.4 ####################
ARG MAVEN_VERSION=3.5.4
ARG USER_HOME_DIR="jenkins"
ARG SHA=ce50b1c91364cb77efe3776f756a6d92b76d9038b0a0782f7d53acf1e997a14d
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && echo "${SHA}  /tmp/apache-maven.tar.gz" | sha256sum -c - \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && rm -rf /var/lib/apt/lists/* \
  && rm /var/cache/apk/* \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn \
  && echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "/home/$USER_HOME_DIR/.m2"

###################### 切换为Jenkins用户 ########################

USER jenkins
WORKDIR /home/jenkins
ENTRYPOINT ["jenkins-slave"]
