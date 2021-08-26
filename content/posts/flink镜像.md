---
title: "Flink镜像"
date: 2021-08-24T20:44:21+08:00
draft: false
---

### 前言

最近在使用 pyflink 做一些特征开发工作，我们内部写了一个代码生成的命令行生成 pyflink 代码。由于这部分工作日后会交与算法同事，为了便于他们在本地开发和调试 pyflink 代码，便封装了一个 pyflink 镜像。

### flink docker image

flink 提供了官方 [image ](https://hub.docker.com/_/flink),以及对应的[使用文档](https://ci.apache.org/projects/flink/flink-docs-release-1.13/docs/deployment/resource-providers/standalone/docker/)，但该 image 并没有内置 pyflink 环境。

```dockerfile
FROM flink:1.13.0

# install python3 and pip3
RUN apt-get update -y && \
apt-get install -y python3.7 python3-pip python3.7-dev && rm -rf /var/lib/apt/lists/*
RUN ln -s /usr/bin/python3 /usr/bin/python

RUN pip3 install pip --upgrade

# install Python Flink
# why need --no-cache-dir option? : https://stackoverflow.com/questions/53204916/what-is-the-meaning-of-failed-building-wheel-for-x-in-pip-install
RUN pip3 install apache-flink --no-cache-dir
```

相关镜像放到了 docker hub 上，[在这](https://hub.docker.com/layers/151820205/1034552569/pyflink/v0.5/images/sha256-79f6e9107336bac98b2e1d826ab88f1ccd1189d559ad1e296cdfa229b415fd64?context=explore)。有了这个镜像，开发人员就可以方便的在本地拉起 flink 环境了，具体的使用方式稍后会更新上来。
