# Mysql
dockerHub地址：https://hub.docker.com/_/mysql

- 下载mysql
> docker pull mysql:latest
> 
- step1 创建网络web-net
> docker network create web-net

- step2 启动mysql容器。MYSQL_ROOT_PASSWORD：初始化root用户的密码
> docker run --name mysql01 --network web-net -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:latest

- step3 启动一个新的mysql容器，在新容器里通过mysql命令连接step2的容器。--rm：在容器退出时自动删除它
> docker run -it --network=web-net --rm mysql mysql -hmysql01 -uroot -p123456


# Postgres
dockerHub地址：https://hub.docker.com/_/postgres

- 下载docker
> docker pull postgres

- step1 创建网络web-net
> docker network create web-net

- step2 启动postgres容器。POSTGRES_PASSWORD：初始化root用户的密码
> docker run --network=web-net -p 5432:5432 --name postgres01 -e POSTGRES_PASSWORD=123456 -d postgres

- step3 启动一个新的postgres容器，在新容器里通过postgres命令连接step2的容器。--rm：在容器退出时自动删除它
> docker run -it --rm --network web-net postgres psql -h postgres01 -U postgres

# Oracle
dockerHub地址：https://hub.docker.com/r/gvenzl/oracle-xe

- 下载docker
> docker pull gvenzl/oracle-xe

- step1 启动oracle容器。ORACLE_PASSWORD：初始化SYS和SYSTEM用户的密码
> docker run -p 1521:1521 --name oracle01 -e ORACLE_PASSWORD=123456 -d gvenzl/oracle-xe

- step2 进入容器
> docker exec -it 9664c462f109 bash

- step3 链接数据库
> sqlplus SYSTEM/123456@localhost:1521/XE

# Zookeeper
dockerHub地址：https://hub.docker.com/_/zookeeper

- 下载zookeeper
> docker pull zookeeper:latest
> 
- step2 启动容器
> docker run --name some-zookeeper --restart always -d zookeeper

- step3 链接服务
> docker run -it --rm --link some-zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper
