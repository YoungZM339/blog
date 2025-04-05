---
title: "在 Windows 上使用 Docker 部署 MySQL 和 Redis 服务"
date: 2025-03-30T00:00:01+08:00
description: "一个用于辅助教学的教程，记录相关的操作步骤和注意事项。"
categories: "教学教程"
tags: ["docker", "mysql", "redis"]

draft: false
---
### 在 Windows 上使用 Docker 部署 MySQL 和 Redis 服务教程

本教程将指导你在 **Windows 系统**上通过 Docker 快速部署 MySQL 和 Redis 服务，适用于本地开发环境搭建。

---

#### 准备工作

1. **系统要求**
   - Windows 10/11（建议最新版本）
   - 管理员权限的 PowerShell 或 CMD

---

#### 步骤 1：安装 Chocolatey（Windows 包管理器）

```powershell
winget install --id=Chocolatey.Chocolatey -e
```

- **作用**：通过 Windows 官方工具 `winget` 安装 Chocolatey，用于后续安装 Docker Desktop。
- **注意**：如果提示权限问题，请以管理员身份运行 PowerShell。

---

#### 步骤 2：启用 WSL（Windows 子系统 Linux）

```powershell
wsl --install
```

- **作用**：安装 WSL 2，这是 Docker Desktop 在 Windows 上的依赖环境。
- **完成后需重启电脑**。

---

#### 步骤 3：安装 Docker Desktop

```powershell
choco install docker-desktop --version=4.25.0
```

- **作用**：通过 Chocolatey 安装指定版本的 Docker Desktop（社区版）。
- **验证安装**：  
  安装完成后，启动 Docker Desktop，任务栏出现鲸鱼图标即表示成功。

---

#### 步骤 4：部署 MySQL 服务

```powershell
docker run --name=mysql-server -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server
```

- **参数解释**：
  - `--name=mysql-server`：容器名称
  - `-p 3306:3306`：将本地 3306 端口映射到容器的 3306 端口
  - `-e MYSQL_ROOT_PASSWORD=123456`：设置 root 用户密码（生产环境请使用复杂密码）
  - `mysql/mysql-server`：官方 MySQL 镜像
- **验证是否运行**：
  ```powershell
  docker ps -a | findstr "mysql-server"
  ```

---

#### 步骤 5：部署 Redis 服务

```powershell
docker run -d --name=redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

- **参数解释**：
  - `redis/redis-stack-server:latest`：Redis 官方镜像（包含 RedisInsight 可视化工具）
- **验证是否运行**：
  ```powershell
  docker ps -a | findstr "redis-stack-server"
  ```

---

#### 测试服务可用性

1. **测试 MySQL**  
   使用数据库工具（如 MySQL Workbench）连接：

   - Host: `127.0.0.1`
   - Port: `3306`
   - Username: `root`
   - Password: `123456`

2. **测试 Redis**  
   使用 `redis-cli` 或 RedisInsight（访问 `http://localhost:8001`）连接：
   ```powershell
   docker exec -it redis-stack-server redis-cli
   ```

---

#### 常见问题

1. **端口冲突**  
   如果 3306 或 6379 端口被占用，修改 `-p` 参数（如 `-p 3307:3306`）。
2. **镜像拉取失败**  
   尝试更换镜像源：

   ```powershell
   docker pull mysql/mysql-server:latest
   docker pull redis/redis-stack-server:latest
   ```

3. **数据持久化（可选）**  
   添加 `-v` 参数挂载数据卷（示例）：
   ```powershell
   docker run --name=mysql-server -v C:/mysql_data:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server
   ```

---

#### 总结

通过以上步骤，你已成功部署了：

- MySQL 服务（端口 3306）
- Redis 服务（端口 6379）

现在可以开始本地开发调试！如需停止服务，使用 `docker stop <容器名>`；删除服务用 `docker rm <容器名>`。
