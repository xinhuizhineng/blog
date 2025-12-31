---
title: "Vue 项目 Nginx 生产环境部署与配置全指南"
author: "常香玉"              # 文章作者
description: "本文详细介绍了 Vue 项目构建后的 Nginx 部署流程，涵盖 Nginx 核心配置、前端路由模式、环境变量管理以及 Windows 环境下的常用操作命令。"
date: 2023-04-27T13:05:18+08:00
lastmod: 2023-04-27         # 文章修改日期
categories: [
   "运维部署",
   "Vue"
]
tags: [
   "Nginx",
   "Vue.js",
   "Vite",
   "Windows"
]
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
```
### ⚠️ 关键注意事项
- **proxy_pass 的斜杠**：
  - `proxy_pass http://ip:port/;`（带斜杠）：会将 `/vpsmobileh5/api` 转发为 `/api`（去除了前缀）。
  - `proxy_pass http://ip:port;`（不带斜杠）：会将 `/vpsmobileh5/api` 转发为 `/vpsmobileh5/api`（保留前缀）。
    
## 3. 前端项目配置详解
为了配合 Nginx 部署，前端代码层面也需要进行相应的适配。

### 3.1 路由模式与基础路径
Vue Router 主要有两种模式：`hash` 和 `history`。

- **推荐配置**：虽然 History 模式 URL 更美观，但若为了简化服务器配置或兼容性，**Hash 模式**通常更省心。
- **路径配置**：建议设置 `base` 为相对路径 `./`，使静态资源能自动适应部署的子目录。
配置示例 (manifest.json 或 router 配置):
```
{
  "h5" : {
    "router" : {
      "mode" : "hash", 
      "base": "./" 
    }
  }
}
```
资源引用规范：

- ❌ 避免使用 `../../static/icon/logo.png` 这种跨层级相对路径。
- ✅ 推荐使用 `static/icon/logo.png` 或绝对路径，确保基于当前目录结构查找。
### 3.2 环境变量管理 (API 基地址)
根据构建工具的不同（Vite vs Webpack/Vue CLI），环境变量的处理方式有所区别。

**场景 A：Vite 项目 (Vue 3 / UniApp)**
1. 创建配置文件 `.env.production`:
```
NODE_ENV=production
# 变量名必须以 VITE_ 开头
VITE_BASE_URL=http://api.prod.com
```
2. 代码中调用:
```
console.log('当前环境', import.meta.env.MODE);

export const defaultUrl = import.meta.env.MODE === 'production' 
    ? import.meta.env.VITE_BASE_URL 
    : 'http://172.168.50.73:6603';
```
**场景 B：Vue CLI 项目 (Vue 2)**
1. 创建配置文件 `.env.production`:
```
NODE_ENV=production
# 变量名必须以 VUE_APP_ 开头
VUE_APP_BASE_URL=http://api.prod.com
```
2. 代码中调用:
```
console.log('当前环境', process.env.NODE_ENV);

export const defaultUrl = process.env.NODE_ENV === 'production' 
    ? process.env.VUE_APP_BASE_URL 
    : 'http://172.168.81.177:9001';

```
### 3.3 构建公共路径 (Vite Config)
在 `vite.config.js` 中配置 `base`，决定了 HTML 中引入 JS/CSS 的路径前缀。
```
import { defineConfig } from "vite";
import uni from "@dcloudio/vite-plugin-uni";

export default defineConfig({
  plugins: [uni()],
  base: '/', // 默认值，表示从根路径加载。如果部署在子目录，请修改此处
  server: {
    port: 9011,
  }
})
```
## 4. Windows 环境与 IIS 混合部署指南
如果您的生产环境是 Windows Server，且涉及 IIS 与 Nginx 共存，请参考以下操作。

### 4.1 部署结构建议
1. **IIS 配置**：新建网站，指向项目物理路径（如 `D:\WebSite\YKT`）。
2. **Nginx 配置**：作为反向代理或静态服务器，配置文件位于 `D:\Nginx\nginx-1.15.6\conf\nginx.conf`。
### 4.2 Nginx 常用命令 (CMD)
请在 Nginx 安装目录（如 `D:\Nginx\nginx-1.15.6`）下打开 CMD 执行：
| 操作 | 命令 | 说明 |
| :--- | :--- | :--- |
| **启动** | `start nginx` | 启动服务，在 Windows 下窗口闪退属正常现象（进程已在后台运行）。 |
| **重载** | `nginx -s reload` | 修改 `nginx.conf` 后，无需重启，热加载新配置。 |
| **停止** | `nginx -s stop` | 快速停止 Nginx 服务。 |
| **退出** | `nginx -s quit` | 处理完当前请求后，优雅退出服务。 |
| **检测** | `nginx -t` | **强烈推荐**。在重载前运行，用于检测配置文件语法是否正确。 |

### 4.3 排查端口占用
如果启动失败，通常是端口冲突导致。
```
# 1. 查看占用 80 端口的进程 PID
netstat -aon | findstr "80"

# 2. 根据 PID 查看是哪个程序 (假设 PID 为 2276)
tasklist | findstr "2276"
```
### ⚠️ Windows 下的严重警告
**不要使用记事本 (Notepad) 编辑** `nginx.conf`！ 记事本保存时会自动添加 **BOM (Byte Order Mark)** 头，这会导致 Nginx 读取配置文件失败。 **解决方案**：请使用 VS Code、Notepad++ 或 Sublime Text 等专业编辑器。

## 5. 常见问题排查 (FAQ)
Q1: 部署后静态资源（JS/CSS）加载 404？

- 原因：构建时的 `publicPath` (Vue CLI) 或 `base` (Vite) 配置与 Nginx 的部署目录不一致。
- 解决：如果部署在根目录，设置为 `/`；如果部署在 `/app/` 子目录，设置为 `/app/` 或 `./`。
Q2: 环境变量不生效？

- 原因：前缀错误或文件未加载。
- 解决：
  - Vite 必须用 `VITE_`开头。
  - Vue CLI 必须用 `VUE_APP_` 开头。
  - 确保 `.env.production` 文件位于项目根目录。
Q3: 刷新页面出现 404 错误？

- 原因：使用了 History 路由模式，但 Nginx 未配置重定向。
- 解决：确保 Nginx 配置中包含 `try_files $uri $uri/ /index.html;`。

