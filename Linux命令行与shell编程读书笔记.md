# Linux命令行与shell编程读书笔记

## Linux命令行
### 基本命令
1. 基本命令:
    * 目录结构：/bin 二进制， /dev 设备节点 /proc 进程
    * 遍历、浏览、切换目录：ls, pwd, cd
    * 处理文件：touch, cp, mv, rename, mv, rm
    * 处理目录：rmdir, mkdir
    * 查看文件：file, cat, head, tail, less
2. 监测命令:
    * 检测进程：top, ps, uptime, who
    * 检测磁盘：df, du, mount

### 环境变量
1. 设置环境变量：
    * export var_name=xxx （“=”两边都不能含有空格，作用的是变量）
    * ~/.zshrc: alias var_name=xxx （“=”两边都不能含有空格，作用的是命令）

### 文件权限
1. 用户权限：
    * 密码文件：/etc/passwd与/etc/shadow
    * 用户增删：useradd, userdel
2. 文件权限：
    * 查看权限：drwxr-xr-x 第一个d表示目录，接着的rwx为文件属主的权限,接着的r-x为文件属组的权限,最后的r-x为其他用户的权限
    * 改变权限：chmod (u/g/o/a) (+/-/=) (r/w/x)， chmon 改变属主

### 文件系统
1. 创建分区：fdisk /dev/sdb

### vim编辑器
1. i 编辑模式
2. Esc 命令模式：
    * :w写、:q退出
    * yy复制，p粘贴
    * dd删除一行
    * x删除一个
    * gg回到起点
    * /word=搜索word
    * u撤回，Ctrl+r重做
    * 数字+操作=数字次操作
    * :%s/pattern/target: 正则，注意转义!! "/"需要写为"\/"


## shell脚本编程
### 基本脚本
1. 显示消息：echo Hello World
2. 变量：echo Date is $(date "+\%Y\%m\%d")
3. 重定向：>, &>, 1>, 2>, >>
4. 管道：ls -lhi | more
5. 数学运算：expr 1 + 5

### 结构化命令
1. if-then： if [] then [] else [] fi 常用来判断文件夹是否存在 if [! -d $dir] then mkdir -p 
2. test： test []
3. case： case [] in p1) [] p2) [] *) []
4. for: for [] in []: do [] done
5. while: while [] do [] done
6. unti: until [] do [] done
7. break, continue

### 处理输入
1. 命令行参数：位置参数 $1, $2, $0为脚本名
2. 特殊参数：$#为参数个数
3. 选项：用case为输入参数建立选项

### I/O
1. 文件描述符：0为STDIN，1为STDOUT，2为STDERR
2. 重定向：tee相当于管道的T型接口，既打印到屏幕又保存到文件, [] | tee -a file
3. 临时文件：mktemp testing.XXXXXX

### 控制脚本
1. 处理信号：Ctrl+C中断（SIGINT），Ctrl+Z暂停（SIGSTP）
2. 后台运行：命令末尾+&；bg
3. 非控制台运行：nohup；tmux
4. 查看作业：jobs, kill，top
5. 定时任务：crontab f1 f2 f3 f4 f5 program，其中f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。

### 进阶 -- sed与awk
1. sed：streming editor [Linux sed 命令](https://www.runoob.com/linux/linux-comm-sed.html)
2. awk：文本分析工具，之所以叫AWK是因为其取了三位创始人 Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的 Family Name 的首字符 [Linux awk 命令](https://www.runoob.com/linux/linux-comm-awk.html)
