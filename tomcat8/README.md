# Dockerfile usage

### build docker images
docker build --no-cache -t alpine-sunjdk8-tomcat8:v1 .

### running docker container
docker run -it -d --restart=always -p 8084:8084 -e Xms=Xms4096m -e Xmx=Xmx4096m -v /data/docker/container/tomcat8/logs:/usr/local/tomcat/logs --name tomcat8 apline-sunjdk8-tomcat8:v1

### unrealized function
1.Not support tomcat-native<br>
2.Tomcat thread pool not optimized

