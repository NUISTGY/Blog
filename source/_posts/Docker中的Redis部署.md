---
title:  Docker中的Redis部署
tags: [Redis, Docker]
categories: [Docker]
date: 2022-9-18

---

## 创建Redis的Docker容器

<center>👇直奔主题👇</center>

### shell脚本

```shell
docker run \
--name myredis \
-v /root/redis_docker/mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-v /root/redis_docker/mydata/redis/data:/data \
-p 6379:6379 \
-d --restart=always redis:5.0 redis-server /etc/redis/redis.conf \
--appendonly yes  \
--requirepass ******
```

### 说明
- --name redis 【容器名】
- -p 6379:6379 【映射端口】
- -v /usr/local/app/redis/redis.conf:/etc/redis/redis.conf 【conf文件挂载目录】
- -v /usr/local/app/redis/data:/data 【data挂载目录】
- -d redis:5.0 【后台运行镜像】
- --restart=always 【docker重启后自动启动镜像】
- redis-server /etc/redis/redis.conf 【在容器执行redis-server启动命令，执行conf文件】
- --appendonly yes 【持久化】
- --requirepass "root" 【设置密码】

### 补充

- docker exec -it redis bash 【进入容器】
- redis-cli 【连接】
- auth root 【登录】
- set hello world
- get hello