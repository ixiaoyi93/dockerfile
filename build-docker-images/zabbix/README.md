## docker 安装 zabbix服务端 
这个compose文件运行了基于Alpine Linux上的Zabbix 3.2最新版本的组件，支持MySQL数据库
```
.env_src          ## zabbix server 端指定参数；
.env_web          ## zabbix web 配置文件，主要还是以官方参数为主，或者直接修改配置文件；
.env_java         ## zabbix javagateway 相关参数配置；
.env_agent        ## zabbix agent 配置文件；
.env_db_mysql     ## zabbix 数据库相关参数说明。主要是数据库库名、用户名、密码等；
```
上面信息配置无误之后，直接执行如下命令即可
```
docker-compose -f /opt/docker-compose.yml up -d
```
参考文件：
https://github.com/zabbix/zabbix-docker
