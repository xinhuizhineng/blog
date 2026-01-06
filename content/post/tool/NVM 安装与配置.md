---
title: "NVM 安装与配置完全指南"
author: "常香玉"
description: "本指南涵盖 Windows 和 macOS/Linux 系统的安装、环境配置及常用命令。"
date: 2024-01-18T11:02:00+08:00
lastmod: 2024-01-18         # 文章修改日期
tags : [                    # 文章所属标签
    "NVM"
]
categories : [              # 文章所属标签
    "Web前端",
    "NVM"
]
---

# NVM 安装与配置完全指南

NVM (Node Version Manager) 是管理 Node.js 版本的利器，允许您在同一台设备上切换不同版本的 Node.js，以适应不同的项目需求。

## 第一步：清理旧环境（重要）  

**注意：** 在安装 NVM 之前，必须彻底卸载电脑上现有的 Node.js 版本并删除残留文件，否则极易产生路径冲突。

### Windows 用户
1.  打开“控制面板” -> “卸载程序”，卸载 Node.js。
2.  手动检查并删除安装目录（通常为 `C:\Program Files\nodejs`）。
3.  检查用户目录下（`C:\Users\用户名`）是否存在 `node_modules`、`.npm` 或 `.npmrc` 文件/文件夹，建议一并删除。

### macOS 用户
*   如果之前通过 pkg 安装，建议手动删除 `/usr/local/bin/node`、`/usr/local/lib/node_modules` 等相关文件。
*   如果是通过 Homebrew 安装，运行：`brew uninstall node`。

## 第二步：安装 NVM

### 方案 A：Windows 系统 (nvm-windows)

#### 1. 下载与安装
*   **下载地址**：[GitHub - nvm-windows releases](https://github.com/coreybutler/nvm-windows/releases)
*   **操作**：推荐下载 `nvm-setup.exe`。双击运行，按照提示点击“下一步”即可。
    *   *提示*：安装路径建议保持默认，或者确保路径中**不包含空格和中文字符**。

#### 2. 环境变量配置 (关键步骤)
虽然安装程序通常会自动配置，但为了确保稳定性，建议手动检查以下配置：

1.  **打开设置**：右键“此电脑” -> “属性” -> “高级系统设置” -> “环境变量”。
2.  **检查/新建用户变量**：

| 变量名 | 变量值示例 | 说明 |
| :--- | :--- | :--- |
| `NVM_HOME` | `C:\Users\YourUser\AppData\Roaming\nvm` | NVM 的安装目录 |
| `NVM_SYMLINK` | `C:\Program Files\nodejs` | Node.js 的快捷方式映射目录 |

3.  **编辑 Path 变量**：
    在“用户变量”或“系统变量”中找到 `Path`，点击“编辑”，确保列表中包含以下两项引用：
    ```text
    %NVM_HOME%
    %NVM_SYMLINK%
    ```
4.  **生效**：保存设置后，**重启** CMD 或 PowerShell 窗口。

#### 3. 配置 NVM 国内镜像
为了解决下载慢的问题，请在**管理员权限**的终端中运行：
```bash
nvm node_mirror https://npmmirror.com/mirrors/node/
nvm npm_mirror https://npmmirror.com/mirrors/npm/
```

#### 进阶配置：自定义 NPM 全局路径 (Windows 推荐)
此步骤可选，但强烈建议配置，以便统一管理全局包（如 vue-cli, yarn）。  

1. 新建文件夹
在您的 NVM 或任意目录下手动创建两个文件夹：

D:\nodejs\node_global (存放全局包)
D:\nodejs\node_cache (存放缓存)
2. 修改 NPM 配置
打开终端执行：
```
bash
# 设置全局模块的安装路径
npm config set prefix "D:\nodejs\node_global"
# 设置缓存路径
npm config set cache "D:\nodejs\node_cache"
```
执行完毕后，您可以运行 npm config list 检查是否生效。  

3. 配置全局路径环境变量 (Environment Variables)

如果不配置此项，安装的全局命令（如 vue）将无法识别。  

操作路径：右键“此电脑” -> 属性 -> 高级系统设置 -> 环境变量。  
    - 配置 Path 变量 (系统变量/用户变量)：
        - 找到 `Path` 变量 -> 编辑 -> 新建。
        - 添加路径：`D:\nodejs\node_global`
    - 配置 NODE_PATH (系统变量)，在“系统变量”区域，点击“新建”：
        - 新建变量名：`NODE_PATH`
        - 变量值：`D:\nodejs\node_global\node_modules`
        - 注意：必须多加一层 `node_modules`，否则 require 无法找到模块。
    - 验证： 重启终端后，运行 `npm install express -g`，检查是否安装到了新目录。

特别提示（如果您正在使用 NVM）
如果您是配合 NVM 使用此配置，这意味着所有 Node 版本都会共用这个 node_global。  

- 优点：切换 Node 版本后，不需要重新安装全局包（如 yarn, cnpm）。
- 缺点：如果某个全局包依赖特定的 Node 版本（例如旧版 Sass），切换 Node 版本后可能会导致该工具报错。

### 方案 B：macOS / Linux 系统 (nvm)
1. 安装脚本
打开终端，运行官方安装脚本：
```
bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
(如果网络连接失败，请尝试搜索“nvm gitee”寻找国内搬运脚本)

2. 配置环境变量 
安装脚本通常会自动写入配置文件。如果没有生效（输入 nvm 提示 command not found），请手动配置：  

  1. 编辑配置文件：
  ```
  macOS (Zsh): vim ~/.zshrc
  Linux (Bash): vim ~/.bashrc
  ```
  3. 添加以下内容：
  ```
  bash
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # 加载 nvm
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # 加载自动补全
  
  #在此处也可以直接配置国内镜像变量
  export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
  ```
  3. 使配置生效：
  ```
  bash
  source ~/.zshrc  # 或 source ~/.bashrc
  ```
## 第三步：使用 NVM 安装 Node.js
配置好 NVM 后，即可灵活管理 Node 版本。  

1. 查看可安装版本：
```
bash
nvm list available  # Windows
nvm ls-remote       # Mac/Linux
```
2. 安装指定版本（推荐安装 LTS 长期支持版）：
```
bash
nvm install 18.17.0   # 安装具体的版本号
nvm install lts       # 自动安装最新的 LTS 版本
```
3. 切换/使用版本：
```
bash
nvm use 18.17.0
(Windows 用户如果遇到 "exit status 1" 乱码报错，请右键选择*“以管理员身份运行”*终端)
```
4. 查看当前已安装版本：
```
bash
nvm list
```

## 第四步：配置 NPM 镜像（加速依赖包下载）  
安装完 Node.js 后，npm 命令自动可用。建议配置淘宝镜像（npmmirror）以加速 npm install。    

1. 设置镜像源：
```
bash
npm config set registry https://registry.npmmirror.com
```
2. 验证是否成功：
```
bash
npm config get registry
# 预期输出: https://registry.npmmirror.com/
```
## 常用命令速查表
| 命令 | 说明 |
| :--- | :--- |
| `nvm install <version>`	| 安装指定版本 (如 nvm install 20.10.0) |
| `nvm use <version>`	| 切换到指定版本 |
| `nvm list` | 列出本地已安装的所有版本 |
| `nvm uninstall <version>` | 卸载指定版本 |
| `nvm alias default <version>` | (Mac/Linux) 设置默认版本，新开终端自动生效 |
| `node -v` |查看当前正在使用的 Node 版本 |
| `npm -v` |查看当前正在使用的 NPM 版本 |

## 💡 常见问题提示
权限问题：Windows 用户在执行 nvm use 切换版本时，必须使用管理员身份运行 CMD 或 PowerShell，否则无法创建软链接。
路径空格：确保 NVM 安装路径不包含空格（如 Program Files 有时会引起兼容性问题，建议安装在 C:\nvm 或类似简单路径）。
