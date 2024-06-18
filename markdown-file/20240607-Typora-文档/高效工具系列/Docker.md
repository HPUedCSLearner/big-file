[toc]





## 参考链接：



1、Docker Hub —— https://hub.docker.com/

2、**Docker — 从入门到实践** —— https://yeasy.gitbook.io/docker_practice



## 一、docker基础

### 1、docker常用命令

```bash
docker rmi ${images}
docker save -o ${my_images}.tar &{my_images}
docker load -i my-nginx-image.tar
docker stop ${container_name}
docker restart ${container_name}

```

### 2、Dockerfile

当然，可以给你一个更具体、更实用的例子。下面是一个包含简单 Python Flask 应用的 `Dockerfile` 示例。这个示例会展示如何从基础镜像开始，安装所需的软件包，复制应用代码，并运行一个 Flask Web 服务器。

#### 目录结构

首先，确保你的项目目录结构如下：

```
my-flask-app/
│
├── app/
│   └── app.py
├── requirements.txt
└── Dockerfile
```

- `app/app.py`: Flask 应用的主程序。
- `requirements.txt`: 列出所有的 Python 依赖包。
- `Dockerfile`: 用于定义如何构建这个项目的 Docker 镜像。

#### app/app.py

创建一个简单的 Flask 应用：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### requirements.txt

列出 Flask 作为依赖项：

```
flask
```

#### Dockerfile

创建一个用于构建 Flask 应用的 `Dockerfile`：

```dockerfile
# 使用官方的 Python 基础镜像
FROM python:3.9-slim

# 设置工作目录
WORKDIR /app

# 将 requirements.txt 复制到工作目录
COPY requirements.txt .

# 安装 Python 依赖包
RUN pip install -r requirements.txt

# 将应用代码复制到工作目录
COPY app app

# 暴露 Flask 默认端口
EXPOSE 5000

# 设置环境变量，告诉 Flask 在容器中运行
ENV FLASK_APP=app/app.py

# 启动 Flask 应用
CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"]
```

#### 构建和运行 Docker 镜像

1. **构建 Docker 镜像**：

   在 `my-flask-app` 目录下运行以下命令：

   ```bash
   docker build -t my-flask-app .
   ```

2. **运行 Docker 容器**：

   使用构建的镜像运行一个容器：

   ```bash
   docker run -d -p 5000:5000 my-flask-app
   ```

   这个命令将在后台运行容器，并将本地主机的 5000 端口映射到容器的 5000 端口。

3. **访问 Flask 应用**：

   打开浏览器并访问 `http://localhost:5000`，你应该会看到 "Hello, World!"。

#### 小结

这个示例展示了如何使用 `Dockerfile` 构建一个包含简单 Flask 应用的 Docker 镜像。通过定义 `Dockerfile`，你可以自动化和标准化应用的构建过程，确保其在不同环境中的一致性和可重复性。

### 3、Docker compose

#### Docker Compose 是什么

Docker Compose 是一个用于定义和运行多容器 Docker 应用的工具。通过使用 YAML 文件来配置应用的服务，Docker Compose 可以一次性启动和管理这些服务。Compose 主要解决了多容器环境下的依赖和配置管理问题，使得开发、测试和生产环境的应用部署更加简便。

#### Docker Compose 的作用

- **定义多容器应用**：通过一个 YAML 文件来定义应用的各个服务及其配置。
- **简化开发流程**：方便地管理应用的启动、停止和重启。
- **隔离环境**：能够在单机上运行多个独立的应用环境。
- **快速启动**：使用单一命令即可启动所有服务。

#### Docker Compose 示例

下面是一个使用 Docker Compose 配置和运行 Python Flask 应用和 Redis 的示例。

#### 目录结构

确保你的项目目录结构如下：

```
my-flask-redis-app/
│
├── app/
│   └── app.py
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

#### app/app.py

创建一个简单的 Flask 应用，使用 Redis 来计数：

```python
from flask import Flask
import redis

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1

@app.route('/')
def hello():
    count = get_hit_count()
    return f'Hello World! I have been seen {count} times.\n'

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

#### requirements.txt

列出 Flask 和 Redis 作为依赖项：

```
flask
redis
```

#### Dockerfile

创建一个用于构建 Flask 应用的 Dockerfile：

```dockerfile
# 使用官方的 Python 基础镜像
FROM python:3.9-slim

# 设置工作目录
WORKDIR /app

# 将 requirements.txt 复制到工作目录
COPY requirements.txt .

# 安装 Python 依赖包
RUN pip install -r requirements.txt

# 将应用代码复制到工作目录
COPY app app

# 暴露 Flask 默认端口
EXPOSE 5000

# 设置环境变量，告诉 Flask 在容器中运行
ENV FLASK_APP=app/app.py

# 启动 Flask 应用
CMD ["flask", "run", "--host=0.0.0.0"]
```

#### docker-compose.yml

使用 Docker Compose 配置 Flask 和 Redis 服务：

```yaml
version: '3'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: "redis:alpine"
```

#### 构建和运行 Docker Compose 应用

1. **构建并启动服务**：

   在 `my-flask-redis-app` 目录下运行以下命令：

   ```bash
   docker-compose up -d
   ```

   这个命令将构建并启动定义在 `docker-compose.yml` 文件中的所有服务。

2. **访问 Flask 应用**：

   打开浏览器并访问 `http://localhost:5000`，你应该会看到类似 "Hello World! I have been seen X times." 的输出。

3. **查看日志**：

   你可以使用以下命令查看服务的日志：

   ```bash
   docker-compose logs
   ```

4. **停止和删除服务**：

   使用以下命令停止并删除所有服务：

   ```bash
   docker-compose down
   ```

#### 小结

通过上述示例，你可以看到 Docker Compose 如何简化多容器应用的定义和管理。使用一个简单的 YAML 文件，你可以轻松地配置、构建和运行多服务应用。这在开发和生产环境中非常有用，能够显著提高工作效率。



## 二、docker run的常用命令配置

```bash
docker run -d --name my-nginx-container --network bridge -p 80:80 my-nginx-image

```



```base

# 目录映射
-v /home/feng/ikuuu:/clash

# 配置端口映射
-p 80:80

# 配置dockers container网络模式
--network bridge
```





## 三、[为 dockerd 设置网络代理](https://yeasy.gitbook.io/docker_practice/advanced_network/http_https_proxy)



"docker pull" 命令是由 dockerd 守护进程执行。而 dockerd 守护进程是由 systemd 管理。因此，如果需要在执行 "docker pull" 命令时使用 HTTP/HTTPS 代理，需要通过 systemd 配置。

- 

  为 dockerd 创建配置文件夹。

复制

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

- 

  为 dockerd 创建 HTTP/HTTPS 网络代理的配置文件，文件路径是 /etc/systemd/system/docker.service.d/http-proxy.conf 。并在该文件中添加相关环境变量。

复制

```
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

- 

  刷新配置并重启 docker 服务。

复制

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## docker部署实践

### 1、aria2

docker部署aria2-pro
https://blog.csdn.net/cjj2006/article/details/136459565

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -e PUID=$UID \
    -e PGID=$GID \
    -e UMASK_SET=022 \
    -e RPC_SECRET=feng \
    -e RPC_PORT=6800 \
    -p 6800:6800 \
    -e LISTEN_PORT=6888 \
    -p 6888:6888 \
    -p 6888:6888/udp \
    -v /home/feng/disk1/download/aria2/config:/config \
    -v /home/feng/disk1/download/aria2/downloads:/downloads \
    p3terx/aria2-pro
```


​	
部署webUI

	docker run -d \
	--name ariang \
	--log-opt max-size=1m \
	--restart unless-stopped \
	-p 6880:6880 \
	p3terx/arian



### 2、clash

```
sudo docker run -d --name clash \
	-v /path/to/your/config.yaml:/root/.config/clash/config.yaml 	\	
	-p 7890:7890 \	
	-p 7891:7891 \
	-p 9090:9090 \
	dreamacro/clash
```



```
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
```



3、

```
docker run -it -d --name kuiper \
-v /home/feng/infer-framwork/kuiperdatawhale:/home/codes/kuiperdatawhale \
registry.cn-hangzhou.aliyuncs.com/hellofss/kuiperinfer:datawhale
```







# 疑问解答

## 1、Flask 是什么，有什么用？

### Flask 是什么

**Flask 是一个使用 Python 编写的轻量级 Web 应用框**架。它基于 Werkzeug WSGI 工具包和 Jinja2 模板引擎。Flask 的设计目标是易于使用和快速开发，并且灵活性高，适用于从简单的单页面应用到复杂的大型项目。

### Flask 的作用

Flask 主要用于开发 Web 应用程序和 RESTful API。它的主要特点包括：

- **轻量级和微框架**：核心功能少，依赖少，但可以通过扩展实现各种功能。
- **灵活性高**：允许开发者自由选择使用的组件和方式。
- **易于学习和使用**：简单明了的 API 和文档，使得新手也能快速上手。
- **可扩展性强**：支持插件和中间件，可以方便地集成第三方库。

### Flask 示例

以下是一个简单的 Flask 应用示例：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```



## 2、在dockerfile中设置WORKDIR 的效果什么？

### Dockerfile 中的 WORKDIR

在 `Dockerfile` 中，`WORKDIR` 指令用于设置工作目录。`WORKDIR` 的作用是在镜像内创建一个目录（如果该目录不存在），并且将这个目录设置为所有后续指令（如 `RUN`、`CMD`、`ENTRYPOINT`、`COPY` 和 `ADD`）的工作目录。

### WORKDIR 的效果

1. **简化路径管理**：设置工作目录后，你无需在后续指令中使用绝对路径，直接使用相对路径即可。
2. **增强可读性**：明确指定工作目录，使 `Dockerfile` 更加易读和维护。
3. **创建目录**：如果指定的目录不存在，`WORKDIR` 会自动创建该目录。

### WORKDIR 示例

以下是一个带有 `WORKDIR` 指令的 `Dockerfile` 示例：

```dockerfile
# 使用官方的 Python 基础镜像
FROM python:3.9-slim

# 设置工作目录
WORKDIR /app

# 将 requirements.txt 复制到工作目录
COPY requirements.txt .

# 安装 Python 依赖包
RUN pip install -r requirements.txt

# 将应用代码复制到工作目录
COPY app .

# 暴露 Flask 默认端口
EXPOSE 5000

# 设置环境变量，告诉 Flask 在容器中运行
ENV FLASK_APP=app.py

# 启动 Flask 应用
CMD ["flask", "run", "--host=0.0.0.0"]
```

在这个示例中：

- `WORKDIR /app` 指定了 `/app` 作为工作目录。
- 后续的 `COPY` 和 `RUN` 指令会在 `/app` 目录下执行。例如，`COPY requirements.txt .` 会将文件复制到 `/app/requirements.txt`。

### 小结

- **Flask** 是一个轻量级的 Python Web 应用框架，适用于开发 Web 应用和 RESTful API。
- **WORKDIR** 指令在 `Dockerfile` 中用于设置工作目录，简化路径管理，增强可读性，并在必要时创建目录。

通过使用 `WORKDIR`，你可以使 `Dockerfile` 更加简洁和易读，避免在后续指令中频繁使用绝对路径。

