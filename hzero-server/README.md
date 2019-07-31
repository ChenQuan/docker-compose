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

| 域名            | 容器/服务 |
| --------------- | --------- |
| db.hzero.org    | mysql     |
| redis.hzero.org | redis     |
| dev.hzero.org   | centos    |

对外开放

http://hzerodoc.saas.hand-china.com/zh/docs/installation-configuration/service-install/local-install/

## 安装

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
 ping dev.hzero.org # centos服务器
```

## 部署

1. 运行容器

```shell
docker-compose -f docker-compose.yaml up -d --force-recreate
```

2. 传输源码到容器（centos）

方法一：从本机复制文件到容器中

```shell
docker cp /home/zero/docker-compose/hzeroserver-register hzero-env:/home/hzeroserver-register # hzero-env：为容器名称
```

> /home/zero/docker-compose/hzeroserver-register: 本地路径,即原始路径
>
> /home/hzeroserver-register:容器（centos)路径,即目标路径

方法二：从Git仓库获取源码

以SSH或Https方式拉取项目源码：

```shell
git clone git@xxxx/demo-register.git 
git clone https://xxxx/demo-register.git
```

3. 下载 [run.sh](http://hzerodoc.saas.hand-china.com/files/docs/installation-configuration/install/run.sh) 脚本放到 hzero-register 根目录下

4. 修改 run.sh，将`JAR`设置为服务名，`MPORT`设置为配置文件中 `management.port` 的端口。该脚本会拉取最新代码、打包、停止原服务、启动新服务。

   ![img](/img/run-shell.jpg)

   设置 run.sh 可执行：`# chmod +x run.sh`

5. 运行脚本`./run.sh`,完成手动部署