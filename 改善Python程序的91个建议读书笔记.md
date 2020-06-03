### A. 格式
1. 充分体现Python优势的代码风格，即为Pythonic，如交换赋值、迭代器、format()、直接返回布尔表达式（如return x>0而不是return True if x>0 else False）等
2. 遵守[PEP8规范](https://www.python.org/dev/peps/pep-0008)
3. 单双引号没有区别；哈希表实现switch case
4. 注释的必要性
5. 适当添加空行，每行不超过80字符
6. 函数4原则：减少嵌套；减少参数；参数设默认值；单一职责
7. 常量集中（大写命名，或导入const包）

### B. 习惯
8. assert应用于捕获用户所定义的约束，而非程序本身的
9. 交换值不使用中间量不仅使代码优雅，也加快运行速度
10. Lazy evaluation: if语句中如果存在and且前一个条件不满足的话，下一个条件不会进行计算，所以可以把可能为真概率最低的条件放在最前面，加速运行
11. 枚举类型：enum.Enum
12. 用isinstance()而非type()检查类型，防止继承带来的不合逻辑。
13. float一定要指明精度再做判断，因为float在计算机内部的存储规则使之无法准确表示而是无限接近
14. eval is evil
15. 同时需要索引和值时优先使用enumerate()
16. is为True表示两个变量指向同一个id地址，==为True表示值相等
17. Python2中需要声明 -\*- coding: utf-8 -\*- 支持unicode编码，Python3自动支持
18. 空 \_\_init\_\_.py 文件构建包，\_\_init\_\_.py可写入import语句使得子包的module可被直接导入，写入\_\_all__变量可控制import *

### C. 基础
19. from import 可能导致的问题：命名空间冲突；循环嵌套导入的实现不能
20. 优先使用absolute import：该问题不存在于Python3
21. ++i 语法不适用于Python：Python会把第二个+解读为正号+
22. with 自动关闭资源（如with open），本质上是调用\_\_enter\_\_和\_\_exit\_\_方法
23. else不仅可以与if，还可以与except, for连用
24. 处理异常：
    * 异常粒度一致与合理：try中不要太多任务
    * except一定要加具体Exception
    * 注意异常捕获顺序（异常内部继承）
    * 异常信息要有提示性
25. finally不要加return
26. None是遵循单例模式的特有类型，不是False也不是0
27. 连接字符串优先用join而不是+，因为前者减少了申请和复制内存的次数，从而减少了运行所需时间
28. format而非%
29. 警惕可变类型，如列表
30. (1)为int, (1,)为tuple
31. 函数传参既不是传值也不是传引用，而是传对象：可变对象直接修改，不可变对象生成新对象赋值修改
32. 默认参数不要用可变对象如[]，最好用None
33. 慎用变长参数\*args与\*\*kwargs
34. str()面向客户，repr()面向开发者；直接输出变量为repr(), print()则调用str()；定义对象时总应该定义\_\_repr__()
35. classmethod用于工厂方法，staticmethod用于独立方法

### D. 库
36. str.find()找不到值时返回-1而不是None; 善用str.\*with()函数
37. 善用sorted中的key参数实现各式各样的排序（结合operator.itemgetter）
38. 用copy深度复制可变对象，避免其本身发生改变
39. 用强大的Counter()进行计数统计更Pythonic，善用其中的most_common()等方法
40. ConfigParser:ini文件中的\[DEFAILT\]键可以设置缺省值，缺省值支持%s语法，可以以一定的规则组合其他键内的值
41. argparse优于getopt, optparse：可分组，可加子命令
42. pandas.read_table可以作为迭代器
43. import xml.etree.cElementTree as ET 优于 xml.dom.minidom与xml.sax
44. cPickle的速度是pickle的1000倍，pickle存在安全问题
45. json——序列化性能稍逊于pickle，但扩展性、可读性更好
46. 用try except语句时辅以traceback.print_exc()完整打印栈信息
47. 用loggin.config配置logging
48. 多线程threading支持守护线程，可通过setDaemon()来设置daemon属性，为True时主线程结束子线程也结束，默认为False
49. 使用Queue保证多线程的安全

### E. 设计 
50. 单例模式不一定要“实例唯一”，重要的是每个实例的状态相同即可，故用[Borg模式](https://github.com/faif/python-patterns/blob/master/patterns/creational/borg.py)取代Singleton模式
51. 结合cls.\_\_bases\_\_+=tuple()进行mixin操作，相当于不需要定义抽象方法的建造者模式
52. 订阅模式message实现松耦合
53. 用state状态机省去大量条件判断或者装饰器，且更容易定位错误

### F. 机制
54. 一切皆对象，包括type
55.  \_\_init\_\_不是构造方法，\_\_new\_\_才是。\_\_new\_\_通常用来实现工厂模式，大部分情况下\_\_init\_\_足够。
56. 注意变量的作用域：局部（Local）、嵌套（Enclosing, nonlocal var）、全局（Global, global var）、内置（Built-in）
57. self就是实例对象本身
58. 继承顺序采用MRO搜索: 自左向右深度优先；避免菱形继承
59. 描述符就是一个具有绑定行为的对象属性， 如\_\_get\_\_、\_\_set\_\_ 、\_\_delete\_\_
60. 默认访问\_\_getattribute\_\_抛出Attribute异常时，才会访问\_\_getattr\_\_。\_\_getattr\_\_一般用于处理事先未定义的属性，实现数据隐藏(调用dir方法不会显示)
61. @property -- 一种特殊的数据描述符，可用于设置校验、检查赋值范围、或者对属性进行二次运算转换后再返回。也可以实现变量(伪)私有化。
62. 创建类时，如果不设置\_\_metaclass\_\_属性，type将被作为默认的元类
63. Python的对象协议即各种魔法方法，如\_\_add\_\_相当于+
64. Python为前缀语法，管道符`|`为中缀语法，有时中缀语法更清楚，可使用pipe包在python中实现中缀语法
65. 迭代器协议：next()是iter_obj.\_\_next\_\_的语法糖，for循环是遍历iter_obj的语法糖。善用itertools
66. 生成器是一种迭代器协议的实现，带有yield关键字的函数会返回一个生成器
67. 生成器实现：协程协议greenlet, gevent包 [协程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017968846697824)
68. GIL使得Python的线程安全不是一个问题，但同时限制了多线程在多核CPU上的运行速度。可使用multiprocessing绕过局限。
69. 循环引用可能导致内存泄漏；可以包含其他对象引用的容器对象（比如：list，set，dict，class，instance）都可能产生循环引用。

### G. 工具
70. PyPI = Python Package Index (PyPI)[https://pypi.org/] 可以直接进入压缩包目录使用python setup.py install安装。
71. 使用pip, yolk安装、管理包。可以直接通过pip install xxx.zip本地安装本地包。
72. distutils.core.setup可用于创建setup.py文件，paster用于创建包
73. 单元测试疑问：真的需要按照unittest规定的格式耗费大量时间编写单元测试吗？
74. 单元测试推测：单元测试可能只是一种思想，一种相对于集成测试的思想，重要的是分块编写、测试各种特殊情况、再组合（就像我平时做的那样），而不是一定要按照规则编写setUp()和tearDown()
75. Test Driven Development （TDD）是手段，不是目的。
76. Pylint检查代码：pylint xxx.py
77. 代码审查：六顶帽子：
    * 1、陈述问题（白帽）；
    * 2、提出解决问题的方案（绿帽）；
    * 3、评估该方案的优点（黄帽）；
    * 4、列举该方案的缺点（黑帽）；
    * 5、对该方案进行直觉判断（红帽）；
    * 6、总结陈述，做出决策（蓝帽）。
78. 发布：setup.py sdist 打包；setup.py register 注册；setup.py sdist upload 上传。

### H. 优化
79. 优化的前提是可行、可读、可维护
80. 借助性能优化工具：Psyco（停止维护）、Pypy（Rpython的子集）
81. 使用 cProfile 或者 timeit 打印性能统计信息，定位性能瓶颈
82. 使用 memory_profiler 和 objgraph 剖析内存增长情况、对象引用关系
83. 常见数据结构基本操作的时间复杂度：
    * 列表：复制、插入O(n)；追加O(1)；切片O(k)
    * 集合：in O(1)-O(n)；| O(s+t)-O(s+t)；& O(min(s,t))-O(len(s)\*len(t))；- O(s)
    * 字典：in O(1)-O(n)；迭代 O(n)
    * 队列：出入列 O(1)；扩大 O(k)；删除 O(n)
84. 循环优化：
    * 减少循环内不必要计算（不变的值）
    * 显式循环改为隐式循环（用公式而非循环）
    * 使用局部变量而非全局变量（优先搜索，局部变量查询起来更快）
    * 尽量将内层嵌套的计算向上移
85. 优先使用生成器而非列表
86. 使用不同的数据结构优化性能：
    * bisect实现二分查找
    * heapq转化列表为堆，以O(1)获取极值，缺点是会打乱顺序
    * array的内存占用更小，数值计算速度更快，但字符处理速度不如列表
87. 充分利用集合的优势：尤其是处理交集、差集这种逻辑时（如获取两个不同目录下的同名文件），会大大加快运行速度
88. 使用multiprocessing规避GIL
    * 进程之间通信用Pipe比Queue快
    * 避免资源共享，因为进程资源共享更耗费资源
    * 为了避免平台差异导致的问题，尽量将相关资源作为子进程参数传入
    * 保证Pool.map()传入的对象是可序列化的
89. 使用线程池可以避免多次创建和销毁线程的开销（线程的生命周期有5个状态：创建、就绪、运行、阻塞和终止）
90. 可以通过Python API创建C模块，用setup.py编译为库，导入Python
91. 用Cython可以自动化实现上述功能，且还可直接调用C函数，进一步提升性能