[toc]

## 1、linux最常用的命令

Linux 是一个强大的操作系统，具有许多实用技巧可以提高你的生产力和操作效率。以下是一些常用的 Linux 使用技巧：

### 文件和目录操作

1. **快速创建文件和目录**
   - 创建文件：`touch filename`
   - 创建目录：`mkdir dirname`

2. **复制、移动和删除文件**
   - 复制文件：`cp source destination`
   - 移动文件：`mv source destination`
   - 删除文件：`rm filename`
   - 删除目录及其内容：`rm -r dirname`

3. **查看文件内容**
   - 查看文件前几行：`head filename`
   - 查看文件后几行：`tail filename`
   - 实时查看文件更新：`tail -f filename`
   - 查看文件内容：`cat filename` 或 `less filename`

### 系统信息

1. **查看系统信息**
   - 查看系统内存使用情况：`free -h`
   - 查看磁盘使用情况：`df -h`
   - 查看 CPU 信息：`lscpu`
   - 查看系统内核版本：`uname -r`
   - 查看系统发行版信息：`cat /etc/os-release`

2. **查看进程**
   - 列出当前运行的进程：`ps aux`
   - 通过名称过滤进程：`ps aux | grep process_name`
   - 交互式进程管理：`top` 或 `htop`（需要安装）

### 网络管理

1. **查看网络配置**
   - 查看 IP 地址：`ip addr` 或 `ifconfig`
   - 查看路由表：`ip route` 或 `route -n`

2. **网络诊断**
   - 测试网络连通性：`ping hostname`
   - 跟踪网络路径：`traceroute hostname`
   - 查看网络端口状态：`netstat -tuln` 或 `ss -tuln`

### 文件权限和所有权

1. **修改文件权限**
   - 查看文件权限：`ls -l filename`
   - 修改文件权限：`chmod 755 filename`（7=读写执行，5=读执行）

2. **修改文件所有权**
   - 修改文件所有者：`chown user:group filename`
   - 递归修改目录权限和所有权：`chmod -R 755 dirname` 和 `chown -R user:group dirname`

### 实用工具

1. **搜索文件**
   - 查找文件：`find /path -name filename`
   - 在文件内容中搜索：`grep 'pattern' filename` 或 `grep -r 'pattern' /path`

2. **归档和压缩**
   - 压缩文件：`tar -czvf archive.tar.gz files`
   - 解压文件：`tar -xzvf archive.tar.gz`

3. **查看日志**
   - 查看系统日志：`dmesg` 或 `journalctl`
   - **查看特定服务日志**：`journalctl -u service_name`

### 其他技巧

1. **命令别名**
   - 创建命令别名：`alias ll='ls -la'`
   - 移除命令别名：`unalias ll`
   - 永久保存别名：将别名添加到 `~/.bashrc` 或 `~/.zshrc`

2. **环境变量**
   - 查看环境变量：`echo $VARIABLE_NAME`
   - 设置环境变量：`export VARIABLE_NAME=value`
   - 永久保存环境变量：将其添加到 `~/.bashrc` 或 `~/.zshrc`

3. **任务计划**
   - 查看当前计划任务：`crontab -l`
   - 编辑计划任务：`crontab -e`
   - 计划任务格式：`* * * * * command`（分钟 小时 日期 月份 星期 命令）

这些技巧只是 Linux 使用的冰山一角，通过不断学习和实践，你会发现 Linux 是一个功能强大且灵活的操作系统，能极大地提高你的工作效率。



## 2、端口映射

在 Linux 上进行端口映射（也称为端口转发），通常涉及到将一个网络端口上的流量重定向到另一个端口或另一台机器上的端口。实现端口映射的方法有多种，常见的工具包括 `iptables` 和 `ssh` 等。以下是一些常见场景及其示例：

### 使用 `iptables` 进行端口映射

`iptables` 是 Linux 内核中集成的一个强大的防火墙工具。以下是几个使用 `iptables` 进行端口映射的示例。

#### 场景 1：将本地端口 8080 映射到本地端口 80

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-port 80
```

这条命令的作用是将所有到达本地 8080 端口的 TCP 流量重定向到本地 80 端口。

#### 场景 2：将本地端口 8080 映射到另一台机器上的端口 80

假设目标机器的 IP 地址是 192.168.1.100。

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80
sudo iptables -t nat -A POSTROUTING -p tcp -d 192.168.1.100 --dport 80 -j MASQUERADE
```

第一条命令将到达本地 8080 端口的 TCP 流量重定向到 192.168.1.100 的 80 端口，第二条命令确保返回的数据包正确路由回源地址。

### 使用 `ssh` 进行端口映射

`ssh` 可以用于本地端口转发、远程端口转发和动态端口转发。以下是几个使用 `ssh` 进行端口映射的示例。

#### 场景 3：本地端口转发

将本地 8080 端口的流量转发到远程服务器 (example.com) 的 80 端口。

```bash
ssh -L 8080:localhost:80 user@example.com
```

这条命令会创建一个 SSH 隧道，所有到本地 8080 端口的流量将通过 SSH 隧道转发到远程服务器的 80 端口。

#### 场景 4：远程端口转发

将远程服务器 (example.com) 的 8080 端口的流量转发到本地 80 端口。

```bash
ssh -R 8080:localhost:80 user@example.com
```

这条命令会创建一个 SSH 隧道，所有到远程服务器 8080 端口的流量将通过 SSH 隧道转发到本地的 80 端口。

#### 场景 5：动态端口转发

使用 `ssh` 创建一个 SOCKS 代理。

```bash
ssh -D 1080 user@example.com
```

这条命令会创建一个 SOCKS 代理，所有通过本地 1080 端口的流量将通过 SSH 隧道传输到远程服务器。

#### SSH 端口映射命令解释

##### 场景 3：本地端口转发

本地端口转发用于将本地计算机的某个端口上的流量通过 SSH 隧道转发到远程服务器的指定端口。常用于访问内网服务或绕过防火墙。

```bash
ssh -L [本地地址:]本地端口:远程地址:远程端口 [用户@]远程服务器
```

具体示例：
```bash
ssh -L 8080:localhost:80 user@example.com
```

解释：
- `-L`：指定本地端口转发。
- `8080`：本地计算机上的端口。
- `localhost`：流量转发的目标地址。在远程服务器上，`localhost` 指的是远程服务器本身。
- `80`：目标地址的端口，即远程服务器上的端口。
- `user@example.com`：SSH 登录所需的用户名和远程服务器地址。

这条命令的效果是：当访问本地计算机的 `localhost:8080` 时，流量会通过 SSH 隧道转发到 `example.com` 服务器，并在该服务器上访问 `localhost:80`。

##### 场景 4：远程端口转发

远程端口转发用于将远程服务器的某个端口上的流量通过 SSH 隧道转发到本地计算机的指定端口。常用于让外网设备访问内网服务。

```bash
ssh -R [远程地址:]远程端口:本地地址:本地端口 [用户@]远程服务器
```

具体示例：
```bash
ssh -R 8080:localhost:80 user@example.com
```

解释：
- `-R`：指定远程端口转发。
- `8080`：远程服务器上的端口。
- `localhost`：流量转发的目标地址。在本地计算机上，`localhost` 指的是本地计算机本身。
- `80`：目标地址的端口，即本地计算机上的端口。
- `user@example.com`：SSH 登录所需的用户名和远程服务器地址。

这条命令的效果是：当访问远程服务器的 `localhost:8080` 时，流量会通过 SSH 隧道转发到你的本地计算机，并在本地计算机上访问 `localhost:80`。

#### 更详细的示例解释

##### 本地端口转发示例详解

```bash
ssh -L 8080:localhost:80 user@example.com
```

1. **本地端口**：`8080`
   - 这是你在本地计算机上要监听的端口。任何发送到本地 `localhost:8080` 的流量都会通过 SSH 隧道。

2. **远程地址**：`localhost`
   - 这是远程服务器的地址。在这种情况下，它指的是 `example.com` 本身，因为我们通过 `user@example.com` 登录到远程服务器。

3. **远程端口**：`80`
   - 这是远程服务器上要访问的端口。也就是说，本地端口 `8080` 上的流量会被转发到远程服务器的 `localhost:80`。

4. **用户和远程服务器**：`user@example.com`
   - 这里指定了用于 SSH 连接的用户名和远程服务器地址。

   整条命令的作用是：当你在浏览器中访问 `http://localhost:8080` 时，实际上你访问的是远程服务器 `example.com` 上的 `localhost:80`。

##### 远程端口转发示例详解

```bash
ssh -R 8080:localhost:80 user@example.com
```

1. **远程端口**：`8080`
   - 这是远程服务器上要监听的端口。任何发送到远程 `localhost:8080` 的流量都会通过 SSH 隧道。

2. **本地地址**：`localhost`
   - 这是本地计算机的地址。在这种情况下，它指的是你当前操作的本地计算机。

3. **本地端口**：`80`
   - 这是本地计算机上要访问的端口。也就是说，远程端口 `8080` 上的流量会被转发到本地计算机的 `localhost:80`。

4. **用户和远程服务器**：`user@example.com`
   - 这里指定了用于 SSH 连接的用户名和远程服务器地址。

   整条命令的作用是：当远程服务器上的某个客户端访问 `http://localhost:8080` 时，实际上它访问的是你的本地计算机 `localhost:80`。

#### 图解流量转发过程

##### 本地端口转发

```text
+---------------+                     +------------------+
| Local Machine |                     | Remote Server    |
|               |                     |                  |
| localhost:8080| --> SSH Tunnel -->  | localhost:80     |
+---------------+                     +------------------+
```

#### 远程端口转发

```text
+---------------+                     +------------------+
| Local Machine |                     | Remote Server    |
|               |                     |                  |
| localhost:80  | <-- SSH Tunnel <--  | localhost:8080   |
+---------------+                     +------------------+
```

这些命令和示例展示了如何使用 SSH 隧道进行端口转发，从而在不同的场景下实现网络流量的重定向。

### 使用 `socat` 进行端口映射

`socat` 是一个通用的二进制数据传输工具，可以用于端口转发。以下是一个使用 `socat` 进行端口映射的示例。

#### 场景 6：将本地端口 8080 映射到远程服务器的端口 80

假设远程服务器的 IP 地址是 192.168.1.100。

```bash
socat TCP4-LISTEN:8080,fork TCP4:192.168.1.100:80
```

这条命令会监听本地的 8080 端口，并将所有接收到的流量转发到远程服务器 192.168.1.100 的 80 端口。

### 使用 `firewalld` 进行端口映射

`firewalld` 是 `iptables` 的前端，提供了一种更友好的方式来管理防火墙。以下是使用 `firewalld` 进行端口映射的示例。

#### 场景 7：将本地端口 8080 映射到本地端口 80

```bash
sudo firewall-cmd --add-forward-port=port=8080:proto=tcp:toport=80 --permanent
sudo firewall-cmd --reload
```

这条命令会将本地 8080 端口的 TCP 流量转发到本地的 80 端口，并且配置永久生效。

### 总结

不同工具适用于不同的场景和需求。`iptables` 和 `firewalld` 适合需要持久配置的场景，而 `ssh` 和 `socat` 更适合临时或特殊需求的场景。通过选择适合的工具和方法，你可以灵活地进行端口映射和流量管理。

## 3、systemctl

`systemctl` 是一个用于管理 `systemd` 系统和服务管理器的命令行工具。`systemd` 是现代 Linux 发行版中用来启动和管理系统服务的系统和服务管理器，取代了旧的 SysV 和 LSB init 脚本。

### `systemctl` 的常用命令

以下是一些常用的 `systemctl` 命令：

1. **启动服务**
   ```bash
   sudo systemctl start service_name
   ```
   
2. **停止服务**
   ```bash
   sudo systemctl stop service_name
   ```
   
3. **重启服务**
   ```bash
   sudo systemctl restart service_name
   ```
   
4. **重新加载服务配置**
   ```bash
   sudo systemctl reload service_name
   ```
   
5. **启用服务（开机自启动）**
   ```bash
   sudo systemctl enable service_name
   ```
   
6. **禁用服务（取消开机自启动）**
   ```bash
   sudo systemctl disable service_name
   ```
   
7. **查看服务状态**
   ```bash
   sudo systemctl status service_name
   ```
   
8. **查看所有服务状态**
   ```bash
   systemctl list-units --type=service
   ```
   
9. **查看服务日志**
   ```bash
   journalctl -u service_name
   ```

### 配置一个 `systemd` 服务

要配置一个新的 `systemd` 服务，你需要创建一个服务单元文件。这些文件通常位于 `/etc/systemd/system/` 或 `/lib/systemd/system/` 目录中，文件扩展名为 `.service`。

#### 示例：创建一个简单的服务

假设你有一个简单的脚本 `/usr/local/bin/my_script.sh`，希望将其作为服务运行。

1. **编写服务单元文件**

   创建文件 `/etc/systemd/system/my_service.service`，内容如下：
   ```ini
   [Unit]
   Description=My Custom Service
   After=network.target

   [Service]
   Type=simple
   ExecStart=/usr/local/bin/my_script.sh
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

   - **[Unit]** 部分描述服务及其依赖关系。
     - `Description`：对服务的简短描述。
     - `After`：定义服务启动的顺序，`network.target` 表示在网络服务启动后启动该服务。

   - **[Service]** 部分定义服务的行为。
     - `Type`：服务的类型，`simple` 表示 `ExecStart` 指定的命令将直接作为主要进程。
     - `ExecStart`：服务启动时执行的命令。
     - `Restart`：定义服务何时重启，这里设置为失败时重启。

   - **[Install]** 部分定义服务的安装信息。
     - `WantedBy`：指定服务应该在哪个目标（运行级别）下启用，`multi-user.target` 表示多用户模式（类似于旧的 runlevel 3）。

2. **重新加载 `systemd` 配置**

   在创建或修改了服务单元文件后，重新加载 `systemd` 配置：
   ```bash
   sudo systemctl daemon-reload
   ```

3. **启用并启动服务**

   启用服务，使其在系统启动时自动运行：
   ```bash
   sudo systemctl enable my_service
   ```

   启动服务：
   ```bash
   sudo systemctl start my_service
   ```

4. **查看服务状态**

   检查服务是否正在运行：
   ```bash
   sudo systemctl status my_service
   ```

### 完整示例

假设你要创建一个 Python 脚本作为服务运行。以下是完整的步骤：

1. **编写 Python 脚本**

   创建文件 `/usr/local/bin/my_script.py`：
   ```python
   #!/usr/bin/env python3
   import time

   while True:
       print("Service is running...")
       time.sleep(60)
   ```

   使脚本可执行：
   ```bash
   chmod +x /usr/local/bin/my_script.py
   ```

2. **创建服务单元文件**

   创建文件 `/etc/systemd/system/my_python_service.service`，内容如下：
   ```ini
   [Unit]
   Description=My Python Service
   After=network.target

   [Service]
   Type=simple
   ExecStart=/usr/bin/env python3 /usr/local/bin/my_script.py
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

3. **重新加载 `systemd` 配置并启动服务**

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable my_python_service
   sudo systemctl start my_python_service
   sudo systemctl status my_python_service
   ```

通过这些步骤，你可以创建并管理自定义的 `systemd` 服务，从而提高系统管理的效率和灵活性。

## NAS

1、NAS系统折腾记 | 设置科学上网环境

https://blog.yanghong.dev/nas-clash-vpn/





参考链接：



