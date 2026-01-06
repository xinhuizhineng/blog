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

## 第一步：清理旧环境（重要）
注意： 在安装 NVM 之前，必须彻底卸载电脑上现有的 Node.js 版本并删除残留文件，否则极易产生路径冲突。  

### Windows:
1. 打开“控制面板” -> “卸载程序”，卸载 Node.js。
2. 手动检查并删除安装目录（通常为 C:\Program Files\nodejs）。
3. 检查用户目录下是否存在 node_modules 或 .npm 文件夹，建议一并删除。
### macOS:
- 如果之前通过 pkg 安装，建议手动删除 /usr/local/bin/node、/usr/local/lib/node_modules 等相关文件。
- 或运行 brew uninstall node (如果是通过 Homebrew 安装)。
## 第二步：安装 NVM
### 方案 A：Windows 系统 (nvm-windows)
1. 下载与安装
- 下载地址：GitHub - nvm-windows releases
- 操作：推荐下载 nvm-setup.exe。双击运行，按照提示点击“下一步”即可。
  - 注意：安装路径建议保持默认，或者确保路径中不包含空格和中文字符。
2. 配置环境变量（新增详细步骤）
虽然安装程序通常会自动配置，但为了确保 nvm 命令在任何终端都能生效，建议手动检查或配置：  

  1. 打开环境变量设置：
  - 右键“此电脑” -> “属性” -> “高级系统设置” -> “环境变量”。
  2. 检查/新建用户变量：
  - NVM_HOME：变量值设为 NVM 的安装目录（例如：C:\Users\YourUser\AppData\Roaming\nvm）。
  - NVM_SYMLINK：变量值设为 Node.js 的快捷方式映射目录（例如：C:\Program Files\nodejs）。
  3. 编辑 Path 变量：
  - 在“用户变量”或“系统变量”中找到 Path，点击“编辑”。
  - 确保列表中包含以下两项（如果没有则新建）：
  ```
  %NVM_HOME%
  %NVM_SYMLINK%
  ```
  4. 保存并生效：
  - 连续点击“确定”保存所有设置。
  - 重启 CMD 或 PowerShell 窗口以加载新的环境变量。
3. 配置国内下载镜像
为了解决 Node.js 下载慢的问题，请在管理员权限的 CMD/PowerShell 中运行：  
```
bash
nvm node_mirror https://npmmirror.com/mirrors/node/
nvm npm_mirror https://npmmirror.com/mirrors/npm/
```
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
|命令	|说明 |
|nvm install <version>	|安装指定版本 (如 nvm install 20.10.0)|
|nvm use <version>	|切换到指定版本|
|nvm list	|列出本地已安装的所有版本|
|nvm uninstall <version>	|卸载指定版本|
|nvm alias default <version>	|(Mac/Linux) 设置默认版本，新开终端自动生效|
|node -v	|查看当前正在使用的 Node 版本|
|npm -v	|查看当前正在使用的 NPM 版本|

## 💡 常见问题提示
权限问题：Windows 用户在执行 nvm use 切换版本时，必须使用管理员身份运行 CMD 或 PowerShell，否则无法创建软链接。
路径空格：确保 NVM 安装路径不包含空格（如 Program Files 有时会引起兼容性问题，建议安装在 C:\nvm 或类似简单路径）。
