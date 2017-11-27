# 使用Docker初始化MySQL、Redis等

> 简介：使用Docker初始化个人开发环境。
>
> 目的：不影响本机整体环境，针对每个项目模拟出一套初始化环境，避免相互影响。
>
> 注意:warning:：需提前安装docker以及docker-compose。
>
> 
>
> 此环境所包含服务：MySQL、Redis、RabbitMQ。

## 如何使用？

依次输入三条命令（亦可用&&连接）：

```bash
$ git clone https://github.com/hdig1007/init-env.git
$ cd ./init-env
$ docker-compose up -d
```

:smile:即可在本机环境启动MySQL、Redis、RabbitMQ。

## ①MySQL

> Docker-MySQL官方地址：https://hub.docker.com/_/mysql/

主要涉及参数:

- **端口号**：3306
- **root**默认**密码**：123456

这些参数通过`docker-compose.yml`配置。

BTW：若要在数据库执行之初初始化一部分数据：

可通过`Dockerfile`将**sql**文件放入容器内部的`/docker-entrypoint-initdb.d`目录。即可在初始化容器时执行初始化操作。

```dockerfile
FROM mysql:5.7.19
COPY ./xx.sql /docker-entrypoint-initdb.d
```

## ②Redis

> Docker-Redis官方地址：https://hub.docker.com/_/redis/

主要涉及参数：

- **端口号**：6379
- 初始化**密码**：123456

其中初始化密码为`Redis.conf`配置文件配置。通过Dockerfile手动设置启动文件。

```dockerfile
FROM redis:2.8
COPY redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf"]
```

## ③RabbitMQ

> Docker-RabbitMQ官方地址：https://hub.docker.com/_/rabbitmq/

主要涉及参数：

- **端口号**：5672;15672
- 默认**用户名**：root
- 默认**用户名密码**：123456

此三个参数均可以通过`docker-compose.yml`配置。其中用户名及用户名密码以环境变量的形式传递配置。

```yaml
rabbitmq:
  container_name: rabbitmq
  image: rabbitmq:3-management
  restart: always
  ports:
    - "5672:5672"
    - "15672:15672"
  environment:
    RABBITMQ_DEFAULT_USER: root
    RABBITMQ_DEFAULT_PASS: 123456
```

