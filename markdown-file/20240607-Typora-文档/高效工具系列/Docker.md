[toc]





## 参考链接：



1、Docker Hub —— https://hub.docker.com/

2、**Docker — 从入门到实践** —— https://yeasy.gitbook.io/docker_practice





## docker常用命令

```bash
docker rmi ${images}
docker save -o ${my_images}.tar &{my_images}
docker load -i my-nginx-image.tar

```



## docker run的常用命令配置

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





## [为 dockerd 设置网络代理](https://yeasy.gitbook.io/docker_practice/advanced_network/http_https_proxy)



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

