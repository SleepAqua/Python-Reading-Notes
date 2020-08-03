# General Knowledges

## Python
* Python的区间永远是左闭右开
    * "1234567"[1:5] = "2345"，包含了"1234567"[1] = "2"，但不包含"1234567"[5] = "6"
    * list(range(1, 5)) = [1, 2, 3, 4], 相当于数学中的[1, 5)
* Python2 -- Python3区别：
    * 大部分差异：https://blog.csdn.net/pangzhaowen/article/details/80650478
    * 个人发现的差异：
        * FileExistsError
            * Python3: Optimistically create the directory, and if FileExistsError is raised, catch it and log the event.
            * Python 2, we must do this instead:
                * Optimistically create the directory.
                * if OSError is raised, catch it.
                * Inspect the exception’s errno attribute. If it’s equal to EEXIST, this means the directory already
                    existed; log that event.
                * If errno is something else, it means we don’t want to catch this exception here; re-raise the error
        * configparser包：在Python2中名为ConfigParser
        * json包：大量差异
        * 除法的截断问题
* 警惕可变类型
    * 可变类型（mutable）：列表，字典
        * l1 = list([] for _ in range(4)) --- √，当执行l2[0].append(1)时l2 ==> [[1], [], [], []]，因为创建了四次列表，每个列表指向不同的内存地址
        * l2 = [[]] * 4 --- × 当执行l2[0].append(1)时l2 ==> [[1], [1], [1], [1]]，因为只创建了一次列表，每个列表都指向同一内存地址
    * 不可变类型（unmutable）：数字，字符串，元组
* 谨慎使用多进程
    * 注意不要把pool = Pool(num)这种创建线程语句写入循环中，否则进程会积累起来不被释放，导致OSError: [Errno 24] Too many open files
    * 线程为进程的子项
* logging 一般化步骤：
    * getLogger > setLevel > addHandler
        * where Handler should be:
        * FIle/Stream Handler > setLevel > setFormatter
            * where Formatter should be:
            * "%(asctime)s  %(levelname)s : %(message)s", "%Y-%m-%d %H:%M:%S"
* 面向对象：
    * 使用\_\_slots\_\_可以限制实例新增绑定变量
* 高性能：
    * Cython对于I/O密集型或者调用第三方包为主的程序性能提升不大。
* r+, w+, a+ 在于光标位置的区别

------------------| r   r+   w   w+   a   a+
------------------|--------------------------
read              | +   +        +        +
write             |     +    +   +    +   +
write after seek  |     +    +   +
create            |          +   +    +   +
truncate          |          +   +
position at start | +   +    +   +
position at end   |                   +   +

* list的最大长度：https://stackoverflow.com/questions/855191/how-big-can-a-python-list-get


## Pandas
* 处理时间序列时善用.dt.方法：如.dt.date, .dt.round()
* SettingwithCopyWarning解决([指导文章](https://www.dataquest.io/blog/settingwithcopywarning/))：
    * Chained Assignment: `data[data.bidder == 'parakeet2004']['bidderrate'] = 100` 会创建副本而不会改变原表格，请用`data.loc[data.bidder == 'parakeet2004', 'bidderrate'] = 100`
    * Hidden chaining: `winners = data.loc[data.bid == data.price]` 后对winners进行修改也会改变原表格（因为winners相当于原表格的视图而非副本）`winners = data.loc[data.bid == data.price].copy()`才能达到只改变winners的效果
* NaN is a float，所以有缺失值的列如果本来是int，都会转换为float
* 行列转换善用.pivot()方法
* pandas 用 ~ 号不如用 == False
* to_hdf的时候append必须保持数据格式一致。
* 注意日期的数据，保存读取一定要转换格式，否则自动读成float会很难办！！
* NA值会导致一列int变为float, 警惕！
* 续：Int格式可以创建int格式的NA (dtype="Int64", pandas特有格式)
* 空值(即np.nan)不等于自身，需要用np.isnan或者math.isnan来判断


## Git
### Commonly used commands:
* git clone *url* 克隆仓库
* git status 查看git状态
* git add . 追踪所有文件
* git rm *file* 移除文件
* git commit -m "what did I do" 提交修改
* git push 推送提交的修改到远程仓库
* git pull 根据远程仓库，同步本地文件
* git log 查看提交记录
* git branch 新建分支
* git checkout 切换分支
* .gitignore只能忽略那些原来没有被track的文件，重新track需要git rm -r --cached
* git reset --hard xxxx 回滚：https://zhuanlan.zhihu.com/p/28532197

## Linux(Shell)
* ls 显示目录下文件 (-a全部, -l 详细, -lh详细+KB显示大小)
* pwd 显示目录
* cd 变换路径 (.. 返回上一层，-回退)
* cat, tac 查看文件
* head, tail 查看文件 (-n 为行数)
* Ctrl+Insert复制，Shift+Insert粘贴
* vi/vim 编辑文件
    * i 编辑模式
    * Esc 命令模式
        * :w写、:q退出
        * yy复制，p粘贴
        * dd删除一行
        * x删除一个
        * gg回到起点
        * /word=搜索word
        * u撤回，Ctrl+r重做
        * 数字+操作=数字次操作
        * :%s/pattern/target: 正则，注意转义!! "/"需要写为"\/"
* crontab -e 编辑 -l 查看 -r 删除
    * f1 f2 f3 f4 f5 program
    * 其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。
    * 可用逗号，短线表示多个或范围
* mv 除了移动文件夹/文件外，还可用于文件夹重命名
* rm 删除文件 （-r 递归删除）
* chgrp [-R] 属组名 文件名： 更改文件属组
* chown [–R] 属主名 文件名： 更改文件属主
* chmod u=rwx, g=rwx, o=rwx 文件名：r、w、x分别为读、写、执行；u、g、o分别为user, group, others
* find + 正则表达式 寻找当前目录的文件
* git 与windows同
* export 打印环境变量（./.zshrc）
* 文件重定向： >为输出、<为输入；1为抓取正常输出；2为抓取bug；> 2&>1为全部（或直接 &>）
* alias的坑：在Bash的非交互模式下(一般的脚本), alias是无效的. 解决：要么修改alias为绝对路径；要么改执行方式为souce
* “source filename”与“sh filename”、“./filename”这三个命令都可以用于执行一个脚本文件，那么它们之间的区别又如何呢？
    * 当shell脚本具有可执行权限时，用sh filename与./filename是没有区别的。./filename是因为当前目录没有在PATH中，所以"."是用来表示当前目录的。
    * sh filename会重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell是新建的，其改变的变量不会被带回父shell，除非使用export。
    * source filename读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面。
* top: 查看进程，s刷新，q退出
* date: +%Y显示年份、+%m显示月份，etc.
    * 可以进行运算，如：date -d "-2 month" +%Y%m%d 会显示两个月前的日期
    * 可以加格式直接输出，如：date +%Y/%m/%d 会显示为2020/01/06这样的输出
* Ctrl+a跳转到行首，Ctrl+e跳转到行尾
* rsync 常用模式 -av 注意：
    * 来源路径（第一个路径）不加`/`为该文件夹本身，加`/`为该文件夹下的所有文件，相当于`/*`
    * 而目标路径（第二个路径）不加`/`相当于加`/`，表示来源文件（夹）存放的路径
* wc用来计数(word count), ls -l |wc -l 打印当前文件夹下的文件总数
* 区分&&与; -- &&只有前一个命令成功才执行下一个而;无论上一个命令成功与否都会执行
* crontab的坑：crontab的python默认为/usr/bin/python，而非一般使用的anaconda下的Python，注意环境！
* Shell在不同Linux版本下，不同的编译器下，语法大相径庭，建议统一用bash或者直接用Python。
* [Unix Shell通配符](https://www.cnblogs.com/fengkui/p/6133896.html) 不同于 [正则表达式](https://docs.python.org/zh-cn/3.6/library/re.html)


## DataBase
* CURD = Create, Update, Retrieve, Delete
* ACID = Atomicity, Consistency, Isolation, Durability
* ETL = Extract, Transform, Load
