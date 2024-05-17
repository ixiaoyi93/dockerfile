FROM hexun/alpine-jdk8:v1

ENV CATALINA_HOME=/opt/tomcat \
    TOMCAT_MAJOR=8 \
    TOMCAT_VERSION=8.5.34 

RUN mkdir -p ${CATALINA_HOME} && \
    wget http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    tar -zxf apache-tomcat-${TOMCAT_VERSION}.tar.gz -C ${CATALINA_HOME} --strip-components=1 && \
    rm -rf apache-tomcat-${TOMCAT_VERSION}.tar.gz \
       ${TOMCAT_HOME}/webapps/*

COPY ./catalina.sh /opt/tomcat/bin/catalina.sh

EXPOSE 8080
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
