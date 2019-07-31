# Hzero-Server
**安装清单**

| 组件           | 描述                | 版本      |
| :------------- | :------------------ | :-------- |
| JDK            | Java运行环境        | 1.8.0_172 |
| Nginx          | 前端代理            | 1.8.1     |
| Node           | JavaScript 运行环境 | 10.15.0   |
| Git            | 源码管理            | 2.22.0 |
| Maven          | 项目构建            | 3.3.9     |
| Redis | 缓存数据库   | 4.0.2                        |
| Mysql | 数据库       | 5.7.17                       |

| 域名            | 容器/服务 |      |
| --------------- | --------- | ---- |
| db.hzero.org    | mysql     |      |
| redis.hzero.org | redis     |      |
| dev.hzero.org   | centos    |      |

http://hzerodoc.saas.hand-china.com/zh/docs/installation-configuration/service-install/local-install/

## 使用

### 一、安装Docker和Docker-compose

Docker：https://yeasy.gitbooks.io/docker_practice/install/

Docker-compose：https://yeasy.gitbooks.io/docker_practice/compose/install.html

### 二、运行容器

拉取仓库：

```bash
https://github.com/ChenQuan/docker-compose.git
```

切换到`hzero-server`目录

```shell
cd hzero-server
```

运行

```shell
docker-compose -f docker-compose.yaml up -d --force-recreate
```

> - -d: 后台运行
> - --force-recreate 强制重新运行

查看容器运行情况

```shell
docker ps
```

```shell
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE                                                            COMMAND                  CREATED             STATUS              PORTS                                                                      NAMES
4fe5e36dde36        redis                                                            "docker-entrypoint.s…"   28 minutes ago      Up 28 minutes       0.0.0.0:6379->6379/tcp                                                     hzero-redis
a268056a9199        chenquan97/hzero-server                                          "/usr/sbin/init"         28 minutes ago      Up 27 minutes       3306/tcp, 0.0.0.0:80->80/tcp, 0.0.0.0:8000-9000->8000-9000/tcp, 6397/tcp   hzero-env
2f6e840271c8        registry.cn-hangzhou.aliyuncs.com/choerodon-tools/mysql:5.7.17   "docker-entrypoint.s…"   28 minutes ago      Up 28 minutes       0.0.0.0:3306->3306/tcp                                                     hzero-mysql

```

进入Centos服务器系统

```shell
docker exec  -it  hzero-env /bin/bash
```

测试容器网络连通性

```shell
 ping db.hzero.org # mysql数据库
 ping redis.hzero.org # redis数据库
```

