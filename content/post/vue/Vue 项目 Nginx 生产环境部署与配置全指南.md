---
title: "Vue 项目 Nginx 生产环境部署与配置全指南"
date: 2023-10-27
categories:
  - "运维部署"
  - "Vue"
tags:
  - "Nginx"
  - "Vue.js"
  - "Vite"
  - "Windows"
description: "本文详细介绍了 Vue 项目构建后的 Nginx 部署流程，涵盖 Nginx 核心配置、前端路由模式、环境变量管理以及 Windows 环境下的常用操作命令。"
---

## 概述

本文档旨在介绍 Vue 项目（包括 Vue CLI、Vite 及 UniApp H5）构建完成后，如何使用 Nginx 进行生产环境的部署与配置。内容涵盖 Nginx 的核心配置、前端路由与路径设置、环境变量管理以及在 Windows 环境下的特殊操作指南。

## 1. 前置要求

在开始部署前，请确保满足以下条件：
*   **构建完成**：Vue 项目已完成开发，并通过流水线或本地构建命令打包生成了静态资源（通常为 `dist` 目录）。
*   **环境准备**：服务器已安装 Nginx。
*   **文件部署**：构建好的静态文件已上传至服务器指定目录。

---

## 2. Nginx 核心配置

以下是一个标准的 Nginx `server` 配置块，适用于单页应用（SPA）的部署。

### 配置文件示例 (`nginx.conf`)

```nginx
server {
    listen       80;
    server_name  test.wxy.cn; # 请替换为实际域名

    # 1. 开启 Gzip 压缩（优化加载速度）
    gzip on;
    gzip_min_length 1k;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/javascript application/json application/javascript application/xml+rss;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";

    # 2. 前端静态资源托管
    location / {
        root   /usr/share/nginx/html; # 静态资源存放路径
        index  index.html;
        # 核心配置：解决 History 模式路由刷新 404 问题
        try_files $uri $uri/ /index.html;
    }

    # 3. 接口反向代理配置
    location /vpsmobileh5 {
        # 如果需要重写路径，可取消注释下面这行
        # rewrite ^/vpsmobileh5(.*)$ \$1 break;
        
        # ⚠️ 注意：proxy_pass 结尾带 / 表示去除匹配路径
        proxy_pass   http://172.168.50.77:16690/; 
        
        proxy_redirect             off;
        proxy_set_header           Host $host;
        proxy_set_header           X-Real-IP $remote_addr;
        proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # 支持 WebSocket
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        
        client_max_body_size       200m;
        expires 0;
    }
}

