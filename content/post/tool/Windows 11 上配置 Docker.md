一、系统要求与准备
1. 系统要求
Windows 11 64位：家庭版/专业版/企业版

必须启用硬件虚拟化（BIOS/UEFI中开启Intel VT-x/AMD-V）

WSL 2 需要 Windows 10 版本 1903 或更高（Windows 11 已内置）

至少 4GB RAM（建议8GB以上）

存储空间：至少 20GB 可用空间

2. 检查虚拟化是否开启
powershell
# 打开 PowerShell（管理员）
systeminfo

# 或使用任务管理器查看
# Ctrl+Shift+Esc → 性能 → CPU → "虚拟化: 已启用"
如果未启用，需要重启进入 BIOS/UEFI 开启：

Intel CPU：开启 Intel Virtualization Technology (VT-x)

AMD CPU：开启 AMD-V

二、安装 Docker Desktop for Windows
方法1：官方安装（推荐）
下载 Docker Desktop

powershell
# 官方下载地址（最新稳定版）
https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe
运行安装程序

双击安装文件

勾选 "Use WSL 2 instead of Hyper-V"（推荐）

安装完成后不要立即启动

启用 WSL 2 功能

powershell
# 以管理员身份打开 PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 重启电脑
shutdown /r /t 0
设置 WSL 2 为默认版本

powershell
wsl --set-default-version 2
方法2：使用 Winget 安装
powershell
# 搜索 Docker Desktop
winget search Docker.Desktop

# 安装
winget install Docker.DockerDesktop
三、安装后的配置
1. 首次启动配置
启动 Docker Desktop

接受服务条款

选择 "Use WSL 2 based engine"（推荐）

配置 WSL 集成：

text
[x] Enable integration with my default WSL distro
[x] Ubuntu (或你安装的其他 WSL 发行版)
2. 验证安装
powershell
# 打开 PowerShell 或 CMD
docker --version
# Docker version 24.0.6, build ed223bc

docker-compose --version
# Docker Compose version v2.21.0

docker run hello-world
# 如果看到欢迎信息，说明安装成功

# 查看 Docker 信息
docker info
3. WSL 2 后端配置
powershell
# 查看 WSL 状态
wsl -l -v
# 应该显示：
#   NAME      STATE           VERSION
# * Ubuntu    Running         2
#   docker-desktop-data Running 2
#   docker-desktop    Running 2

# 如果版本不是2，升级：
wsl --set-version Ubuntu 2

# 设置默认发行版
wsl --set-default Ubuntu
四、Docker Desktop 设置优化
1. 资源分配
打开 Docker Desktop → Settings → Resources：

CPU：建议分配 4-8 核（根据你的 CPU 核心数）

Memory：建议分配 4-8 GB（不要超过总内存的50%）

Swap：1-2 GB

Disk image size：至少 50 GB

2. Docker Engine 配置
json
// Settings → Docker Engine
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ],
  "insecure-registries": [],
  "debug": false,
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "builder": {
    "gc": {
      "enabled": true,
      "defaultKeepStorage": "10GB"
    }
  }
}
3. WSL 2 设置
Enable integration with additional WSL 2 distros：选择要集成的发行版

Start Docker Desktop when you log in：根据需要开启

Expose daemon on tcp://localhost:2375 without TLS：不建议开启

五、常用命令与操作
基础命令
powershell
# 镜像操作
docker images                    # 查看镜像
docker pull ubuntu:latest        # 拉取镜像
docker rmi <image_id>           # 删除镜像

# 容器操作
docker ps                        # 查看运行中的容器
docker ps -a                     # 查看所有容器
docker run -it ubuntu bash       # 运行交互式容器
docker start <container_id>     # 启动容器
docker stop <container_id>      # 停止容器
docker rm <container_id>        # 删除容器

# 清理
docker system prune -a           # 清理所有未使用的资源
Windows 特有命令
powershell
# WSL 与 Docker 交互
wsl --shutdown                   # 关闭所有 WSL 发行版
wsl -d docker-desktop           # 进入 docker-desktop WSL
wsl -d docker-desktop-data      # 进入数据卷 WSL

# 重启 Docker 服务
net stop com.docker.service
net start com.docker.service
六、开发环境配置
1. 项目结构示例
text
my-project/
├── docker-compose.yml
├── Dockerfile
├── .dockerignore
├── src/
└── data/
2. 简单 Dockerfile 示例
dockerfile
# 基础镜像
FROM python:3.11-slim

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY requirements.txt .

# 安装依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制源代码
COPY . .

# 暴露端口
EXPOSE 8000

# 启动命令
CMD ["python", "app.py"]
3. Docker Compose 示例
yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - ./src:/app/src
    environment:
      - DEBUG=1
    depends_on:
      - db
  
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
七、常见问题解决
1. Docker 启动失败
powershell
# 1. 检查 WSL 2 是否正常
wsl --status

# 2. 重置 Docker
Docker Desktop → Troubleshoot → Reset to factory defaults

# 3. 重新安装 WSL 2 内核
# 下载：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
2. 网络问题
powershell
# 查看 Docker 网络
docker network ls

# 创建自定义网络
docker network create my-network

# 配置 DNS（在 Docker Desktop Settings → Docker Engine）
{
  "dns": ["8.8.8.8", "114.114.114.114"]
}
3. 磁盘空间不足
powershell
# 清理 Docker 数据
docker system prune --all --volumes

# 或通过 Docker Desktop
Settings → Resources → Advanced → Disk image location
# 可以移动到更大的磁盘
4. 权限问题
powershell
# 以管理员运行 Docker Desktop
# 或在 PowerShell 中：
net localgroup docker-users "你的用户名" /add
5. 端口被占用
powershell
# 查看端口占用
netstat -ano | findstr :端口号

# 停止占用进程
taskkill /PID 进程号 /F
八、性能优化建议
1. 文件系统性能
dockerfile
# 在 Dockerfile 中使用 .dockerignore
# .dockerignore 文件内容：
.git
node_modules
*.log
*.tmp
2. 卷映射优化
yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    volumes:
      # 使用命名卷（性能更好）
      - app_data:/data
      # 特定文件映射（避免整个目录）
      - ./config:/app/config:ro  # 只读
      
volumes:
  app_data:
3. WSL 2 性能优化
创建 %UserProfile%\.wslconfig：

ini
[wsl2]
memory=8GB        # 限制内存使用
processors=4      # 分配 CPU 核心
swap=2GB          # 交换空间
localhostForwarding=true
九、GUI 应用支持
1. 运行 GUI 应用
dockerfile
# Dockerfile 示例（运行 Firefox）
FROM ubuntu:22.04

RUN apt update && apt install -y firefox

# 允许连接到主机 X11 服务器
ENV DISPLAY=host.docker.internal:0
2. 安装 Windows 的 X 服务器
安装 VcXsrv 或 XMing

启动 XLaunch，选择 "Disable access control"

在 WSL 中设置：

bash
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
十、常用工具扩展
1. 安装 Docker 扩展
powershell
# 安装 dive（查看镜像层）
docker run --rm -it wagoodman/dive:latest

# 安装 lazydocker（TUI 管理工具）
docker run --rm -it lazyteam/lazydocker
2. VS Code 集成
安装 Docker 和 Dev Containers 扩展

在项目根目录创建 .devcontainer/devcontainer.json

按 F1 → Reopen in Container

十一、备份与迁移
1. 导出/导入镜像
powershell
# 导出镜像
docker save -o myimage.tar myimage:tag

# 导入镜像
docker load -i myimage.tar

# 导出容器
docker export -o mycontainer.tar container_id
docker import mycontainer.tar
2. 备份 Docker 数据
powershell
# 备份所有镜像
docker save $(docker images -q) -o all_images.tar

# 备份卷数据
docker run --rm -v my_volume:/data -v $(pwd):/backup ubuntu tar czf /backup/backup.tar.gz /data
快速检查清单
虚拟化已启用

WSL 2 已安装并设为默认

Docker Desktop 使用 WSL 2 后端

资源分配合理（内存/CPU/磁盘）

配置了国内镜像加速器

防火墙允许 Docker 通信
