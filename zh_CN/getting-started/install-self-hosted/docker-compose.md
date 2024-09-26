# Docker Compose 部署

### 前提条件

> 安装 Dify 之前, 请确保你的机器已满足最低安装要求：
> - CPU >= 2 Core
> - RAM >= 4GB

| 操作系统                       | 软件                                                             | 描述                                                                                                                                                                                   |
| -------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| macOS 10.14 or later       | Docker Desktop                                                 | 为 Docker 虚拟机（VM）至少分配 2 个虚拟 CPU(vCPU) 和 8GB 初始内存，否则安装可能会失败。有关更多信息，请参考 [《在 Mac 内安装 Docker 桌面端》](https://docs.docker.com/desktop/install/mac-install/)。                                 |
| Linux platforms            | <p>Docker 19.03 or later<br>Docker Compose 1.28 or later</p> | 请参阅[安装 Docker](https://docs.docker.com/engine/install/) 和[安装 Docker Compose](https://docs.docker.com/compose/install/) 以获取更多信息。                                                      |
| Windows with WSL 2 enabled | <p>Docker Desktop<br></p>                                      | 我们建议将源代码和其他数据绑定到 Linux 容器中时，将其存储在 Linux 文件系统中，而不是 Windows 文件系统中。有关更多信息，请参阅[使用 WSL 2 后端在 Windows 上安装 Docker Desktop](https://docs.docker.com/desktop/windows/install/#wsl-2-backend)。 |

### 克隆 Dify 代码仓库

克隆 Dify 源代码至本地环境。

```bash
git clone https://github.com/langgenius/dify.git
```

### 启动 Dify

1. 进入 Dify 源代码的 Docker 目录

   ```bash
   cd dify/docker
   ```

2. 复制环境配置文件

    ```bash
   cp .env.example .env
   ```

3. 启动 Docker 容器

    根据你系统上的 Docker Compose 版本，选择合适的命令来启动容器。你可以通过 `$ docker compose version` 命令检查版本，详细说明请参考 [Docker 官方文档](https://docs.docker.com/compose/#compose-v2-and-the-new-docker-compose-command)：

    - 如果版本是 Docker Compose V2，使用以下命令：
  
    ```bash
    docker compose up -d
    ```

    - 如果版本是 Docker Compose V1，使用以下命令：

    ```bash
    docker-compose up -d
    ```

运行命令后，你应该会看到类似以下的输出，显示所有容器的状态和端口映射：

```Shell
[+] Running 11/11
 ✔ Network docker_ssrf_proxy_network  Created                                                                 0.1s 
 ✔ Network docker_default             Created                                                                 0.0s 
 ✔ Container docker-redis-1           Started                                                                 2.4s 
 ✔ Container docker-ssrf_proxy-1      Started                                                                 2.8s 
 ✔ Container docker-sandbox-1         Started                                                                 2.7s 
 ✔ Container docker-web-1             Started                                                                 2.7s 
 ✔ Container docker-weaviate-1        Started                                                                 2.4s 
 ✔ Container docker-db-1              Started                                                                 2.7s 
 ✔ Container docker-api-1             Started                                                                 6.5s 
 ✔ Container docker-worker-1          Started                                                                 6.4s 
 ✔ Container docker-nginx-1           Started                                                                 7.1s
```

最后检查是否所有容器都正常运行：

```bash
docker compose ps
```

在这个输出中，你应该可以看到包括 3 个业务服务 `api / worker / web`，以及 6 个基础组件 `weaviate / db / redis / nginx / ssrf_proxy / sandbox` 。

```bash
NAME                  IMAGE                              COMMAND                   SERVICE      CREATED              STATUS                        PORTS
docker-api-1          langgenius/dify-api:0.6.13         "/bin/bash /entrypoi…"   api          About a minute ago   Up About a minute             5001/tcp
docker-db-1           postgres:15-alpine                 "docker-entrypoint.s…"   db           About a minute ago   Up About a minute (healthy)   5432/tcp
docker-nginx-1        nginx:latest                       "sh -c 'cp /docker-e…"   nginx        About a minute ago   Up About a minute             0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp
docker-redis-1        redis:6-alpine                     "docker-entrypoint.s…"   redis        About a minute ago   Up About a minute (healthy)   6379/tcp
docker-sandbox-1      langgenius/dify-sandbox:0.2.1      "/main"                   sandbox      About a minute ago   Up About a minute             
docker-ssrf_proxy-1   ubuntu/squid:latest                "sh -c 'cp /docker-e…"   ssrf_proxy   About a minute ago   Up About a minute             3128/tcp
docker-weaviate-1     semitechnologies/weaviate:1.19.0   "/bin/weaviate --hos…"   weaviate     About a minute ago   Up About a minute             
docker-web-1          langgenius/dify-web:0.6.13         "/bin/sh ./entrypoin…"   web          About a minute ago   Up About a minute             3000/tcp
docker-worker-1       langgenius/dify-api:0.6.13         "/bin/bash /entrypoi…"   worker       About a minute ago   Up About a minute             5001/tcp
```

通过这些步骤，你应该可以成功在本地安装 Dify。

### 更新 Dify

进入 dify 源代码的 docker 目录，按顺序执行以下命令：

```bash
cd dify/docker
docker compose down
git pull origin main
docker compose pull
docker compose up -d
```

#### 同步环境变量配置 (重要！)

* 如果 `.env.example` 文件有更新，请务必同步修改您本地的 `.env` 文件。
* 检查 `.env` 文件中的所有配置项，确保它们与您的实际运行环境相匹配。您可能需要将 `.env.example` 中的新变量添加到 `.env` 文件中，并更新已更改的任何值。

### 访问 Dify

你可以先前往管理员初始化页面设置设置管理员账户：

```bash
# 本地环境
http://localhost/install

# 服务器环境

http://your_server_ip/install
```

Dify 主页面：

```bash
# 本地环境
http://localhost

# 服务器环境
http://your_server_ip
```

### 自定义配置

编辑 `.env` 文件中的环境变量值。然后重新启动 Dify：

```bash
docker compose down
docker compose up -d
```

完整的环境变量集合可以在 `docker/.env.example` 中找到。
