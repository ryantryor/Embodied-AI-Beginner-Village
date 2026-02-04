**常用 Linux 命令 + Shell 基础详解**  

以下内容按实际使用频率和重要程度排序，分为几个层级，每条命令都包含：

- 基本用法
- 常用选项（高频）
- 典型使用场景
- 记忆小技巧 / 易错点

### 第一层：最核心的导航与文件操作命令（必须滚瓜烂熟）

| 命令     | 基本含义              | 常用写法与选项                              | 典型用法举例                                      | 记忆 / 注意事项                              |
|----------|-----------------------|---------------------------------------------|---------------------------------------------------|----------------------------------------------|
| pwd      | 显示当前工作目录      | `pwd`                                       | `pwd` → /home/student/project                     | 几乎只单独使用                               |
| cd       | 切换目录              | `cd 路径`<br>`cd ..`<br>`cd -`<br>`cd`      | `cd ..` 上级<br>`cd -` 回到上一个目录<br>`cd` 回家目录 | `cd -` 非常实用但很多人忘记                   |
| ls       | 列出目录内容          | `ls`<br>`ls -l`<br>`ls -a`<br>`ls -lh`<br>`ls -la` | `ls -la` 显示所有文件+详细信息<br>`ls *.cpp` 只看cpp文件 | `-l` 长格式、`-a` 含隐藏、`-h` 人类可读大小   |
| mkdir    | 创建目录              | `mkdir dir`<br>`mkdir -p path/to/dir`       | `mkdir -p a/b/c/d` 递归创建多层目录               | `-p` 非常重要，防止父目录不存在报错           |
| rmdir    | 删除空目录            | `rmdir dir`                                 | 只删空目录                                        | 空目录专用，非空会失败                       |
| rm       | 删除文件/目录         | `rm file`<br>`rm -r dir`<br>`rm -rf dir`    | `rm -rf temp/` 危险！<br>`rm *.tmp` 删除临时文件  | `-r` 递归、`-f` 强制，**慎用 rm -rf /**       |
| cp       | 复制                  | `cp src dst`<br>`cp -r srcdir dstdir`       | `cp -r project project_backup`                    | `-r` 用于目录，`-i` 覆盖前询问               |
| mv       | 移动 / 重命名         | `mv src dst`<br>`mv file newname`           | `mv old.cpp new.cpp`<br>`mv file /tmp/`           | 同一目录下就是重命名，不同目录就是移动       |

### 第二层：查看、查找内容的核心命令

| 命令     | 主要作用                     | 常用写法与选项                              | 典型用法举例                                              | 记忆 / 注意事项                              |
|----------|------------------------------|---------------------------------------------|-----------------------------------------------------------|----------------------------------------------|
| cat      | 查看文件内容（小文件）       | `cat file.txt`<br>`cat -n file`             | `cat config.yaml`                                         | 大文件慎用（一次性全部加载）                 |
| less     | 分页查看文件（大文件推荐）   | `less file.log`<br>（上下箭头 / 搜索 /q退出） | `less access.log`                                         | 比 cat 更友好，大文件首选                     |
| head     | 查看文件开头                 | `head -n 10 file`<br>`head -n 20`           | `head -n 5 data.csv`                                      | 默认10行，`-n` 指定行数                      |
| tail     | 查看文件结尾（日志神器）     | `tail -n 50 file`<br>`tail -f file`         | `tail -f /var/log/nginx/access.log` 实时跟踪日志          | `-f` follow 实时查看，非常常用               |
| grep     | 在文本中搜索匹配行           | `grep 模式 file`<br>`grep -i`（忽略大小写）<br>`grep -r`（递归目录）<br>`grep -v`（取反）<br>`grep -n`（显示行号） | `grep "error" error.log`<br>`grep -r "TODO" .`<br>`ps aux \| grep python` | 几乎所有查找都离不开 grep                     |
| find     | 按文件名/类型/大小/时间查找文件 | `find 路径 -name 模式`<br>`find . -type f -name "*.cpp"`<br>`find . -mtime -7`（7天内修改） | `find . -name "*.log" -delete`<br>`find /var -size +100M` | 功能强大但语法稍复杂，`-name` 最常用          |

### 第三层：文本处理三剑客（awk、sed、xargs）——进阶必备

| 命令   | 核心作用                     | 常用写法示例                                                                 | 典型场景举例                                                                 | 记忆口诀 / 关键点                              |
|--------|------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------|
| grep   | 过滤行                       | `grep -E "error\|warning" log`                                               | 查找包含 error 或 warning 的行                                               | 基础过滤器                                     |
| sed    | 流编辑器（替换、删除、插入） | `sed 's/旧/新/g' file` <br>`sed -i 's/old/new/g' file`（原地修改）<br>`sed '/pattern/d' file`（删除匹配行） | `sed -i 's/127.0.0.1/localhost/g' hosts`<br>`sed '1,10d' file` 删除前10行 | **s/旧/新/g** 最经典写法<br>`-i` 记得加     |
| awk    | 字段处理 + 编程能力          | `awk '{print $1,$3}' file`<br>`awk -F, '{print $2}' csv`<br>`awk '{sum+=$3} END {print sum}'` | `awk '{print $1}' access.log \| sort \| uniq -c \| sort -nr` 统计IP访问量 | 第一列 $1、第二列 $2<br>END 块在最后执行     |
| xargs  | 把标准输入转为命令行参数     | `find . -name "*.tmp" \| xargs rm`<br>`echo file1 file2 \| xargs cp -t /backup` | `grep -l "error" *.log \| xargs less`<br>`find . -name "*.bak" -print0 \| xargs -0 rm` | 解决“参数太长”问题<br>危险操作必加 `-t` 预览 |

