# Linux 常用命令速查手册

---

## 目录

1. [文件与目录操作](#1-文件与目录操作)
2. [文件查看与编辑](#2-文件查看与编辑)
3. [文件权限与所有权](#3-文件权限与所有权)
4. [用户与组管理](#4-用户与组管理)
5. [进程管理](#5-进程管理)
6. [网络相关](#6-网络相关)
7. [磁盘与存储](#7-磁盘与存储)
8. [压缩与解压](#8-压缩与解压)
9. [查找与搜索](#9-查找与搜索)
10. [系统信息](#10-系统信息)
11. [包管理](#11-包管理)
12. [Shell 快捷操作](#12-shell-快捷操作)
13. [常用组合技巧](#13-常用组合技巧)

---

## 1. 文件与目录操作

| 命令 | 说明 | 示例 |
|------|------|------|
| `ls` | 列出目录内容 | `ls -la`（显示隐藏文件+详细信息） |
| `cd` | 切换目录 | `cd /home/user`、`cd ..`、`cd ~` |
| `pwd` | 显示当前路径 | `pwd` |
| `mkdir` | 创建目录 | `mkdir -p a/b/c`（递归创建） |
| `rmdir` | 删除空目录 | `rmdir empty_dir` |
| `rm` | 删除文件/目录 | `rm -rf dir/`（强制递归删除，慎用！） |
| `cp` | 复制 | `cp -r src/ dest/`（递归复制目录） |
| `mv` | 移动/重命名 | `mv old.txt new.txt` |
| `touch` | 创建空文件/更新时间戳 | `touch file.txt` |
| `ln` | 创建链接 | `ln -s target link`（软链接） |

---

## 2. 文件查看与编辑

| 命令 | 说明 | 示例 |
|------|------|------|
| `cat` | 查看文件全部内容 | `cat file.txt` |
| `less` | 分页查看（可上下翻页） | `less large_file.log` |
| `more` | 分页查看（只能向下） | `more file.txt` |
| `head` | 查看前N行 | `head -n 20 file.txt` |
| `tail` | 查看后N行 | `tail -f log.txt`（实时跟踪） |
| `wc` | 统计行数/字数/字节 | `wc -l file.txt`（统计行数） |
| `sort` | 排序 | `sort -n numbers.txt`（数字排序） |
| `uniq` | 去重（需先排序） | `sort file | uniq -c`（计数） |
| `diff` | 比较文件差异 | `diff file1 file2` |
| `nano` | 简易文本编辑器 | `nano file.txt` |
| `vim` | 高级文本编辑器 | `vim file.txt` |

---

## 3. 文件权限与所有权

| 命令 | 说明 | 示例 |
|------|------|------|
| `chmod` | 修改权限 | `chmod 755 script.sh`、`chmod +x run.sh` |
| `chown` | 修改所有者 | `chown user:group file` |
| `chgrp` | 修改所属组 | `chgrp devs project/` |

### 权限数字速记

```
r=4  w=2  x=1
755 → rwxr-xr-x（所有者全权限，其他人可读可执行）
644 → rw-r--r--（所有者读写，其他人只读）
700 → rwx------（仅所有者全权限）
```

---

## 4. 用户与组管理

| 命令 | 说明 | 示例 |
|------|------|------|
| `whoami` | 当前用户名 | `whoami` |
| `id` | 显示用户ID和组信息 | `id username` |
| `useradd` | 添加用户 | `useradd -m newuser` |
| `userdel` | 删除用户 | `userdel -r olduser` |
| `passwd` | 修改密码 | `passwd username` |
| `su` | 切换用户 | `su - root` |
| `sudo` | 以管理员权限执行 | `sudo apt update` |
| `groups` | 查看用户所属组 | `groups username` |

---

## 5. 进程管理

| 命令 | 说明 | 示例 |
|------|------|------|
| `ps` | 查看进程 | `ps aux`（所有进程详情） |
| `top` | 实时进程监控 | `top`（按q退出） |
| `htop` | 增强版top | `htop` |
| `kill` | 终止进程 | `kill -9 PID`（强制终止） |
| `killall` | 按名称终止 | `killall nginx` |
| `bg` | 放到后台运行 | `bg %1` |
| `fg` | 放到前台运行 | `fg %1` |
| `jobs` | 查看后台任务 | `jobs -l` |
| `nohup` | 不挂断运行 | `nohup ./script.sh &` |
| `&` | 后台运行 | `command &` |

---

## 6. 网络相关

| 命令 | 说明 | 示例 |
|------|------|------|
| `ping` | 测试连通性 | `ping -c 4 google.com` |
| `curl` | HTTP请求工具 | `curl -X GET https://api.example.com` |
| `wget` | 下载文件 | `wget https://example.com/file.tar.gz` |
| `ifconfig` / `ip` | 查看网络接口 | `ip addr show` |
| `netstat` | 查看网络连接 | `netstat -tulnp`（监听端口） |
| `ss` | 现代版netstat | `ss -tulnp` |
| `scp` | 远程复制 | `scp file.txt user@host:/path/` |
| `ssh` | 远程登录 | `ssh user@192.168.1.100` |
| `rsync` | 高效同步 | `rsync -avz src/ user@host:dest/` |
| `traceroute` | 追踪路由 | `traceroute google.com` |
| `nslookup` / `dig` | DNS查询 | `dig example.com` |

---

## 7. 磁盘与存储

| 命令 | 说明 | 示例 |
|------|------|------|
| `df` | 查看磁盘使用 | `df -h`（人类可读格式） |
| `du` | 查看目录大小 | `du -sh *`（当前目录各项大小） |
| `mount` | 挂载设备 | `mount /dev/sdb1 /mnt/usb` |
| `umount` | 卸载设备 | `umount /mnt/usb` |
| `fdisk` | 磁盘分区 | `fdisk -l`（列出分区） |
| `lsblk` | 列出块设备 | `lsblk` |

---

## 8. 压缩与解压

| 命令 | 说明 | 示例 |
|------|------|------|
| `tar` | 打包/解包 | `tar -czf archive.tar.gz dir/`（压缩） |
| | | `tar -xzf archive.tar.gz`（解压） |
| `zip` | zip压缩 | `zip -r archive.zip dir/` |
| `unzip` | zip解压 | `unzip archive.zip` |
| `gzip` | gzip压缩 | `gzip file`（原文件被替换） |
| `gunzip` | gzip解压 | `gunzip file.gz` |

### tar 常用参数速记

```
-c 创建  -x 解压  -z gzip  -j bzip2  -f 指定文件名  -v 显示过程
```

---

## 9. 查找与搜索

| 命令 | 说明 | 示例 |
|------|------|------|
| `find` | 查找文件 | `find / -name "*.py" -type f` |
| `locate` | 快速查找（基于索引） | `locate config.ini` |
| `which` | 查找命令路径 | `which python` |
| `whereis` | 查找命令相关文件 | `whereis nginx` |
| `grep` | 文本搜索 | `grep -rn "error" logs/` |

### grep 常用参数

```bash
-r  递归搜索
-n  显示行号
-i  忽略大小写
-v  反向匹配（不包含的行）
-c  统计匹配行数
-l  只显示文件名
-E  使用扩展正则（等同 egrep）
```

---

## 10. 系统信息

| 命令 | 说明 | 示例 |
|------|------|------|
| `uname` | 系统信息 | `uname -a`（全部信息） |
| `hostname` | 主机名 | `hostname` |
| `uptime` | 运行时间与负载 | `uptime` |
| `free` | 内存使用 | `free -h` |
| `lscpu` | CPU信息 | `lscpu` |
| `date` | 日期时间 | `date "+%Y-%m-%d %H:%M:%S"` |
| `cal` | 日历 | `cal 2025` |
| `env` | 环境变量 | `env` |
| `export` | 设置环境变量 | `export PATH=$PATH:/new/path` |
| `history` | 命令历史 | `history | grep ssh` |
| `dmesg` | 内核日志 | `dmesg | tail` |

---

## 11. 包管理

### Debian/Ubuntu (apt)

```bash
sudo apt update            # 更新包索引
sudo apt upgrade           # 升级所有包
sudo apt install nginx     # 安装
sudo apt remove nginx      # 卸载
sudo apt search keyword    # 搜索
```

### CentOS/RHEL (yum/dnf)

```bash
sudo yum update            # 更新
sudo yum install nginx     # 安装
sudo yum remove nginx      # 卸载
sudo yum search keyword    # 搜索
```

### 通用 (pip - Python)

```bash
pip install package        # 安装
pip install -r requirements.txt  # 批量安装
pip list                   # 已安装列表
pip freeze > requirements.txt    # 导出依赖
```

---

## 12. Shell 快捷操作

| 快捷键/语法 | 说明 |
|-------------|------|
| `Ctrl + C` | 中断当前命令 |
| `Ctrl + Z` | 挂起当前命令 |
| `Ctrl + D` | 退出终端 |
| `Ctrl + R` | 搜索历史命令 |
| `Ctrl + L` | 清屏（等同 `clear`） |
| `Tab` | 自动补全 |
| `!!` | 执行上一条命令 |
| `!$` | 上条命令的最后参数 |
| `cmd1 && cmd2` | cmd1成功才执行cmd2 |
| `cmd1 \|\| cmd2` | cmd1失败才执行cmd2 |
| `cmd1 \| cmd2` | 管道，cmd1输出作为cmd2输入 |
| `>` | 重定向输出（覆盖） |
| `>>` | 重定向输出（追加） |
| `2>&1` | 标准错误合并到标准输出 |
| `< file` | 从文件读取输入 |

---

## 13. 常用组合技巧

```bash
# 查找并删除7天前的日志
find /var/log -name "*.log" -mtime +7 -delete

# 实时查看最新日志并高亮关键词
tail -f app.log | grep --color=auto "ERROR"

# 统计当前目录下代码行数
find . -name "*.py" | xargs wc -l

# 查看占用某端口的进程
lsof -i :8080
# 或
ss -tulnp | grep 8080

# 批量重命名（将 .txt 改为 .md）
for f in *.txt; do mv "$f" "${f%.txt}.md"; done

# 监控目录变化
watch -n 2 "ls -la /tmp/"

# 快速创建备份
cp config.yaml{,.bak}

# 按内存排序进程
ps aux --sort=-%mem | head -20

# 查看最大的10个文件
du -ah . | sort -rh | head -10

# SSH 端口转发（本地转发）
ssh -L 8080:localhost:3000 user@remote_host
```

---

## 速记卡片

| 场景 | 命令 |
|------|------|
| 我在哪？ | `pwd` |
| 这里有啥？ | `ls -la` |
| 文件多大？ | `du -sh file` |
| 磁盘满了？ | `df -h` |
| 谁在占端口？ | `ss -tulnp \| grep PORT` |
| 进程挂了？ | `ps aux \| grep name` → `kill PID` |
| 日志在刷啥？ | `tail -f /var/log/syslog` |
| 找个文件 | `find / -name "filename"` |
| 文件里搜内容 | `grep -rn "keyword" dir/` |
| 远程传文件 | `scp local user@host:remote` |

---

> **提示**：善用 `man command` 或 `command --help` 查看任何命令的详细用法！
