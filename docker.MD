# Mysql
dockerHub地址：https://hub.docker.com/_/mysql

- 下载mysql
> docker pull mysql:latest
> 
- step1 创建网络web-net
> docker network create web-net

- step2 启动mysql容器。MYSQL_ROOT_PASSWORD：初始化root用户的密码
> docker run --name my-mysql --network web-net -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest

- step3 启动一个新的mysql容器，在新容器里通过mysql命令连接step2的容器。--rm：在容器退出时自动删除它
> docker run -it --network=web-net --rm mysql mysql -hmysql01 -uroot -p123456


# Postgres
dockerHub地址：https://hub.docker.com/_/postgres

- 下载docker
> docker pull postgres

- step1 创建网络web-net
> docker network create web-net

- step2 启动postgres容器。POSTGRES_PASSWORD：初始化root用户的密码
> docker run --name my-postgres --network=web-net -p 5432:5432 -e POSTGRES_PASSWORD=123456 -d postgres

- step3 启动一个新的postgres容器，在新容器里通过postgres命令连接step2的容器。--rm：在容器退出时自动删除它
> docker run -it --rm --network web-net postgres psql -h postgres01 -U postgres

# Oracle
dockerHub地址：https://hub.docker.com/r/gvenzl/oracle-xe

- 下载docker
> docker pull gvenzl/oracle-xe

- step1 启动oracle容器。ORACLE_PASSWORD：初始化SYS和SYSTEM用户的密码
> docker run --name my-oracle --network=web-net -p 1521:1521 -e ORACLE_PASSWORD=123456 -d gvenzl/oracle-xe

- step2 进入容器
> docker exec -it 9664c462f109 bash

- step3 链接数据库
> sqlplus SYSTEM/123456@localhost:1521/XE

# Zookeeper
dockerHub地址：https://hub.docker.com/_/zookeeper

- 下载zookeeper
> docker pull zookeeper:latest
> 
- step1 启动容器
> docker run --name my-zookeeper --network=web-net -p 2181:2181 -e ALLOW_ANONYMOUS_LOGIN=yes --restart always -d zookeeper

- step2 链接服务
> docker run -it --rm --link my-zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper

# Reids
dockerHub地址：https://hub.docker.com/_/redis

- 下载redis
> docker pull redis:latest
> 
- step1 启动容器
> docker run --name some-redis --network=web-net -p 6349:6349 -d redis

- step2 链接服务
> docker exec -it <容器id> redis-cli

# Kafka
dockerHub地址：https://hub.docker.com/r/bitnami/kafka

- 下载kafka
> docker pull bitnami/kafka:latest

- step1 启动容器
> docker run --name my-kafka --network=web-net -p 9092:9092 -e ALLOW_PLAINTEXT_LISTENER=yes -e KAFKA_CFG_ZOOKEEPER_CONNECT=my-zookeeper:2181
-d bitnami/kafka:latest

- step2 链接服务
> docker run -it --rm --network web-net -e KAFKA_CFG_ZOOKEEPER_CONNECT=my-zookeeper:2181 bitnami/kafka:latest kafka-topics.sh --list  --bootstrap-server my-kafka:9092


# xxl-job
dockerHub地址：https://hub.docker.com/r/xuxueli/xxl-job-admin

- 下载image
> docker docker run xuxueli/xxl-job-admin:2.4.0

- step1 启动容器，注意mysql的host和密码要修改成对应的值
> docker run --name my-xxl-job --network=web-net -p 8080:8080 -e PARAMS="--spring.datasource.url=jdbc:mysql://my-mysql:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai --spring.datasource.password=123456" -d xuxueli/xxl-job-admin:2.4.0

- step2 访问后台
- http://localhost:8080/xxl-job-admin/jobinfo
- 默认账号：admin/123456

