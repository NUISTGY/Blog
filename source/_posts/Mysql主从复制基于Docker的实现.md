---
title:  Mysql主从复制————基于Docker的实现
tags: [Mysql, Docker, 数据库, 后端]
categories: [数据库]
date: 2022-9-21

---

## 主库

**主库采用远程服务器中安装的Mysql 8.0.26**
### Step1 修改配置文件

vi打开/etc/my.cnf写入👇

```
log-bin=mysql-bin   #[必须]启用二进制日志
server-id=100       #[必须]配置服务器ID，可自定
```

### Step2 重启Mysql服务

```
systemctl restart mysqld
```

### Step3 创建从库用户并授权

```
create user ‘#userName’@’#host’ identified by ‘#passWord’;
```
- **#userName** 代表你要创建的此数据库的新用户账号
- **#host** 代表访问权限，如下：
  - **%** 代表通配所有host地址权限(可远程访问)
  - **localhost** 为本地权限(不可远程访问)
  - 指定特殊Ip访问权限 如10.138.106.102

```
grant all privileges on *.* to '#userName'@'#host';
flush privileges;
```

### 查看并记录主库状态

```
show master status;
```
记录下**File**和**Position**

## 从库

从库使用了Docker

### 创建容器并挂在数据卷
vi打开/etc/my.cnf写入👇
```
docker run -id \
-p 3307:3306 \
--name=c_mysql \
-v $PWD/conf:/etc/mysql \
-v $PWD/logs:/logs \
-v $PWD/mysql-files:/var/lib/mysql-files \
-e MYSQL_ROOT_PASSWORD=**** \
mysql:8.0.26
```

### 修改从库配置文件
vi打开$PWD/conf/my.cnf文件添加👇
```
[mysqld]
## 设置serverid,同一个局域网内要唯一
server_id=101
##指定不需要同步的数据库名称
binlog-ignore-db=mysql
##开启二进制日志功能
log-bin=mall-mysql-slave1-bin
##设置二进制日志使用内存大小(事务)
binlog_cache_size=1M
##设置使用的二进制日志格式
binlog_format=mixed
##二进制日志过期清理时间
expire_logs_days=7
##跳过主从复制中所有错误或指定类型的错误，避免slave端复制中断
###1062主键重复，1032主重数据不一致
slave_skip_errors=1062
##配置中继日志
relay_log=mall-mysql-relay-bin
##表示slave将复制事件写进自己的二进制日志
log_slave_updates=1
##slave设置为只读
read_only=1
```

### 重启Docker容器

### 开启从库Slave模式
进入容器Bash后登录数据库，执行👇
```
change master to master_host='****', master_user='slave', master_password='****', master_port=3306, master_log_file='mysql-bin.000004', master_log_pos=156;
```

- master_log_file 参考主库状态的**File**
- master_log_pos 参考主库状态的**Position**

```
start slave;
```

查看是否开启
```
show slave status;
```