---
title: "Docker容器占用大量存储"
author: "常香玉"
description: "Docker容器占用大量存储"
date: 2023-01-17T11:02:00+08:00
lastmod: 2022-01-17         # 文章修改日期
tags : [                    # 文章所属标签
    "Docker"
]
categories : [              # 文章所属标签
    "Docker",
]
---

服务产生大量core文件，导致存储空间被占满
可通过命令`find /home/docker -name core.*`查看产生core文件的路径（docker数据目录以现场配置为准）
1. 删除所有容器，包含base和c3目录下的
`docker-compose down`
2. 使用df -h检查空间是否已经恢复
3. 修改`docker-compose.yml`,在每个服务的最后增加以下配置：
```
    ulimits:
        core: 0
```
![](images/web/docker-1.png)
5. 启动docker容器
`docker-compose up -d`
