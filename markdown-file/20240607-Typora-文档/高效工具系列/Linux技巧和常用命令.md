[toc]

## linux最常用的命令

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



## 端口映射



## systemctl



## NAS

1、NAS系统折腾记 | 设置科学上网环境

https://blog.yanghong.dev/nas-clash-vpn/





参考链接：



