## Dockerfile说明
此Jenkins slave镜像主要是配合 Jenkins on kubernetes进行实现，主要完成从salve内部实现构建镜像功能。
## 功能说明
支持maven构建
支持docker镜像构建
## 使用方式，如下：
```
docker build -t jenkins-slave-mvn:v1
```
