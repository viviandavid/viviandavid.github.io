---
layout:     post
title:      stack 一个更优秀的服务管理者
subtitle:   集群服务的管理
date:       2019-08-06
author:     BY LSD
header-img: img/post-bg-swift.jpg
catalog: true
tags:
    - docker
    - stack
---

推荐博客 [stack](https://blog.csdn.net/u011499747/article/details/74019957)


### stack的前提
swarm集群搭建完成，镜像已经打包完成，放在镜像仓库中。可以参考我前面的文章。

### compose.yml文件

```
version: "3.2"
services:
#测试web
  daa-test:
    image: xx:xx:xx:xx:123456/bizteam/daa-test:1.0-SNAPSHOT
#    hostname: testweb
    ports:
      - "8072:8072"
    networks:
      test-net:
        aliases:
          - daa-test-net
    environment:
      - EUREKA_ADDRESS=eureka-net
      - SPRING_PROFILES_ACTIVE=docker
      - MYSQL_IP=192.168.164.10
      - MYSQL_DATABASE=test
      - MYSQL_USERNAME=root
      - MYSQL_PASSWORD=123456
      - IS_SHOW_IP=false
      - HOST_NAME=daa-testweb
    deploy:
      replicas: 1
      restart_policy:
        condition: on-failure
      resources:
        limits:
          memory: 1GB
        reservations:
          memory: 512M

#第二个web省略。。
  

networks:
  test-net:
    external:
      name: test-net

```

执行yml文件
> docker stack deploy -c docker-stack-daa-test.yml test --with-registry-auth --resolve-image=never

stack查看
> docker stack ls

stack service 查看
> docker stack services serviceName

service 查看
> docker service ls

service 详情查看
> docker service inspect  id/name


