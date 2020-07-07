# docker-alinode

Node.js 性能平台 - Node.js Performance Platform  

> 集成 alinode + yarn + pm2 + agenthub  
> 自动启动 agenthub 服务，让你可以像 pm2 镜像一样方便使用。  
> 增加时间为上海时间

[GitHub](https://github.com/toomeefed/docker-alinode)
|
[Docker Store](https://store.docker.com/r/toomee/alinode)
|
[《Node.js性能平台运行时版本和官方对应列表》](https://help.aliyun.com/knowledge_detail/60811.html)
|
[《官网 Dockerfile 模板》](https://github.com/toomeefed/docker-alinode/tree/master/official)

## 标签对应关系

镜像 | 基础镜像 | AliNode | Node | Dockerfile
:-- | :-- | :-- | :-- | :--
toomee/alinode:3 | debian:jessie | v3.15.0 | v8.16.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/3/jessie/Dockerfile)
toomee/alinode:3-slim | debian:jessie-slim | v3.15.0 | v8.16.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/3/slim/Dockerfile)
toomee/alinode:3-alpine | alpine:3.8 | v3.14.1 | v8.15.1 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/3/alpine/Dockerfile)
toomee/alinode:4 | debian:jessie | v4.8.0 | v10.16.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/4/jessie/Dockerfile)
toomee/alinode:4-slim | debian:jessie-slim | v4.8.0 | v10.16.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/4/slim/Dockerfile)
toomee/alinode:4-alpine | alpine:3.8 | v4.7.0 | v10.15.3 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/4/alpine/Dockerfile)
toomee/alinode:5 | debian:jessie | v5.13.0 | v12.13.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/5/jessie/Dockerfile)
toomee/alinode:5-slim | debian:jessie-slim | v5.13.0 | v12.13.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/5/slim/Dockerfile)
toomee/alinode:5-alpine | alpine:3.9 | v5.13.0 | v12.13.0 | [Dockerfile](https://github.com/toomeefed/docker-alinode/blob/master/5/alpine/Dockerfile)


### 所有镜像

```sh
$ docker images toomee/alinode
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
toomee/alinode      5-alpine            0cebf1ac010e        1 minutes ago       115MB
toomee/alinode      5.13-alpine         0cebf1ac010e        1 minutes ago       115MB
toomee/alinode      5.13.0-alpine       0cebf1ac010e        1 minutes ago       115MB
toomee/alinode      5-slim              4e8ca2cd1b31        2 minutes ago       228MB
toomee/alinode      5.13-slim           4e8ca2cd1b31        2 minutes ago       228MB
toomee/alinode      5.13.0-slim         4e8ca2cd1b31        2 minutes ago       228MB
toomee/alinode      5                   20724a6d2b7e        3 minutes ago       276MB
toomee/alinode      5.13                20724a6d2b7e        3 minutes ago       276MB
toomee/alinode      5.13.0              20724a6d2b7e        3 minutes ago       276MB
toomee/alinode      4-alpine            b60e826e8854        7 minutes ago       106MB
toomee/alinode      4.7-alpine          b60e826e8854        7 minutes ago       106MB
toomee/alinode      4.7.0-alpine        b60e826e8854        7 minutes ago       106MB
toomee/alinode      4-slim              403c69f5e02b        8 minutes ago       225MB
toomee/alinode      4.8-slim            403c69f5e02b        8 minutes ago       225MB
toomee/alinode      4.8.0-slim          403c69f5e02b        8 minutes ago       225MB
toomee/alinode      4                   dd8497da944f        9 minutes ago       272MB
toomee/alinode      4.8                 dd8497da944f        9 minutes ago       272MB
toomee/alinode      4.8.0               dd8497da944f        9 minutes ago       272MB
toomee/alinode      3-alpine            8f1d88e5fdcf        10 minutes ago      101MB
toomee/alinode      3.14-alpine         8f1d88e5fdcf        10 minutes ago      101MB
toomee/alinode      3.14.1-alpine       8f1d88e5fdcf        10 minutes ago      101MB
toomee/alinode      3-slim              cdcc791e5b8b        11 minutes ago      216MB
toomee/alinode      3.15-slim           cdcc791e5b8b        11 minutes ago      216MB
toomee/alinode      3.15.0-slim         cdcc791e5b8b        11 minutes ago      216MB
toomee/alinode      3                   3a04e8b4b73f        12 minutes ago      264MB
toomee/alinode      3.15                3a04e8b4b73f        12 minutes ago      264MB
toomee/alinode      3.15.0              3a04e8b4b73f        12 minutes ago      264MB
```

## 使用说明

假设你的项目是如下结构

```
.
├── src
│   └── app.js
├── package.json
├── ecosystem.config.js
└── README.md
```

### 拉取镜像

```sh
$ docker pull toomee/alinode:5
```

### 1. 直接启动

到当前项目目录下执行如下命令：

```sh
$ docker run -d \
  -p 8000:8000 \
  -v $PWD:/app \
  -e "APP_ID=应用ID" \
  -e "APP_SECRET=应用密钥" \
  -h my-alinode \
  --name my-alinode \
  toomee/alinode:5
```

### 2. 基于配置启动

项目跟目录创建 `alinode.config.json` 内容如下：

> [Agenthub 文档](https://github.com/aliyun-node/agenthub)

```json
{
  "appid": "<YOUR APPID>",
  "secret": "<YOUR SECRET>",
  "logdir": "/tmp/",
  "error_log": [
    "</path/to/your/error.log>",
    "您的应用在业务层面产生的异常日志的路径",
    "例如：/root/.logs/error.#YYYY#-#MM#-#DD#-#HH#.log",
    "可选"
  ],
  "packages": [
    "</path/to/your/package.json>",
    "可以输入多个package.json的路径",
    "可选"
  ]
}
```

然后启动容器：

```sh
$ docker run -d \
  -p 8000:8000 \
  -v $PWD:/app \
  -h my-alinode \
  --name my-alinode \
  toomee/alinode:5
```

### 常用命令

命令 | 描述
:-- | :--
`$ docker exec my-alinode pm2 monit` | 监控每个进程 CPU 使用情况
`$ docker exec my-alinode pm2 list` | 列出进程
`$ docker exec my-alinode pm2 logs` | 查看日志
`$ docker exec my-alinode pm2 reload all` | 无缝重启所有进程

**PS: 具体使用说明看 PM2 和 alinode 文档**


## 高级应用

多环境基于配置部署方案：

添加 `alinode.config.json`, `alinode.config.pre.json` 到根目录。

启动 pre 环境容器：

```sh
$ docker run -d \
  -p 8000:8000 \
  -v $PWD:/app \
  -e "ALINODE_CONFIG=alinode.config.pre.json" \
  -h my-alinode \
  --name my-alinode \
  toomee/alinode:5
```

启动 正式 环境容器：

```sh
$ docker run -d \
  -p 8000:8000 \
  -v $PWD:/app \
  -h my-alinode \
  --name my-alinode \
  toomee/alinode:5
```

## docker-compose

也可以使用 docker-compose.yml 启动。

```yml
web:
  image: toomee/alinode:5
  restart: always
  hostname: my-alinode
  container_name: my-alinode
  environment:
    APP_ID: 应用ID
    APP_SECRET: 应用密钥
    # 或者使用自定义环境配置
    # ALINODE_CONFIG: alinode.config.pre.json
  ports:
    - 8000:8000
  volumes:
    - $PWD:/app
```

常用命令

```sh
$ docker-compose pull    # 更新/拉取镜像
$ docker-compose up -d   # 启动
$ docker-compose restart # 重启
$ docker-compose down    # 关闭并删除
```

## 特别说明

`-h my-alinode` 是容器 hostname 最终会显示在 <https://node.console.aliyun.com/> 平台实例列表中。
如果不写，会显示默认容器名，也就是随机值。

## 自定义

推荐自定义修改 Dockerfile 然后构建合适自己的镜像。  
由于官网还没开源，所以 Dockerfile 也是未知的，官方镜像就像是黑匣子。  
这个是我研究官网镜像总结出来的，是基于官网镜像直接抽取的，兼容性目前来看没啥问题。  
有问题，欢迎反馈。
