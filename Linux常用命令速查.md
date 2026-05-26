# Linux 常用命令速查手册（完整版）

---

## 目录

1. [文件与目录操作](#1-文件与目录操作)
2. [文件查看与编辑](#2-文件查看与编辑)
3. [文本处理三剑客（grep/sed/awk）](#3-文本处理三剑客grepsedawk)
4. [文件权限与所有权](#4-文件权限与所有权)
5. [用户与组管理](#5-用户与组管理)
6. [进程管理](#6-进程管理)
7. [服务与定时任务](#7-服务与定时任务)
8. [网络相关](#8-网络相关)
9. [磁盘与存储](#9-磁盘与存储)
10. [压缩与解压](#10-压缩与解压)
11. [查找与搜索](#11-查找与搜索)
12. [系统信息](#12-系统信息)
13. [包管理](#13-包管理)
14. [防火墙与安全](#14-防火墙与安全)
15. [Shell 脚本基础](#15-shell-脚本基础)
16. [Shell 快捷操作](#16-shell-快捷操作)
17. [常用组合技巧](#17-常用组合技巧)
18. [速记卡片](#18-速记卡片)

---


## 1. 文件与目录操作

### `ls` — 列出目录内容（list）

```bash
ls              # 列出当前目录文件
ls -l           # 长格式显示（权限、大小、时间等）
ls -a           # 显示所有文件（包括以.开头的隐藏文件）
ls -la          # 长格式 + 隐藏文件（最常用组合）
ls -lh          # 长格式 + 人类可读大小（KB/MB/GB）
ls -lt          # 按修改时间排序（最新的在前）
ls -lS          # 按文件大小排序（最大的在前）
ls -R           # 递归列出子目录内容
ls -d */        # 只列出目录
ls -i           # 显示 inode 号
```

**选项含义：**
- `-l` (long) 长格式，显示详细信息
- `-a` (all) 显示所有文件，包括隐藏文件
- `-h` (human-readable) 文件大小用 KB/MB/GB 显示
- `-t` (time) 按修改时间排序
- `-S` (size) 按大小排序
- `-R` (recursive) 递归显示
- `-d` (directory) 只显示目录本身，不显示内容
- `-i` (inode) 显示文件的 inode 编号

---


### `cd` — 切换目录（change directory）

```bash
cd /home/user   # 切换到绝对路径
cd ..           # 返回上级目录
cd ../..        # 返回上两级
cd ~            # 回到家目录（等同 cd）
cd -            # 回到上一次所在的目录
cd /            # 回到根目录
```

---

### `pwd` — 显示当前工作目录（print working directory）

```bash
pwd             # 显示当前路径
pwd -P          # 显示真实物理路径（解析符号链接）
pwd -L          # 显示逻辑路径（含符号链接，默认）
```

---

### `mkdir` — 创建目录（make directory）

```bash
mkdir mydir             # 创建单个目录
mkdir dir1 dir2 dir3    # 同时创建多个目录
mkdir -p a/b/c/d        # 递归创建（父目录不存在时自动创建）
mkdir -m 755 mydir      # 创建时指定权限
mkdir -v mydir          # 显示创建过程
```

**选项含义：**
- `-p` (parents) 递归创建，父目录不存在也不报错
- `-m` (mode) 指定目录权限
- `-v` (verbose) 显示详细操作过程

---


### `rm` — 删除文件或目录（remove）

```bash
rm file.txt             # 删除文件
rm -i file.txt          # 删除前确认
rm -f file.txt          # 强制删除（不提示）
rm -r dir/              # 递归删除目录及其内容
rm -rf dir/             # 强制递归删除（⚠️ 极度危险！）
rm -v file.txt          # 显示删除过程
```

**选项含义：**
- `-r` (recursive) 递归删除目录和子目录
- `-f` (force) 强制删除，不提示确认
- `-i` (interactive) 每次删除前询问确认
- `-v` (verbose) 显示删除了哪些文件

> ⚠️ `rm -rf /` 会删除整个系统！永远不要执行！

---

### `cp` — 复制文件或目录（copy）

```bash
cp file1 file2          # 复制文件
cp file dir/            # 复制文件到目录
cp -r dir1/ dir2/       # 递归复制整个目录
cp -i file1 file2       # 覆盖前确认
cp -u src dst           # 仅当源文件更新时才复制
cp -p file1 file2       # 保留权限、时间戳等属性
cp -a dir1/ dir2/       # 归档复制（保留所有属性+递归）
cp -v file1 file2       # 显示复制过程
```

**选项含义：**
- `-r` (recursive) 递归复制目录
- `-i` (interactive) 覆盖前提示
- `-u` (update) 仅复制更新的文件
- `-p` (preserve) 保留文件属性（权限、时间戳）
- `-a` (archive) 等同 `-dpR`，归档模式，保留一切
- `-v` (verbose) 显示过程

---


### `mv` — 移动或重命名（move）

```bash
mv old.txt new.txt      # 重命名文件
mv file.txt dir/        # 移动文件到目录
mv dir1/ dir2/          # 移动/重命名目录
mv -i src dst           # 覆盖前确认
mv -u src dst           # 仅当源文件更新时才移动
mv -v src dst           # 显示移动过程
mv -n src dst           # 不覆盖已存在的文件
```

**选项含义：**
- `-i` (interactive) 覆盖前提示
- `-u` (update) 仅移动更新的文件
- `-v` (verbose) 显示过程
- `-n` (no-clobber) 不覆盖已有文件

---

### `touch` — 创建空文件 / 更新时间戳

```bash
touch file.txt          # 文件不存在则创建，存在则更新时间戳
touch -t 202501011200 file  # 指定时间戳（YYYYMMDDhhmm）
touch -c file.txt       # 不创建文件，仅更新已有文件时间
touch -a file.txt       # 仅更新访问时间
touch -m file.txt       # 仅更新修改时间
```

**选项含义：**
- `-t` (time) 使用指定时间而非当前时间
- `-c` (no-create) 不创建新文件
- `-a` (access) 仅改变访问时间
- `-m` (modify) 仅改变修改时间

---

### `ln` — 创建链接（link）

```bash
ln file hardlink        # 创建硬链接（共享 inode）
ln -s target symlink    # 创建软链接/符号链接（类似快捷方式）
ln -sf target symlink   # 强制覆盖已存在的链接
ln -v target link       # 显示创建过程
```

**选项含义：**
- `-s` (symbolic) 创建符号链接（软链接）
- `-f` (force) 如果链接已存在，强制覆盖
- `-v` (verbose) 显示过程

**硬链接 vs 软链接：**
- 硬链接：指向相同 inode，删除原文件不影响，不能跨文件系统
- 软链接：指向文件路径，类似快捷方式，可以跨文件系统

---

### `rmdir` — 删除空目录（remove directory）

```bash
rmdir empty_dir         # 删除空目录（非空会报错）
rmdir -p a/b/c          # 递归删除空的父目录
rmdir -v dir            # 显示过程
```

**选项含义：**
- `-p` (parents) 递归删除路径中的空目录
- `-v` (verbose) 显示过程

---


## 2. 文件查看与编辑

### `cat` — 查看/连接文件（concatenate）

```bash
cat file.txt            # 显示文件全部内容
cat -n file.txt         # 显示行号
cat -b file.txt         # 只对非空行编号
cat -s file.txt         # 压缩连续空行为一行
cat file1 file2 > merged.txt  # 合并多个文件
cat -A file.txt         # 显示不可见字符（如Tab显示为^I）
```

**选项含义：**
- `-n` (number) 对所有行编号
- `-b` (number-nonblank) 仅对非空行编号
- `-s` (squeeze-blank) 多个连续空行合并为一个
- `-A` (show-all) 显示所有不可见字符

---

### `less` — 分页查看（可前后翻页）

```bash
less file.txt           # 分页查看
less -N file.txt        # 显示行号
less -S file.txt        # 不换行（长行截断显示）
less +F file.txt        # 类似 tail -f，实时跟踪
```

**less 内部操作：**
- `空格/f` 向下翻页
- `b` 向上翻页
- `g` 跳到文件开头
- `G` 跳到文件末尾
- `/keyword` 向下搜索
- `?keyword` 向上搜索
- `n` 下一个匹配
- `N` 上一个匹配
- `q` 退出

---

### `head` — 查看文件开头

```bash
head file.txt           # 默认显示前10行
head -n 20 file.txt     # 显示前20行
head -n -5 file.txt     # 显示除了最后5行的所有内容
head -c 100 file.txt    # 显示前100个字节
```

**选项含义：**
- `-n` (lines) 指定显示行数
- `-c` (bytes) 指定显示字节数

---

### `tail` — 查看文件末尾

```bash
tail file.txt           # 默认显示最后10行
tail -n 20 file.txt     # 显示最后20行
tail -n +5 file.txt     # 从第5行开始显示到末尾
tail -f log.txt         # 实时跟踪文件新增内容（常用于看日志）
tail -F log.txt         # 跟踪 + 文件被重建时重新打开
tail -c 200 file.txt    # 显示最后200字节
```

**选项含义：**
- `-n` (lines) 指定行数
- `-f` (follow) 持续跟踪文件末尾（文件增长时实时输出）
- `-F` (follow + retry) 同 `-f`，但文件被删除重建后会重新打开
- `-c` (bytes) 指定字节数

---


### `wc` — 统计（word count）

```bash
wc file.txt             # 显示 行数 词数 字节数
wc -l file.txt          # 仅统计行数
wc -w file.txt          # 仅统计词数
wc -c file.txt          # 仅统计字节数
wc -m file.txt          # 统计字符数
wc -L file.txt          # 最长行的长度
```

**选项含义：**
- `-l` (lines) 行数
- `-w` (words) 词数
- `-c` (bytes) 字节数
- `-m` (chars) 字符数
- `-L` (max-line-length) 最长行长度

---

### `sort` — 排序

```bash
sort file.txt           # 按字母顺序排序
sort -n file.txt        # 按数字大小排序
sort -r file.txt        # 逆序排序
sort -k 2 file.txt      # 按第2列排序
sort -t ":" -k 3 -n /etc/passwd  # 指定分隔符，按第3列数字排序
sort -u file.txt        # 排序并去重
sort -h file.txt        # 按人类可读数字排序（1K < 1M < 1G）
```

**选项含义：**
- `-n` (numeric) 按数字排序
- `-r` (reverse) 逆序
- `-k` (key) 按指定列排序
- `-t` (field-separator) 指定列分隔符
- `-u` (unique) 去除重复行
- `-h` (human-numeric) 按人类可读数字排（如 2K, 1G）

---

### `uniq` — 去重（需先排序）

```bash
sort file | uniq         # 去除相邻重复行
sort file | uniq -c      # 统计每行重复次数
sort file | uniq -d      # 仅显示重复的行
sort file | uniq -u      # 仅显示不重复的行
sort file | uniq -i      # 忽略大小写
```

**选项含义：**
- `-c` (count) 前面显示重复次数
- `-d` (repeated) 只输出重复行
- `-u` (unique) 只输出不重复行
- `-i` (ignore-case) 忽略大小写

---

### `diff` — 比较文件差异

```bash
diff file1 file2        # 比较两个文件
diff -u file1 file2     # 统一格式输出（最常用，类似git diff）
diff -y file1 file2     # 并排显示
diff -r dir1/ dir2/     # 递归比较两个目录
diff -q file1 file2     # 只报告是否不同，不显示细节
diff --color file1 file2  # 彩色输出
```

**选项含义：**
- `-u` (unified) 统一格式，带上下文
- `-y` (side-by-side) 并排对比
- `-r` (recursive) 递归比较目录
- `-q` (brief) 只报告是否有差异
- `--color` 彩色高亮差异

---

### `tee` — 同时输出到屏幕和文件

```bash
command | tee output.txt        # 输出到屏幕同时写入文件
command | tee -a output.txt     # 追加模式（不覆盖）
command | tee file1 file2       # 同时写入多个文件
```

**选项含义：**
- `-a` (append) 追加到文件末尾而非覆盖

---

### `tr` — 字符转换/删除（translate）

```bash
echo "hello" | tr 'a-z' 'A-Z'   # 小写转大写
echo "hello" | tr -d 'l'         # 删除指定字符
echo "hello   world" | tr -s ' ' # 压缩重复字符
cat file | tr '\t' ' '           # Tab 替换为空格
```

**选项含义：**
- `-d` (delete) 删除指定字符
- `-s` (squeeze) 压缩重复字符为一个

---

### `cut` — 按列提取文本

```bash
cut -d ":" -f 1 /etc/passwd     # 以:为分隔符，提取第1列
cut -d "," -f 1,3 data.csv      # 提取第1和第3列
cut -c 1-10 file.txt            # 提取每行前10个字符
cut -f 2 file.tsv               # 默认Tab分隔，取第2列
```

**选项含义：**
- `-d` (delimiter) 指定分隔符
- `-f` (fields) 指定提取哪些列
- `-c` (characters) 按字符位置提取

---


## 3. 文本处理三剑客（grep/sed/awk）

### `grep` — 文本搜索（Global Regular Expression Print）

```bash
grep "error" file.txt           # 搜索含 error 的行
grep -i "error" file.txt        # 忽略大小写
grep -r "TODO" src/             # 递归搜索目录
grep -n "error" file.txt        # 显示行号
grep -c "error" file.txt        # 统计匹配行数
grep -l "error" *.log           # 只显示包含匹配的文件名
grep -v "debug" file.txt        # 反向匹配（不含debug的行）
grep -w "is" file.txt           # 全词匹配（不匹配this）
grep -A 3 "error" file.txt      # 显示匹配行及其后3行
grep -B 2 "error" file.txt      # 显示匹配行及其前2行
grep -C 2 "error" file.txt      # 显示匹配行及前后各2行
grep -E "err|warn" file.txt     # 扩展正则（多个模式）
grep -P "\d{3}" file.txt        # Perl正则
grep --color=auto "key" file    # 高亮匹配内容
grep -o "pattern" file.txt      # 只输出匹配的部分
```

**选项含义：**
- `-i` (ignore-case) 忽略大小写
- `-r` (recursive) 递归搜索目录
- `-n` (line-number) 显示行号
- `-c` (count) 统计匹配行数
- `-l` (files-with-matches) 只列出文件名
- `-v` (invert-match) 反向匹配
- `-w` (word-regexp) 全词匹配
- `-A` (after-context) 显示匹配后N行
- `-B` (before-context) 显示匹配前N行
- `-C` (context) 显示前后各N行
- `-E` (extended-regexp) 扩展正则（等同 `egrep`）
- `-P` (perl-regexp) Perl 兼容正则
- `-o` (only-matching) 只输出匹配到的部分
- `--color` 彩色高亮

---

### `sed` — 流编辑器（Stream Editor）

```bash
# 替换（s = substitute）
sed 's/old/new/' file           # 每行替换第一个匹配
sed 's/old/new/g' file          # 全局替换（g = global）
sed 's/old/new/gi' file         # 全局替换 + 忽略大小写
sed -i 's/old/new/g' file       # 直接修改原文件（⚠️）
sed -i.bak 's/old/new/g' file   # 修改前备份为 file.bak

# 删除（d = delete）
sed '3d' file                   # 删除第3行
sed '2,5d' file                 # 删除第2到第5行
sed '/pattern/d' file           # 删除匹配行
sed '/^$/d' file                # 删除空行
sed '/^#/d' file                # 删除注释行（#开头）

# 打印（p = print）
sed -n '5p' file                # 只打印第5行
sed -n '2,5p' file              # 打印第2到5行
sed -n '/error/p' file          # 打印匹配行

# 插入/追加
sed '3a\new line' file          # 第3行后追加（a = append）
sed '3i\new line' file          # 第3行前插入（i = insert）

# 多条命令
sed -e 's/a/b/' -e 's/c/d/' file  # 执行多个操作
```

**选项含义：**
- `-i` (in-place) 直接修改文件
- `-n` (quiet) 静默模式，不自动打印，配合 `p` 使用
- `-e` (expression) 指定多个编辑命令
- `-E` (extended-regexp) 使用扩展正则

---


### `awk` — 强大的文本处理工具

```bash
# 基本格式：awk '条件 {动作}' 文件
awk '{print $1}' file           # 打印每行第1列（默认空格分隔）
awk '{print $1, $3}' file       # 打印第1和第3列
awk -F ":" '{print $1}' /etc/passwd   # 指定:为分隔符
awk '{print NR, $0}' file       # NR=行号，$0=整行
awk 'NR==5' file                # 打印第5行
awk 'NR>=2 && NR<=5' file       # 打印第2到5行
awk '/error/' file              # 打印含error的行
awk '$3 > 100' file             # 第3列大于100的行
awk '{sum += $1} END {print sum}' file  # 求第1列的和
awk 'BEGIN{FS=":"} {print $1}' /etc/passwd  # BEGIN中设置分隔符
awk '{print length, $0}' file | sort -n     # 按行长排序
awk '!seen[$0]++' file          # 去重（不需要先排序）
awk -F "," '{print $1","$3}' data.csv       # 处理CSV
```

**常用内置变量：**
- `$0` 整行内容
- `$1, $2...` 第1列、第2列...
- `NR` (Number of Records) 当前行号
- `NF` (Number of Fields) 当前行的列数
- `FS` (Field Separator) 输入字段分隔符
- `OFS` (Output Field Separator) 输出字段分隔符
- `RS` (Record Separator) 记录分隔符（默认换行）

**选项含义：**
- `-F` (field-separator) 指定输入字段分隔符
- `-v` (variable) 定义变量，如 `awk -v n=5 'NR==n'`

---

### `xargs` — 将输入转为命令参数

```bash
find . -name "*.tmp" | xargs rm          # 删除所有.tmp文件
echo "a b c" | xargs -n 1               # 每次传1个参数（每行一个）
find . -name "*.py" | xargs grep "import"  # 在找到的文件中搜索
cat urls.txt | xargs -I {} curl {}       # 用{}作为占位符
find . -name "*.log" | xargs -P 4 gzip  # 并行4个进程压缩
echo "1 2 3" | xargs -n 1 -I {} echo "num: {}"
```

**选项含义：**
- `-n` (max-args) 每次传递的最大参数数量
- `-I {}` (replace) 用 `{}` 替代输入内容
- `-P` (max-procs) 最大并行进程数
- `-d` (delimiter) 指定分隔符
- `-0` (null) 以 null 字符分隔（配合 `find -print0`）
- `-t` (verbose) 显示执行的命令

---


## 4. 文件权限与所有权

### `chmod` — 修改权限（change mode）

```bash
chmod 755 script.sh     # 数字模式：rwxr-xr-x
chmod 644 file.txt      # rw-r--r--
chmod 700 private/      # rwx------
chmod +x script.sh      # 给所有人添加执行权限
chmod u+x script.sh     # 仅给所有者添加执行权限
chmod g-w file.txt      # 移除组的写权限
chmod o=r file.txt      # 设置其他人为只读
chmod -R 755 dir/       # 递归修改目录内所有文件权限
chmod a+r file.txt      # 给所有人添加读权限
```

**权限数字速记：**
```
r(读)=4  w(写)=2  x(执行)=1
第1位=所有者  第2位=组  第3位=其他人

755 → rwxr-xr-x  所有者全权限，其他人可读可执行
644 → rw-r--r--  所有者读写，其他人只读
700 → rwx------  仅所有者全权限
600 → rw-------  仅所有者读写
777 → rwxrwxrwx  所有人全权限（⚠️ 通常不安全）
```

**字母模式：**
- `u` (user) 所有者
- `g` (group) 所属组
- `o` (others) 其他人
- `a` (all) 所有人
- `+` 添加权限，`-` 移除权限，`=` 设置权限

**选项含义：**
- `-R` (recursive) 递归修改
- `-v` (verbose) 显示过程

---

### `chown` — 修改所有者（change owner）

```bash
chown user file.txt         # 修改所有者
chown user:group file.txt   # 同时修改所有者和组
chown :group file.txt       # 只修改组
chown -R user:group dir/    # 递归修改
chown --reference=ref file  # 参照另一个文件的所有权
```

**选项含义：**
- `-R` (recursive) 递归修改
- `--reference` 参照指定文件设置

---

### `chgrp` — 修改所属组（change group）

```bash
chgrp group file.txt        # 修改文件所属组
chgrp -R group dir/         # 递归修改
```

---

### `umask` — 默认权限掩码

```bash
umask               # 查看当前掩码
umask 022           # 设置掩码（新文件默认权限 = 666 - 022 = 644）
umask 077           # 更严格（新文件 600，新目录 700）
```

> 文件默认最大权限 666，目录默认最大权限 777，减去 umask 值就是实际权限。

---


## 5. 用户与组管理

### `whoami` — 显示当前登录用户名

```bash
whoami              # 输出当前用户名
```

### `id` — 显示用户ID和组信息

```bash
id                  # 当前用户的 UID、GID 和所属组
id username         # 查看指定用户信息
id -u               # 只显示 UID
id -g               # 只显示 GID
id -G               # 显示所有所属组的 GID
id -nG              # 显示所有所属组名称
```

### `useradd` — 添加用户

```bash
useradd newuser             # 创建用户（不自动创建家目录）
useradd -m newuser          # 创建用户并自动创建家目录
useradd -m -s /bin/bash user  # 指定shell
useradd -m -G sudo,docker user  # 创建并加入多个组
useradd -e 2025-12-31 user  # 设置账户过期时间
```

**选项含义：**
- `-m` (create-home) 自动创建家目录
- `-s` (shell) 指定登录 shell
- `-G` (groups) 指定附加组（多个用逗号分隔）
- `-g` (gid) 指定主组
- `-e` (expiredate) 账户过期日期
- `-d` (home-dir) 指定家目录路径

### `usermod` — 修改已有用户

```bash
usermod -aG docker user     # 将用户添加到 docker 组
usermod -s /bin/zsh user    # 修改用户shell
usermod -L user             # 锁定用户（禁止登录）
usermod -U user             # 解锁用户
usermod -l newname oldname  # 重命名用户
```

**选项含义：**
- `-aG` (append Groups) 追加到附加组（不加-a会覆盖！）
- `-s` (shell) 修改 shell
- `-L` (lock) 锁定账户
- `-U` (unlock) 解锁账户
- `-l` (login) 更改用户名

### `userdel` — 删除用户

```bash
userdel user            # 删除用户（保留家目录）
userdel -r user         # 删除用户并删除家目录
```

**选项含义：**
- `-r` (remove) 同时删除家目录和邮件

### `passwd` — 修改密码

```bash
passwd              # 修改当前用户密码
passwd username     # 修改指定用户密码（需root）
passwd -l user      # 锁定用户密码
passwd -u user      # 解锁用户密码
passwd -d user      # 删除密码（允许无密码登录）
passwd -e user      # 强制用户下次登录时改密码
```

### `su` — 切换用户（switch user）

```bash
su root             # 切换到root（不切换环境变量）
su - root           # 切换到root（完整切换环境）
su - user           # 切换到指定用户
su -c "command" user  # 以其他用户执行单条命令
```

**选项含义：**
- `-` / `-l` (login) 模拟完整登录（加载用户环境变量）
- `-c` (command) 执行单个命令后返回

### `sudo` — 以其他用户权限执行（super user do）

```bash
sudo command            # 以root身份执行命令
sudo -u user command    # 以指定用户身份执行
sudo -i                 # 切换到root shell
sudo -s                 # 以root打开shell（不切换环境）
sudo -l                 # 列出当前用户可用的sudo权限
sudo !!                 # 用sudo重新执行上一条命令
```

**选项含义：**
- `-u` (user) 指定以哪个用户执行
- `-i` (login) 模拟root登录
- `-s` (shell) 以root开启shell
- `-l` (list) 列出允许的命令

### `groups` — 查看所属组

```bash
groups              # 当前用户所属组
groups username     # 指定用户所属组
```

### `groupadd` / `groupdel` — 组管理

```bash
groupadd developers     # 创建组
groupdel developers     # 删除组
```

---


## 6. 进程管理

### `ps` — 查看进程快照（process status）

```bash
ps                  # 当前终端的进程
ps aux              # 所有进程详细信息（最常用）
ps -ef              # 所有进程（另一种格式）
ps aux | grep nginx # 查找nginx进程
ps -u username      # 查看指定用户的进程
ps --sort=-%mem     # 按内存使用排序
ps --sort=-%cpu     # 按CPU使用排序
ps -p PID           # 查看指定PID的进程
ps axjf             # 树状显示进程关系
```

**选项含义：**
- `a` (all) 显示所有用户的进程
- `u` (user-oriented) 以用户为主的格式显示
- `x` 包括没有终端的进程
- `-e` (every) 所有进程
- `-f` (full) 完整格式
- `--sort` 指定排序字段

### `top` — 实时进程监控

```bash
top                 # 启动实时监控
top -d 2            # 每2秒刷新
top -p PID          # 只监控指定进程
top -u username     # 只显示指定用户进程
top -n 5            # 刷新5次后退出
```

**top 内部操作：**
- `q` 退出
- `M` 按内存排序
- `P` 按CPU排序
- `T` 按运行时间排序
- `k` 终止进程（输入PID）
- `1` 显示每个CPU核心
- `h` 帮助

**选项含义：**
- `-d` (delay) 刷新间隔秒数
- `-p` (pid) 监控指定PID
- `-u` (user) 过滤用户
- `-n` (number) 刷新次数

### `kill` — 发送信号终止进程

```bash
kill PID            # 发送TERM信号（优雅终止，默认信号15）
kill -9 PID         # 发送KILL信号（强制终止）
kill -15 PID        # 发送TERM信号（等同默认）
kill -HUP PID       # 发送HUP信号（让进程重新加载配置）
kill -STOP PID      # 暂停进程
kill -CONT PID      # 恢复暂停的进程
kill -l             # 列出所有信号
```

**常用信号：**
- `1` (HUP) 挂起，通常用于重新加载配置
- `2` (INT) 中断，等同 Ctrl+C
- `9` (KILL) 强制终止，不可被捕获
- `15` (TERM) 正常终止，可被捕获处理
- `18` (CONT) 继续运行
- `19` (STOP) 暂停进程

### `killall` — 按名称终止进程

```bash
killall nginx       # 终止所有名为nginx的进程
killall -9 python   # 强制终止所有python进程
killall -u user     # 终止某用户的所有进程
killall -i nginx    # 交互式确认
```

### `pkill` — 按模式终止进程

```bash
pkill -f "python app.py"    # 按完整命令行匹配终止
pkill -u user               # 终止用户所有进程
pkill -signal nginx         # 发送指定信号
```

**选项含义：**
- `-f` (full) 匹配完整命令行
- `-u` (user) 按用户匹配

### `pgrep` — 按模式查找进程PID

```bash
pgrep nginx         # 查找nginx的PID
pgrep -l nginx      # 显示PID和进程名
pgrep -f "python"   # 按完整命令行匹配
pgrep -u root       # 查找root用户的进程
```

### 后台任务管理

```bash
command &           # 在后台运行命令
jobs                # 列出当前终端的后台任务
jobs -l             # 列出任务并显示PID
fg %1               # 将任务1放到前台
bg %1               # 将任务1放到后台继续运行
nohup cmd &         # 忽略挂断信号，关闭终端不影响
disown %1           # 将任务从shell的作业列表中移除
```

### `lsof` — 列出打开的文件（list open files）

```bash
lsof                    # 列出所有打开的文件
lsof -i :8080           # 查看占用8080端口的进程
lsof -u username        # 查看用户打开的文件
lsof -p PID             # 查看进程打开的文件
lsof +D /var/log/       # 查看目录下被打开的文件
lsof -i tcp             # 查看所有TCP连接
```

**选项含义：**
- `-i` (internet) 网络连接
- `-u` (user) 指定用户
- `-p` (pid) 指定进程
- `+D` (directory) 指定目录

---


## 7. 服务与定时任务

### `systemctl` — 系统服务管理（systemd）

```bash
systemctl start nginx       # 启动服务
systemctl stop nginx        # 停止服务
systemctl restart nginx     # 重启服务
systemctl reload nginx      # 重新加载配置（不中断服务）
systemctl status nginx      # 查看服务状态
systemctl enable nginx      # 设置开机自启
systemctl disable nginx     # 取消开机自启
systemctl is-active nginx   # 检查是否运行中
systemctl is-enabled nginx  # 检查是否开机自启
systemctl list-units --type=service  # 列出所有服务
systemctl list-units --failed       # 列出失败的服务
systemctl daemon-reload     # 重新加载 systemd 配置
```

### `journalctl` — 查看系统日志（systemd 日志）

```bash
journalctl                      # 查看所有日志
journalctl -u nginx             # 查看nginx服务日志
journalctl -u nginx --since today  # 今天的nginx日志
journalctl -f                   # 实时跟踪日志
journalctl -n 50                # 最近50条
journalctl --since "2025-01-01" --until "2025-01-02"  # 时间范围
journalctl -p err               # 只看错误级别
journalctl --disk-usage         # 日志占用磁盘空间
```

**选项含义：**
- `-u` (unit) 指定服务单元
- `-f` (follow) 实时跟踪
- `-n` (lines) 显示行数
- `-p` (priority) 日志级别（emerg/alert/crit/err/warning/notice/info/debug）
- `--since` / `--until` 时间范围

### `crontab` — 定时任务

```bash
crontab -l              # 列出当前用户的定时任务
crontab -e              # 编辑定时任务
crontab -r              # 删除所有定时任务
crontab -u user -l      # 查看指定用户的任务
```

**cron 时间格式：**
```
分  时  日  月  周  命令
*   *   *   *   *   command

# 示例
*/5 * * * * /path/script.sh      # 每5分钟执行
0 2 * * * /path/backup.sh        # 每天凌晨2点
0 0 * * 0 /path/weekly.sh        # 每周日零点
0 9 1 * * /path/monthly.sh       # 每月1号9点
30 8 * * 1-5 /path/workday.sh    # 工作日8:30
```

**时间字段：**
- `*` 任意值
- `,` 多个值（如 `1,3,5`）
- `-` 范围（如 `1-5`）
- `/` 步进（如 `*/10` 每10单位）

---


## 8. 网络相关

### `ping` — 测试网络连通性

```bash
ping google.com         # 持续ping（Ctrl+C停止）
ping -c 4 google.com    # 发送4个包后停止
ping -i 2 host          # 每2秒发送一次
ping -s 1024 host       # 指定包大小（字节）
ping -W 3 host          # 超时时间3秒
```

**选项含义：**
- `-c` (count) 发送包数量
- `-i` (interval) 发送间隔秒数
- `-s` (size) 数据包大小
- `-W` (timeout) 等待响应超时时间
- `-q` (quiet) 静默模式，只显示统计

### `curl` — URL数据传输工具

```bash
curl https://example.com            # GET请求
curl -o file.html url               # 下载保存为指定文件名
curl -O url                         # 保存为URL中的文件名
curl -X POST url                    # 指定请求方法
curl -d "key=value" url             # POST发送数据
curl -H "Content-Type: application/json" url  # 设置请求头
curl -u user:pass url               # HTTP基本认证
curl -I url                         # 只获取响应头
curl -L url                         # 跟随重定向
curl -s url                         # 静默模式（不显示进度）
curl -k url                         # 忽略SSL证书验证
curl -w "%{http_code}" url          # 显示HTTP状态码
curl --connect-timeout 5 url        # 连接超时5秒
curl -x proxy:port url              # 使用代理
```

**选项含义：**
- `-o` (output) 指定输出文件名
- `-O` (remote-name) 用远程文件名保存
- `-X` (request) 指定HTTP方法
- `-d` (data) POST数据
- `-H` (header) 添加请求头
- `-u` (user) 用户认证
- `-I` (head) 只获取头信息
- `-L` (location) 跟随重定向
- `-s` (silent) 静默
- `-k` (insecure) 忽略SSL
- `-w` (write-out) 自定义输出格式
- `-x` (proxy) 设置代理

### `wget` — 文件下载工具

```bash
wget url                        # 下载文件
wget -O name.zip url            # 指定保存文件名
wget -c url                     # 断点续传
wget -q url                     # 静默下载
wget -b url                     # 后台下载
wget -r url                     # 递归下载整个网站
wget --limit-rate=1m url        # 限速1MB/s
wget -P /path/ url              # 指定保存目录
wget -i urls.txt                # 从文件读取URL列表批量下载
```

**选项含义：**
- `-O` (output-document) 指定文件名
- `-c` (continue) 断点续传
- `-q` (quiet) 静默
- `-b` (background) 后台运行
- `-r` (recursive) 递归下载
- `-P` (directory-prefix) 保存目录
- `-i` (input-file) 从文件读取URL
- `--limit-rate` 限制下载速度

### `ip` — 网络配置工具（替代 ifconfig）

```bash
ip addr show                # 显示所有网络接口信息（简写 ip a）
ip addr show eth0           # 查看指定接口
ip link show                # 查看链路层信息
ip link set eth0 up         # 启用网卡
ip link set eth0 down       # 禁用网卡
ip route show               # 查看路由表（简写 ip r）
ip route add default via 192.168.1.1  # 添加默认网关
ip neigh show               # 查看ARP表（简写 ip n）
```

### `ss` — 查看网络连接（替代 netstat）

```bash
ss -t               # TCP连接
ss -u               # UDP连接
ss -l               # 监听中的端口
ss -n               # 数字格式（不解析域名）
ss -p               # 显示进程信息
ss -tulnp           # 最常用组合：TCP+UDP+监听+数字+进程
ss -s               # 统计摘要
ss state established # 只看已建立的连接
ss dst 192.168.1.1  # 过滤目标地址
```

**选项含义：**
- `-t` (tcp) TCP连接
- `-u` (udp) UDP连接
- `-l` (listening) 监听中的
- `-n` (numeric) 不解析主机名
- `-p` (processes) 显示进程
- `-s` (summary) 摘要统计
- `-a` (all) 所有连接

### `netstat` — 网络统计（较旧，可用ss替代）

```bash
netstat -tulnp      # 查看监听端口及进程
netstat -an         # 所有连接（数字格式）
netstat -r          # 路由表
netstat -i          # 网络接口统计
netstat -s          # 协议统计
```

### `scp` — 安全远程复制（secure copy）

```bash
scp file.txt user@host:/path/       # 上传文件
scp user@host:/path/file.txt .      # 下载文件
scp -r dir/ user@host:/path/        # 递归复制目录
scp -P 2222 file user@host:/path/   # 指定端口
scp -C file user@host:/path/        # 压缩传输
```

**选项含义：**
- `-r` (recursive) 递归复制目录
- `-P` (port) 指定SSH端口
- `-C` (compress) 传输时压缩
- `-i` (identity) 指定私钥文件

### `ssh` — 远程登录

```bash
ssh user@host               # 登录远程主机
ssh -p 2222 user@host       # 指定端口
ssh -i ~/.ssh/key user@host # 使用指定私钥
ssh user@host "command"     # 远程执行命令
ssh -L 8080:localhost:3000 user@host  # 本地端口转发
ssh -R 9090:localhost:8080 user@host  # 远程端口转发
ssh -D 1080 user@host      # SOCKS代理
ssh -N -f -L 8080:db:3306 user@host  # 后台端口转发
```

**选项含义：**
- `-p` (port) 指定端口
- `-i` (identity) 私钥文件
- `-L` (local) 本地端口转发
- `-R` (remote) 远程端口转发
- `-D` (dynamic) 动态端口转发（SOCKS代理）
- `-N` (no command) 不执行远程命令
- `-f` (background) 后台运行

### `rsync` — 高效文件同步

```bash
rsync -av src/ dest/                    # 本地同步
rsync -avz src/ user@host:dest/         # 远程同步（压缩传输）
rsync -avz --delete src/ dest/          # 同步并删除目标多余文件
rsync -avz --exclude="*.log" src/ dest/ # 排除文件
rsync -avzP src/ user@host:dest/        # 显示进度+支持断点续传
rsync --dry-run -av src/ dest/          # 预演（不实际执行）
```

**选项含义：**
- `-a` (archive) 归档模式（保留权限、时间等）
- `-v` (verbose) 显示详细过程
- `-z` (compress) 传输时压缩
- `-P` (progress + partial) 显示进度+支持断点续传
- `--delete` 删除目标中源不存在的文件
- `--exclude` 排除指定文件
- `--dry-run` 预演模式

### `traceroute` — 追踪路由路径

```bash
traceroute google.com       # 显示到目标的每一跳
traceroute -n google.com    # 不解析域名（更快）
traceroute -m 20 host       # 最大跳数20
```

### `nslookup` / `dig` — DNS查询

```bash
nslookup example.com        # 查询域名IP
dig example.com             # 详细DNS查询
dig +short example.com      # 只输出IP
dig example.com MX          # 查询邮件记录
dig @8.8.8.8 example.com   # 指定DNS服务器查询
```

---


## 9. 磁盘与存储

### `df` — 查看磁盘空间使用（disk free）

```bash
df                  # 显示所有文件系统
df -h               # 人类可读格式（GB/MB）
df -T               # 显示文件系统类型
df -i               # 显示 inode 使用情况
df /home            # 查看指定挂载点
```

**选项含义：**
- `-h` (human-readable) 人类可读
- `-T` (type) 显示文件系统类型
- `-i` (inodes) 显示inode使用

### `du` — 查看目录/文件大小（disk usage）

```bash
du -sh dir/             # 查看目录总大小
du -sh *                # 当前目录每项大小
du -ah dir/             # 递归显示所有文件大小
du -h --max-depth=1     # 只显示第一层子目录大小
du -sh * | sort -rh     # 按大小倒序排列
du --exclude="*.log" -sh dir/  # 排除某些文件
```

**选项含义：**
- `-s` (summarize) 只显示总计
- `-h` (human-readable) 人类可读
- `-a` (all) 显示所有文件（不只是目录）
- `--max-depth=N` 限制递归深度
- `--exclude` 排除模式

### `mount` / `umount` — 挂载/卸载文件系统

```bash
mount                       # 查看当前所有挂载
mount /dev/sdb1 /mnt/usb    # 挂载设备到目录
mount -t ext4 /dev/sda1 /mnt  # 指定文件系统类型
mount -o ro /dev/sdb1 /mnt  # 以只读方式挂载
umount /mnt/usb             # 卸载
umount -l /mnt/usb          # 懒卸载（繁忙时延迟卸载）
```

**选项含义：**
- `-t` (type) 文件系统类型
- `-o` (options) 挂载选项（ro只读, rw读写, noexec禁止执行）
- `-l` (lazy) 延迟卸载

### `fdisk` — 磁盘分区管理

```bash
fdisk -l                # 列出所有磁盘和分区
fdisk /dev/sdb          # 对指定磁盘进行分区操作（交互）
```

### `lsblk` — 列出块设备

```bash
lsblk                  # 树形显示所有块设备
lsblk -f               # 显示文件系统信息
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT  # 自定义列
```

### `mkfs` — 创建文件系统（格式化）

```bash
mkfs.ext4 /dev/sdb1     # 格式化为ext4
mkfs.xfs /dev/sdb1      # 格式化为xfs
mkfs.vfat /dev/sdb1     # 格式化为FAT32
```

---


## 10. 压缩与解压

### `tar` — 打包/解包工具（tape archive）

```bash
# 打包压缩
tar -czf archive.tar.gz dir/        # gzip压缩打包
tar -cjf archive.tar.bz2 dir/       # bzip2压缩打包
tar -cJf archive.tar.xz dir/        # xz压缩打包（压缩率最高）
tar -cf archive.tar dir/             # 仅打包不压缩
tar -czf archive.tar.gz file1 file2  # 打包多个文件

# 解包解压
tar -xzf archive.tar.gz             # 解压gzip
tar -xjf archive.tar.bz2            # 解压bzip2
tar -xJf archive.tar.xz             # 解压xz
tar -xf archive.tar                 # 解包（自动检测格式）
tar -xzf archive.tar.gz -C /path/   # 解压到指定目录

# 查看内容
tar -tzf archive.tar.gz             # 列出压缩包内容（不解压）
tar -tvf archive.tar.gz             # 详细列出内容

# 排除文件
tar -czf backup.tar.gz --exclude="*.log" dir/
```

**选项含义：**
- `-c` (create) 创建归档
- `-x` (extract) 解压归档
- `-t` (list) 列出内容
- `-z` (gzip) 使用gzip压缩/解压
- `-j` (bzip2) 使用bzip2
- `-J` (xz) 使用xz
- `-f` (file) 指定归档文件名（必须放在最后！）
- `-v` (verbose) 显示过程
- `-C` (directory) 指定解压目标目录
- `--exclude` 排除文件

### `zip` / `unzip`

```bash
zip archive.zip file1 file2     # 压缩文件
zip -r archive.zip dir/         # 递归压缩目录
zip -e archive.zip file         # 加密压缩（需输入密码）
zip -u archive.zip newfile      # 更新压缩包
unzip archive.zip               # 解压
unzip archive.zip -d /path/     # 解压到指定目录
unzip -l archive.zip            # 列出内容（不解压）
unzip -o archive.zip            # 覆盖已有文件
```

**选项含义：**
- `-r` (recurse) 递归压缩
- `-e` (encrypt) 加密
- `-u` (update) 只更新已更改的文件
- `-d` (directory) 指定解压目录
- `-l` (list) 列出内容
- `-o` (overwrite) 覆盖

### `gzip` / `gunzip`

```bash
gzip file               # 压缩文件（原文件被替换为 file.gz）
gzip -k file            # 压缩但保留原文件
gzip -d file.gz         # 解压（等同 gunzip）
gzip -9 file            # 最大压缩率
gzip -l file.gz         # 查看压缩信息
gunzip file.gz          # 解压
zcat file.gz            # 不解压直接查看内容
```

**选项含义：**
- `-k` (keep) 保留原文件
- `-d` (decompress) 解压
- `-9` 最大压缩（1-9，9最高）
- `-l` (list) 显示压缩信息

---


## 11. 查找与搜索

### `find` — 在文件系统中查找文件

```bash
# 按名称查找
find / -name "*.py"             # 查找所有.py文件
find . -name "config*"          # 当前目录下以config开头的文件
find . -iname "readme*"         # 忽略大小写

# 按类型查找
find . -type f                  # 只查找文件
find . -type d                  # 只查找目录
find . -type l                  # 只查找符号链接

# 按大小查找
find . -size +100M              # 大于100MB的文件
find . -size -1k                # 小于1KB的文件
find . -empty                   # 空文件和空目录

# 按时间查找
find . -mtime -7                # 最近7天内修改的文件
find . -mtime +30               # 30天前修改的文件
find . -mmin -60                # 最近60分钟内修改的
find . -newer ref.txt           # 比ref.txt更新的文件

# 按权限查找
find . -perm 755                # 权限为755的文件
find . -perm -u+x               # 所有者有执行权限的文件

# 按所有者查找
find . -user root               # 属于root的文件
find . -group www               # 属于www组的文件

# 执行操作
find . -name "*.tmp" -delete    # 查找并删除
find . -name "*.sh" -exec chmod +x {} \;  # 查找并执行命令
find . -name "*.log" -exec rm {} +        # 更高效的执行
find . -type f -name "*.py" -print0 | xargs -0 grep "import"  # 配合xargs

# 组合条件
find . -name "*.py" -a -size +1M    # AND（-a 可省略）
find . -name "*.py" -o -name "*.js" # OR
find . ! -name "*.pyc"              # NOT
find . -maxdepth 2 -name "*.txt"    # 限制搜索深度
```

**选项含义：**
- `-name` 按文件名匹配（支持通配符）
- `-iname` 忽略大小写的name
- `-type` 文件类型（f文件, d目录, l链接）
- `-size` 文件大小（+大于, -小于, c字节, k千字节, M兆, G吉）
- `-mtime` 修改时间（天）
- `-mmin` 修改时间（分钟）
- `-perm` 权限
- `-user` / `-group` 所有者/组
- `-exec` 对找到的文件执行命令（`{}` 代表文件，`\;` 结束）
- `-delete` 删除匹配文件
- `-maxdepth` 最大搜索深度
- `-mindepth` 最小搜索深度
- `-print0` 以null分隔输出（配合 `xargs -0`）

---

### `locate` — 快速定位文件（基于数据库索引）

```bash
locate filename         # 快速查找文件
locate -i filename      # 忽略大小写
locate -n 10 "*.conf"   # 只显示前10个结果
locate -c "*.py"        # 统计匹配数量
sudo updatedb           # 更新文件数据库（新文件需先更新）
```

**选项含义：**
- `-i` (ignore-case) 忽略大小写
- `-n` (limit) 限制输出数量
- `-c` (count) 只统计数量

### `which` — 查找命令的可执行文件路径

```bash
which python            # 输出 /usr/bin/python
which -a python         # 显示所有匹配的路径
```

### `whereis` — 查找命令的二进制、源码和手册位置

```bash
whereis nginx           # 查找nginx相关的所有文件
whereis -b nginx        # 只查找二进制文件
whereis -m nginx        # 只查找手册文件
```

### `type` — 查看命令类型

```bash
type ls                 # 显示 ls 是别名、内置还是外部命令
type -a ls              # 显示所有定义
```

---


## 12. 系统信息

### `uname` — 系统内核信息（Unix name）

```bash
uname               # 输出内核名称（Linux）
uname -a            # 所有信息
uname -r            # 内核版本
uname -m            # 机器架构（x86_64）
uname -n            # 主机名
uname -s            # 内核名称
```

**选项含义：**
- `-a` (all) 全部信息
- `-r` (release) 内核发行版本
- `-m` (machine) 硬件架构
- `-n` (nodename) 主机名
- `-s` (kernel-name) 内核名称

### `hostname` — 主机名管理

```bash
hostname            # 查看主机名
hostname -I         # 查看本机IP地址
hostnamectl         # 查看详细主机信息（systemd）
hostnamectl set-hostname newname  # 设置主机名
```

### `uptime` — 系统运行时间和负载

```bash
uptime              # 运行时间、用户数、平均负载
uptime -p           # 以易读格式显示运行时间
uptime -s           # 系统启动时间
```

### `free` — 内存使用情况

```bash
free                # 默认KB显示
free -h             # 人类可读格式
free -m             # 以MB显示
free -g             # 以GB显示
free -s 2           # 每2秒刷新一次
```

**选项含义：**
- `-h` (human) 人类可读
- `-m` (mega) 兆字节
- `-g` (giga) 吉字节
- `-s` (seconds) 持续显示间隔

**输出说明：**
- `total` 总内存
- `used` 已使用
- `free` 完全空闲
- `available` 可用（含可回收缓存）
- `buff/cache` 缓冲/缓存

### `lscpu` — CPU信息

```bash
lscpu               # 详细CPU信息（架构、核心数、频率等）
```

### `date` — 日期与时间

```bash
date                            # 当前日期时间
date "+%Y-%m-%d %H:%M:%S"      # 自定义格式
date +%s                        # Unix时间戳
date -d "2025-01-01"            # 解析指定日期
date -d "+3 days"               # 3天后的日期
date -d "last monday"           # 上周一
date --set="2025-01-01 12:00"   # 设置系统时间（需root）
```

**格式符号：**
- `%Y` 四位年份
- `%m` 月份(01-12)
- `%d` 日(01-31)
- `%H` 时(00-23)
- `%M` 分(00-59)
- `%S` 秒(00-59)
- `%s` Unix时间戳
- `%A` 星期全名
- `%a` 星期缩写

### `env` / `export` — 环境变量

```bash
env                         # 查看所有环境变量
echo $PATH                  # 查看单个变量
export MY_VAR="hello"       # 设置环境变量（当前会话）
export PATH=$PATH:/new/path # 追加PATH
unset MY_VAR               # 删除环境变量
printenv HOME              # 打印指定变量
```

### `history` — 命令历史

```bash
history                 # 查看全部历史
history 20              # 最近20条
history | grep ssh      # 搜索历史中的ssh命令
!100                    # 执行第100条历史命令
!!                      # 重复上一条命令
!ssh                    # 执行最近的ssh开头的命令
history -c              # 清除历史
```

### `dmesg` — 内核环形缓冲日志

```bash
dmesg                   # 查看内核日志
dmesg | tail -20        # 最近20行
dmesg -T                # 显示人类可读时间
dmesg -l err            # 只看错误级别
dmesg --follow          # 实时跟踪
```

### `watch` — 定期执行命令并显示结果

```bash
watch df -h             # 每2秒刷新磁盘使用
watch -n 1 "ps aux | head"  # 每1秒刷新
watch -d ls -la         # 高亮变化部分
watch -n 5 free -h      # 每5秒看内存
```

**选项含义：**
- `-n` (interval) 刷新间隔秒数（默认2秒）
- `-d` (differences) 高亮变化内容

---


## 13. 包管理

### Debian/Ubuntu（apt）

```bash
sudo apt update                 # 更新包索引（不安装）
sudo apt upgrade                # 升级所有已安装的包
sudo apt full-upgrade           # 升级（允许删除旧包）
sudo apt install nginx          # 安装包
sudo apt install nginx=1.18.0-0 # 安装指定版本
sudo apt remove nginx           # 卸载（保留配置）
sudo apt purge nginx            # 卸载（删除配置）
sudo apt autoremove             # 删除不再需要的依赖
sudo apt search keyword         # 搜索包
apt show nginx                  # 显示包详情
apt list --installed            # 列出已安装的包
apt list --upgradable           # 列出可升级的包
sudo apt-get clean              # 清理下载缓存
```

### CentOS/RHEL（yum / dnf）

```bash
sudo yum update                 # 更新所有包
sudo yum install nginx          # 安装
sudo yum remove nginx           # 卸载
sudo yum search keyword         # 搜索
yum info nginx                  # 包信息
yum list installed              # 已安装列表
sudo yum clean all              # 清理缓存

# dnf（CentOS 8+ 推荐）
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf upgrade
```

### 通用（pip — Python包管理）

```bash
pip install package             # 安装
pip install package==2.0.0      # 安装指定版本
pip install -U package          # 升级包
pip install -r requirements.txt # 从文件批量安装
pip uninstall package           # 卸载
pip list                        # 已安装列表
pip show package                # 包详情
pip freeze > requirements.txt   # 导出当前环境依赖
pip search keyword              # 搜索（可能不可用）
```

### Node.js（npm）

```bash
npm install package             # 安装到项目（dependencies）
npm install -D package          # 安装为开发依赖
npm install -g package          # 全局安装
npm uninstall package           # 卸载
npm update                      # 更新所有包
npm list                        # 查看已安装
npm run script                  # 运行脚本
npx command                     # 临时运行包命令
```

---


## 14. 防火墙与安全

### `iptables` — 传统防火墙

```bash
iptables -L                     # 列出所有规则
iptables -L -n -v               # 详细列出（数字格式）
iptables -A INPUT -p tcp --dport 80 -j ACCEPT   # 允许80端口
iptables -A INPUT -p tcp --dport 22 -j ACCEPT   # 允许SSH
iptables -A INPUT -j DROP       # 拒绝其他所有入站
iptables -D INPUT 3             # 删除第3条规则
iptables -F                     # 清空所有规则
iptables-save > rules.txt       # 保存规则
iptables-restore < rules.txt    # 恢复规则
```

**选项含义：**
- `-L` (list) 列出规则
- `-A` (append) 追加规则
- `-D` (delete) 删除规则
- `-F` (flush) 清空规则
- `-p` (protocol) 协议（tcp/udp/icmp）
- `--dport` (destination port) 目标端口
- `-j` (jump) 动作（ACCEPT/DROP/REJECT）
- `-n` (numeric) 数字显示
- `-v` (verbose) 详细信息

### `ufw` — Ubuntu 简易防火墙

```bash
sudo ufw status             # 查看状态
sudo ufw enable             # 启用防火墙
sudo ufw disable            # 禁用防火墙
sudo ufw allow 80           # 允许80端口
sudo ufw allow ssh          # 允许SSH（22）
sudo ufw deny 3306          # 拒绝3306
sudo ufw delete allow 80    # 删除规则
sudo ufw allow from 192.168.1.0/24  # 允许网段
sudo ufw reset              # 重置所有规则
```

### `firewalld` — CentOS/RHEL 防火墙

```bash
sudo firewall-cmd --state                          # 查看状态
sudo firewall-cmd --list-all                       # 列出所有规则
sudo firewall-cmd --add-port=80/tcp --permanent    # 永久开放80
sudo firewall-cmd --remove-port=80/tcp --permanent # 移除
sudo firewall-cmd --add-service=http --permanent   # 按服务名开放
sudo firewall-cmd --reload                         # 重新加载
```

### 安全相关命令

```bash
# 查看登录记录
last                    # 最近登录记录
lastb                   # 失败的登录尝试
w                       # 当前登录用户及活动
who                     # 当前登录用户

# SSH密钥管理
ssh-keygen -t rsa -b 4096               # 生成密钥对
ssh-keygen -t ed25519                    # 推荐的更安全算法
ssh-copy-id user@host                    # 复制公钥到远程主机

# 文件完整性
md5sum file             # 计算MD5校验
sha256sum file          # 计算SHA256校验
```

---


## 15. Shell 脚本基础

### 变量

```bash
name="world"            # 定义变量（等号两边无空格！）
echo "Hello $name"      # 使用变量
echo "Hello ${name}!"   # 花括号明确变量边界
readonly PI=3.14        # 只读变量
unset name              # 删除变量
```

### 特殊变量

```bash
$0          # 脚本名称
$1 ~ $9     # 位置参数（第1到第9个参数）
$#          # 参数个数
$@          # 所有参数（作为独立字符串）
$*          # 所有参数（作为一个字符串）
$?          # 上一条命令的退出状态（0=成功）
$$          # 当前脚本的PID
$!          # 最近一个后台进程的PID
```

### 条件判断

```bash
# if 语句
if [ condition ]; then
    command
elif [ condition ]; then
    command
else
    command
fi

# 文件测试
[ -f file ]     # 文件存在且为普通文件
[ -d dir ]      # 目录存在
[ -e path ]     # 路径存在（文件或目录）
[ -r file ]     # 可读
[ -w file ]     # 可写
[ -x file ]     # 可执行
[ -s file ]     # 文件非空

# 字符串测试
[ -z "$str" ]   # 字符串为空
[ -n "$str" ]   # 字符串非空
[ "$a" = "$b" ] # 字符串相等
[ "$a" != "$b" ]# 字符串不等

# 数字比较
[ $a -eq $b ]   # 等于 (equal)
[ $a -ne $b ]   # 不等于 (not equal)
[ $a -gt $b ]   # 大于 (greater than)
[ $a -lt $b ]   # 小于 (less than)
[ $a -ge $b ]   # 大于等于 (greater or equal)
[ $a -le $b ]   # 小于等于 (less or equal)
```

### 循环

```bash
# for 循环
for i in 1 2 3 4 5; do
    echo $i
done

for file in *.txt; do
    echo "Processing $file"
done

for i in $(seq 1 10); do
    echo $i
done

# while 循环
while [ $count -lt 10 ]; do
    echo $count
    count=$((count + 1))
done

# 逐行读取文件
while IFS= read -r line; do
    echo "$line"
done < file.txt
```

### 函数

```bash
# 定义函数
my_function() {
    echo "参数1: $1"
    echo "参数2: $2"
    return 0
}

# 调用函数
my_function "hello" "world"
```

### 实用模式

```bash
# 脚本开头（推荐）
#!/bin/bash
set -euo pipefail   # e:出错即停 u:未定义变量报错 o pipefail:管道错误

# 检查命令是否存在
command -v docker &>/dev/null || { echo "Docker未安装"; exit 1; }

# 默认值
name=${1:-"default"}    # 如果$1为空则用default

# 字符串操作
${var#pattern}      # 删除最短前缀匹配
${var##pattern}     # 删除最长前缀匹配
${var%pattern}      # 删除最短后缀匹配
${var%%pattern}     # 删除最长后缀匹配
${var/old/new}      # 替换第一个匹配
${var//old/new}     # 替换所有匹配
${#var}             # 字符串长度
```

---


## 16. Shell 快捷操作

| 快捷键/语法 | 说明 |
|-------------|------|
| `Ctrl + C` | 中断当前命令（发送SIGINT信号） |
| `Ctrl + Z` | 挂起当前命令（发送SIGTSTP，可用fg恢复） |
| `Ctrl + D` | 退出终端 / 发送EOF |
| `Ctrl + R` | 反向搜索历史命令（输入关键词匹配） |
| `Ctrl + L` | 清屏（等同 `clear`） |
| `Ctrl + A` | 光标移到行首 |
| `Ctrl + E` | 光标移到行尾 |
| `Ctrl + U` | 删除光标到行首的内容 |
| `Ctrl + K` | 删除光标到行尾的内容 |
| `Ctrl + W` | 删除光标前一个单词 |
| `Ctrl + Y` | 粘贴刚删除的内容 |
| `Alt + B` | 光标后移一个单词 |
| `Alt + F` | 光标前移一个单词 |
| `Tab` | 自动补全命令/文件名 |
| `Tab Tab` | 显示所有可能的补全 |
| `!!` | 执行上一条命令 |
| `sudo !!` | 用sudo重新执行上一条命令 |
| `!$` | 上条命令的最后一个参数 |
| `!^` | 上条命令的第一个参数 |
| `!n` | 执行第n条历史命令 |
| `!string` | 执行最近以string开头的命令 |

### 重定向与管道

```bash
cmd > file          # 标准输出重定向到文件（覆盖）
cmd >> file         # 标准输出追加到文件
cmd 2> file         # 标准错误重定向到文件
cmd 2>&1            # 标准错误合并到标准输出
cmd &> file         # 标准输出和错误都写入文件
cmd < file          # 从文件读取输入
cmd1 | cmd2         # 管道：cmd1的输出作为cmd2的输入
cmd1 | tee file | cmd2  # 管道同时保存到文件
```

### 命令连接

```bash
cmd1 && cmd2        # cmd1成功（返回0）才执行cmd2
cmd1 || cmd2        # cmd1失败（非0）才执行cmd2
cmd1 ; cmd2         # 无论cmd1是否成功都执行cmd2
(cmd1 ; cmd2)       # 在子shell中执行
{ cmd1 ; cmd2 ; }   # 在当前shell中分组执行
```

### 命令替换

```bash
result=$(command)   # 推荐写法
result=`command`    # 旧写法（不推荐）
echo "Today is $(date +%A)"
```

---


## 17. 常用组合技巧

```bash
# 查找并删除7天前的日志
find /var/log -name "*.log" -mtime +7 -delete

# 实时查看最新日志并高亮关键词
tail -f app.log | grep --color=auto "ERROR"

# 统计当前目录下代码行数
find . -name "*.py" | xargs wc -l

# 查看占用某端口的进程
lsof -i :8080
ss -tulnp | grep 8080

# 批量重命名（将 .txt 改为 .md）
for f in *.txt; do mv "$f" "${f%.txt}.md"; done

# 监控目录变化
watch -n 2 "ls -la /tmp/"

# 快速创建备份
cp config.yaml{,.bak}

# 按内存排序进程（前20）
ps aux --sort=-%mem | head -20

# 查看最大的10个文件
du -ah . | sort -rh | head -10

# SSH 端口转发（本地转发）
ssh -L 8080:localhost:3000 user@remote_host

# 查看某目录下各子目录大小（排序）
du -h --max-depth=1 /var | sort -rh

# 替换文件中所有匹配内容
sed -i 's/old_text/new_text/g' file.txt

# 统计某日志中各IP的访问次数
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20

# 查看系统中最近修改的文件
find / -mmin -10 -type f 2>/dev/null

# 递归查找并替换目录中所有文件的内容
find . -name "*.py" -exec sed -i 's/old/new/g' {} +

# 清空文件内容但保留文件
> file.txt
# 或
truncate -s 0 file.txt

# 同时查看多个日志
tail -f /var/log/syslog /var/log/auth.log

# 生成随机密码
openssl rand -base64 32
# 或
tr -dc 'A-Za-z0-9!@#$%' < /dev/urandom | head -c 16

# 检查端口是否开放（无需安装额外工具）
echo > /dev/tcp/host/port && echo "open" || echo "closed"

# 比较两个目录的文件列表
diff <(ls dir1) <(ls dir2)

# 统计代码行数（排除空行和注释）
grep -cv '^\s*$\|^\s*#' script.py

# 下载网页中所有图片
wget -r -A "*.jpg,*.png" -P images/ https://example.com

# 查看TCP连接状态统计
ss -ant | awk '{print $1}' | sort | uniq -c | sort -rn

# 快速HTTP服务器（Python）
python3 -m http.server 8080

# 定时检查网站是否可访问
while true; do curl -s -o /dev/null -w "%{http_code}" https://site.com; sleep 60; done
```

---


## 18. 速记卡片

| 场景 | 命令 | 说明 |
|------|------|------|
| 我在哪？ | `pwd` | 打印当前工作目录 |
| 这里有啥？ | `ls -la` | 列出所有文件详情 |
| 文件多大？ | `du -sh file` | 显示文件/目录总大小 |
| 磁盘满了？ | `df -h` | 各分区使用情况 |
| 谁在占端口？ | `ss -tulnp \| grep PORT` | 查找端口对应进程 |
| 进程挂了？ | `ps aux \| grep name` → `kill PID` | 找到并杀掉 |
| 日志在刷啥？ | `tail -f /var/log/syslog` | 实时看日志 |
| 找个文件 | `find / -name "filename"` | 全盘搜索文件名 |
| 文件里搜内容 | `grep -rn "keyword" dir/` | 递归搜索+显示行号 |
| 远程传文件 | `scp local user@host:remote` | 安全复制 |
| 看看内存 | `free -h` | 内存使用情况 |
| CPU占用高？ | `top` 或 `htop` | 实时进程监控 |
| 改密码 | `passwd` | 修改当前用户密码 |
| 服务起不来？ | `systemctl status service` | 查看服务状态 |
| 谁登录了？ | `w` 或 `who` | 查看在线用户 |
| 网络不通？ | `ping host` + `traceroute host` | 测试连通性和路径 |
| 批量操作 | `find ... \| xargs ...` | 查找+批量执行 |
| 文本替换 | `sed -i 's/old/new/g' file` | 文件内替换 |
| 按列提取 | `awk '{print $1}' file` | 取第1列 |
| 看看谁最大 | `du -sh * \| sort -rh \| head` | 找出空间大户 |

---

> **提示**：
> - 善用 `man command` 查看完整手册
> - 善用 `command --help` 查看简短帮助
> - 善用 `tldr command` 查看常用示例（需安装 tldr）
> - 善用 `type command` 确认命令类型（别名/内置/外部）



---

## 19. 每个命令的常用组合

> 按命令分类，列出日常工作中最实用的管道组合和联合用法。

---

### ls 组合

```bash
ls -lhS | head -10              # 列出当前目录最大的10个文件
ls -lt | head -20               # 最近修改的20个文件
ls -la | grep "^d"              # 只列出目录
ls -la | grep "^-"              # 只列出普通文件
ls -lR | grep "\.py$"           # 递归列出所有.py文件
ls -1 | wc -l                   # 统计当前目录文件数量
ls -la --time=atime             # 按访问时间显示
```

---

### find 组合

```bash
find . -name "*.log" -size +100M                    # 找出大于100M的日志
find . -name "*.py" -exec grep -l "import os" {} \; # 找含特定内容的文件
find . -mtime -1 -type f                            # 24小时内修改的文件
find . -empty -type f -delete                       # 删除所有空文件
find . -name "*.tmp" -mtime +7 -delete              # 删除7天前的临时文件
find . -type f -name "*.sh" -exec chmod +x {} +     # 给所有.sh加执行权限
find . -type f | xargs grep -l "TODO"               # 找包含TODO的文件
find / -user root -perm -4000 2>/dev/null           # 找所有SUID文件
find . -name "node_modules" -type d -prune -exec rm -rf {} + # 删除所有node_modules
find . -type f -printf "%s %p\n" | sort -rn | head -10       # 找最大的10个文件
```

---

### grep 组合

```bash
grep -rn "error" . | grep -v "node_modules"         # 搜索但排除目录
grep -rn "TODO\|FIXME\|HACK" src/                   # 搜多个关键词
grep -c "error" *.log | grep -v ":0$"               # 找含error的文件及次数
grep -rl "old_api" . | xargs sed -i 's/old_api/new_api/g'  # 批量替换
ps aux | grep nginx | grep -v grep                  # 查进程（排除grep自身）
cat access.log | grep "404" | awk '{print $7}' | sort | uniq -c | sort -rn  # 统计404的URL
history | grep "git" | tail -20                     # 最近20条git相关命令
dmesg | grep -i "error\|fail\|warn"                 # 内核日志中的错误
grep -P "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}" file  # 提取IP地址
netstat -an | grep "ESTABLISHED" | wc -l            # 统计已建立的连接数
```

---

### awk 组合

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20   # 访问量TOP20 IP
df -h | awk '$5 > "80%" {print $0}'                                  # 磁盘使用超80%的分区
ps aux | awk '$3 > 50 {print $0}'                                    # CPU占用超50%的进程
ps aux | awk '{mem += $6} END {print mem/1024 " MB"}'                # 总内存占用
awk -F: '$3 >= 1000 {print $1}' /etc/passwd                          # 列出普通用户
cat access.log | awk '{print $4}' | cut -d: -f2 | sort | uniq -c     # 每小时访问量
awk '/error/{count++} END{print count}' app.log                      # 统计error出现次数
ls -l | awk '{total += $5} END {print total/1024/1024 " MB"}'        # 目录总大小
awk 'NR>=10 && NR<=20' file.txt                                      # 打印第10到20行
netstat -an | awk '/^tcp/ {print $6}' | sort | uniq -c | sort -rn   # TCP状态统计
```

---

### sed 组合

```bash
sed -i 's/http:/https:/g' *.html                    # 批量替换http为https
sed -n '10,20p' file.txt                            # 打印第10到20行
sed -i '/^$/d' file.txt                             # 删除所有空行
sed -i '/^#/d' config.conf                          # 删除所有注释行
sed -i '1i#!/bin/bash' script.sh                    # 在文件开头插入一行
sed -i '$a# End of file' file.txt                   # 在文件末尾追加
sed 's/[[:space:]]*$//' file.txt                    # 删除行尾空白
find . -name "*.py" -exec sed -i 's/old/new/g' {} + # 递归批量替换
sed -n '/START/,/END/p' file.txt                    # 打印两个标记之间的内容
sed -i.bak 's/foo/bar/g' file && rm file.bak        # 替换并清理备份
```

---

### xargs 组合

```bash
find . -name "*.pyc" -print0 | xargs -0 rm -f       # 安全删除（处理空格文件名）
cat urls.txt | xargs -n 1 -P 5 wget                 # 5个并发下载
echo "file1 file2 file3" | xargs -n 1 cp -t /dest/  # 批量复制到目标
find . -name "*.jpg" | xargs -I {} cp {} /backup/    # 批量备份图片
docker ps -q | xargs docker stop                     # 停止所有容器
git branch --merged | grep -v "main" | xargs git branch -d  # 删除已合并分支
find . -name "*.gz" | xargs -P 4 gunzip              # 4进程并行解压
cat hosts.txt | xargs -I {} ssh {} "uptime"          # 批量检查服务器
ps aux | grep zombie | awk '{print $2}' | xargs kill # 杀掉僵尸进程
```

---

### ps / kill 组合

```bash
ps aux --sort=-%mem | head -10                       # 内存占用TOP10
ps aux --sort=-%cpu | head -10                       # CPU占用TOP10
ps -ef | grep python | grep -v grep | awk '{print $2}' | xargs kill  # 杀掉所有python进程
ps aux | awk '{print $2, $4, $11}' | sort -k2 -rn | head  # 按内存排序显示进程
pgrep -f "app.py" | xargs kill -9                    # 按命令名强制杀
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head    # 自定义输出列
watch -n 1 "ps aux --sort=-%cpu | head -5"           # 实时监控CPU最高进程
ps aux | awk 'NR>1{a[$1]+=$6} END{for(i in a) print a[i]/1024"MB", i}' | sort -rn  # 按用户统计内存
```

---

### tail / head / cat 组合

```bash
tail -f /var/log/syslog | grep --line-buffered "error"   # 实时过滤日志
tail -f log1.log log2.log                                # 同时跟踪多个日志
head -100 big.csv | column -t -s ","                     # 格式化预览CSV
cat file.txt | tr ',' '\n'                               # 逗号分隔转换为每行一个
cat /dev/null > large.log                                # 清空日志不删文件
tail -n +2 data.csv                                      # 跳过表头（从第2行开始）
head -1 data.csv && tail -n +2 data.csv | sort -t',' -k2 # 保留表头排序
cat access.log | cut -d' ' -f1 | sort -u | wc -l        # 统计独立IP数
tac file.txt                                             # 倒序显示文件（最后一行在前）
cat -n script.py | grep "def "                           # 带行号显示函数定义
```

---

### sort / uniq 组合

```bash
sort file.txt | uniq -c | sort -rn                  # 统计频率并排序
cut -d',' -f2 data.csv | sort | uniq -c | sort -rn  # 统计CSV某列的值分布
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -20  # IP访问排行
comm -13 <(sort file1) <(sort file2)                # 在file2中但不在file1中的行
sort -t'.' -k1,1n -k2,2n -k3,3n -k4,4n ips.txt     # IP地址排序
paste -d',' file1 file2 | sort -t',' -k2 -rn        # 合并文件后按第2列排序
```

---

### du / df 组合

```bash
du -h --max-depth=1 / 2>/dev/null | sort -rh | head -15    # 根目录下最大目录
du -sh /var/log/* | sort -rh | head -10                    # 最大的日志文件
df -h | awk '$5+0 > 80'                                    # 使用率超80%的分区
watch -n 60 "df -h | grep '/dev/sda1'"                     # 持续监控磁盘使用
du -ah . 2>/dev/null | sort -rh | head -20                 # 当前目录最大的20个文件/目录
find / -xdev -type f -size +500M 2>/dev/null               # 全盘找大文件
ncdu /                                                      # 交互式磁盘分析（需安装）
```

---

### chmod / chown 组合

```bash
find . -type f -exec chmod 644 {} +                  # 所有文件设为644
find . -type d -exec chmod 755 {} +                  # 所有目录设为755
find /var/www -type f -exec chmod 644 {} + && find /var/www -type d -exec chmod 755 {} +  # Web目录标准权限
chown -R www-data:www-data /var/www/html             # Web目录归属
find . -name "*.sh" -exec chmod +x {} +              # 所有脚本加执行权限
chmod 600 ~/.ssh/id_rsa                              # SSH私钥安全权限
stat -c "%a %n" *                                    # 显示所有文件的数字权限
```

---

### tar / zip 组合

```bash
tar -czf backup_$(date +%Y%m%d).tar.gz /data/       # 带日期的备份
tar -czf - dir/ | ssh user@host "cat > backup.tar.gz"  # 打包直接传到远程
tar -xzf file.tar.gz -C /target/ --strip-components=1  # 解压去掉一层目录
find . -name "*.log" -mtime +30 | tar -czf old_logs.tar.gz -T -  # 打包30天前的日志
tar -czf backup.tar.gz --exclude="*.log" --exclude="node_modules" .  # 排除多种文件
zip -r deploy.zip . -x "*.git*" -x "node_modules/*"  # 打包部署文件
```

---

### ssh / scp / rsync 组合

```bash
ssh user@host "cd /app && git pull && systemctl restart app"  # 远程部署
ssh user@host "tar czf - /data" > local_backup.tar.gz        # 远程打包本地保存
rsync -avz --delete --exclude=".git" ./src/ user@host:/app/  # 同步代码（删除多余）
rsync -avz -e "ssh -p 2222" src/ user@host:dest/            # 指定端口同步
ssh -t user@jump "ssh user@internal"                          # 通过跳板机连接
scp -r user@host:/var/log/app/ ./logs/                       # 批量下载远程日志
for host in host1 host2 host3; do ssh $host "uptime"; done   # 批量检查
```

---

### systemctl / journalctl 组合

```bash
systemctl list-units --failed                                # 查看失败的服务
systemctl restart nginx && systemctl status nginx            # 重启并确认状态
journalctl -u nginx --since "1 hour ago" | grep error       # 最近1小时nginx错误
journalctl -u app -f | grep --line-buffered "Exception"     # 实时监控异常
systemctl list-units --type=service --state=running          # 列出运行中的服务
journalctl --disk-usage && journalctl --vacuum-size=500M    # 查看并清理日志
systemctl show nginx -p ActiveState                          # 快速检查服务状态
```

---

### curl / wget 组合

```bash
curl -s https://api.example.com | python3 -m json.tool      # 格式化JSON响应
curl -o /dev/null -s -w "%{http_code}\n" https://site.com   # 只看状态码
curl -X POST -H "Content-Type: application/json" -d '{"key":"val"}' url  # POST JSON
while true; do curl -s -o /dev/null -w "%{http_code} %{time_total}s\n" url; sleep 5; done  # 持续探测
wget -r -l 1 -A "*.pdf" https://example.com/docs/           # 下载某页所有PDF
curl -u user:pass ftp://host/file.txt -o file.txt           # FTP下载
curl -I -L url 2>/dev/null | grep "^HTTP\|^Location"        # 跟踪重定向链
```

---

### docker 常用组合（补充）

```bash
docker ps -q | xargs docker stop                             # 停止所有容器
docker images -q -f "dangling=true" | xargs docker rmi       # 删除悬空镜像
docker system prune -af                                      # 清理所有未使用资源
docker logs -f --tail 100 container_name                     # 跟踪最近100行日志
docker exec -it container_name /bin/bash                     # 进入容器
docker stats --no-stream                                     # 容器资源快照
docker cp container:/path/file ./local/                      # 从容器复制文件
docker-compose up -d && docker-compose logs -f               # 启动并跟踪日志
```

---

### git 常用组合（补充）

```bash
git log --oneline --graph -20                                # 可视化最近20条提交
git diff --stat HEAD~5                                       # 最近5次提交的修改统计
git stash && git pull && git stash pop                       # 暂存→拉取→恢复
git branch -a | grep -v "main\|master" | xargs git branch -D  # 删除所有本地分支
git log --author="name" --since="1 week ago" --oneline       # 某人最近一周提交
git log --all --full-history -- "**/filename"                 # 查找文件历史
git reset --soft HEAD~1                                      # 撤销上次提交保留修改
git clean -fd && git checkout .                              # 清理所有未跟踪和修改
```

---

### 网络排查组合

```bash
ss -tulnp | grep LISTEN                                      # 所有监听端口
ping -c 3 host && traceroute host                            # 连通性+路由
curl -s ifconfig.me                                          # 查看公网IP
nslookup domain && dig domain +short                         # DNS双重验证
ss -s                                                        # 网络连接统计摘要
iptables -L -n --line-numbers                                # 带行号的防火墙规则
tcpdump -i eth0 port 80 -c 100                              # 抓取80端口前100个包
netstat -ant | awk '{print $6}' | sort | uniq -c | sort -rn  # 连接状态统计
lsof -i :3306                                                # 谁在用3306端口
ip route get 8.8.8.8                                         # 查看到某IP走哪个网卡
```

---

### 系统监控组合

```bash
top -bn1 | head -20                                          # 非交互式取top快照
free -h && echo "---" && df -h                               # 内存+磁盘一起看
vmstat 1 5                                                   # 5秒系统性能快照
iostat -x 1 3                                                # 磁盘IO监控
uptime && who && last -5                                     # 快速系统概况
cat /proc/loadavg                                            # 系统负载
ps -eo user,pid,%cpu,%mem,vsz,rss,comm --sort=-%mem | head -15  # 详细进程排行
sar -u 1 5                                                   # CPU使用率（需安装sysstat）
watch -n 2 "free -h; echo '---'; df -h /; echo '---'; uptime"  # 实时监控面板
```

---

### 用户和权限组合

```bash
id && groups                                                 # 快速查看自己的身份
last -10                                                     # 最近10条登录记录
who -b                                                       # 系统上次启动时间
awk -F: '$3>=1000 && $3<65534 {print $1}' /etc/passwd       # 列出所有普通用户
grep "Failed password" /var/log/auth.log | tail -20          # 最近的登录失败
passwd -S username                                           # 查看密码状态
find / -perm -4000 2>/dev/null                               # 查找所有SUID文件
getent group sudo                                            # 查看sudo组成员
```

---

### 文本处理综合组合

```bash
# JSON处理
cat data.json | python3 -m json.tool                         # 格式化JSON
cat data.json | python3 -c "import sys,json; print(json.load(sys.stdin)['key'])"  # 提取字段

# CSV处理
head -1 data.csv | tr ',' '\n' | cat -n                      # 查看CSV列名和编号
awk -F',' '{print $1","$3}' data.csv                         # 提取CSV指定列
sort -t',' -k2 -rn data.csv                                  # 按CSV第2列数字排序

# 日志分析
awk '{print $4}' access.log | cut -d: -f2 | sort | uniq -c | sort -rn  # 每小时请求量
grep "500" access.log | awk '{print $7}' | sort | uniq -c | sort -rn   # 500错误的URL排行
awk '$10 > 1000000' access.log                               # 响应大于1MB的请求
awk '{sum += $10} END {print sum/1024/1024 " MB"}' access.log  # 总流量

# 多文件操作
paste file1.txt file2.txt                                    # 按列合并两个文件
pr -m -t file1 file2                                         # 并排显示两个文件
diff <(sort file1) <(sort file2)                             # 比较排序后的差异
```

---

### crontab 实用组合

```bash
# 每天凌晨3点备份数据库
0 3 * * * mysqldump -u root -p'pass' dbname | gzip > /backup/db_$(date +\%Y\%m\%d).sql.gz

# 每5分钟检查服务是否存活，挂了就重启
*/5 * * * * systemctl is-active --quiet nginx || systemctl restart nginx

# 每天清理7天前的日志
0 2 * * * find /var/log/app/ -name "*.log" -mtime +7 -delete

# 每小时同步代码
0 * * * * cd /var/www/app && git pull origin main >> /var/log/deploy.log 2>&1

# 每周日凌晨做全量备份
0 1 * * 0 tar -czf /backup/full_$(date +\%Y\%m\%d).tar.gz /data/
```

---

### 一行搞定的实用组合

```bash
# 快速查看服务器状态概况
echo "=== 系统 ===" && uptime && echo "=== 内存 ===" && free -h && echo "=== 磁盘 ===" && df -h && echo "=== 负载TOP5 ===" && ps aux --sort=-%cpu | head -6

# 查找并杀掉占用某端口的进程
lsof -ti :8080 | xargs kill -9

# 一键清理系统（Ubuntu）
sudo apt autoremove -y && sudo apt clean && sudo journalctl --vacuum-time=3d

# 快速创建项目结构
mkdir -p project/{src,tests,docs,config} && touch project/{README.md,Makefile,.gitignore}

# 导出所有环境变量为 .env 格式
env | sort | sed 's/=\(.*\)/="\1"/' > .env.backup

# 统计代码仓库各语言行数
find . -name "*.py" -o -name "*.js" -o -name "*.go" | xargs wc -l | sort -rn | head -20

# 监控某个进程的内存变化
while true; do ps -o rss= -p $(pgrep app_name) | awk '{print $1/1024"MB"}'; sleep 5; done

# 批量修改文件扩展名
for f in *.jpeg; do mv "$f" "${f%.jpeg}.jpg"; done

# 查看当前连接最多的IP
ss -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -rn | head -10
```

---

> **提示**：组合命令的核心思想是 **管道（|）** + **命令替换 $()** + **xargs**，掌握这三个就能自由组合出各种强大操作！
