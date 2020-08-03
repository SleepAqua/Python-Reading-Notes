<!--
 * @version: Python 3.7.3
 * @Author: Louis
 * @Date: 2020-07-29 09:56:19
 * @LastEditors: Louis
 * @LastEditTime: 2020-07-31 16:09:15
--> 
# Python自带电池（标准库）总结

## 笔者
因诺数据：田夏旭

## 简介
仅总结了本人不熟悉的一些电池。[信息来源](https://docs.python.org/3/library/)

## 模块
### 内建
1. breakpoint: calls `pdb.set_trace()`, __New in version 3.7__
2. compile: compile for compiling into code, so that code obj can be executed bt `exec()` or `eval()`
3. eval and exec: all for executing code, accept str or code objects, diff:
   1. `eval` for only static execution, return vals
   2. `exec` for dynamic execution, return None
4. delattr, hasattr, and setattr: all for operating object's attributes, respectively:
   1. delattr(obj, "attr") == del obj.attr
   2. hasattr(obj, "attr") == try getattr(obj, "attr"): return True; except AttributeError: return False
   3. setattr(obj, "attr", val) == obj.attr = val
5. filter: construct an iterator from those elements of iterable for which function returns true. `filter(function, iterable)`
6. locals and globals: return a dictionary representing the current local symbol table, locally and globally (including themselves)
7. chr and ord: Unicode transforming. chr returns a str giving a integer; ord returns a integer giving a str
8. property: return a property attribute. Use @property instead.
9. repr, ascii and print: all print something on the stream:
   1. repr returns a string containing a printable representation of an object. A class can defind `__repr__()` to control its repr return
   2. ascii is as same as `repr` but escape the non-ASCII characters
   3. print directly print the obsj to the stream, `print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`, use "\r"+str and `end=""` to refresh words. 
10. slice: return a slice obj, overridding used
   4. one arg specify `stop`, two args specify `start` and `stop`, three args specify `start`, `stop` and `step`
   5. usage example: `s = slice(*args); start, stop, step = s.start or 0, s.stop or sys.maxsize, s.step or 1`
11. reversed: return a reverse iterator
12. Ellipsis: `... is Ellipsis`, singleton, syntax sugar, can be used to fill range by defining `__getitem__` with `if key is ...:`
13. the built-in objects considered false:
    1. constants defined to be false: None and False.
    2. zero of any numeric type: 0, 0.0, 0j, Decimal(0), Fraction(0, 1)
    3. empty sequences and collections: '', (), [], {}, set(), range(0)
14. Operations summary:
    1. Boolean Operations -- and, or, not
    2. Comparisons -- <, <=, >, >=, ==, !=, is, is not
    3. Numeric Operations -- +, -, *, /, //(floor), %(remainder), conjugate, divmod(floor & remainder), **(pow)
    4. Bitwise Operations -- |, ^, &, <<, >>, ~
    5. Sequence Operations -- in, not in, +, *, len, min, max, .index, .count, append, clear, copy, insert, pop, remove, reverse
15. String Methods:
    1. casefold and lower: `casefold` is more aggressive than `lower` because it is intended to remove all case distinctions in a string
    2. center: `"{:-^50}".format("Hello") == "Hello".center(50, "-")`
    3. count: `count(sub[, start[, end]])` return the number of non-overlapping occurrences of substring sub in the range [start, end]
    4. expandtabs: return a copy of the string where all tab characters are replaced by given spaces (default 8)
    5. find: return the lowest index in the string where substring sub is found within the slice `s[start:end]`, return -1 if sub is not found, rather than 0 or raise errors!
    6. isalnum: return True if one of the followings is true: isalpha(), isdecimal(), isdigit(), or isnumeric()
    7. maketrans and translate: 
       1. maketrans is static method, returns a translation table usable for str.translate()
       2. `given_str.translate(str.maketrans(intab, outtab))` realizes the str substitution ruled by dict given by maketrans
### 文本处理
1. string: 
   1. constants: ascii_letters, ascii_lowercase, ascii_uppercase, digits, etc.
   2. formatter: `{:,}.format(digit)` gives a number with thousands separators, '<^>' for aligns of left, center and right.
   3. Template: `s = string.Template('$who likes $what'); s.substitute(who='tim', what='kung pao') == 'tim likes kung pao'`
   4. other func:
      1. `string.capwords("Be happy, young lad!") == " ".join([word.capitalize() for word in "Be happy, young lad!".split()])`

2. re:
   1. flags: 
      1. re.A == re.ASCII: make ASCII-only matching instead of full Unicode matching.
      2. re.DEBUG: Display debug information about compiled expression.
      3. re.I == re.IGNORECASE: perform case-insensitive matching
      4. re.L == re.LOCALE: make \w, \W, \b, \B and case-insensitive matching dependent on the current locale
      5. re.I == re.MULTILINE: make `^|$` matches at the beginning|ending of each line
      6. re.S == re.DOTALL: make `.` matches any character at all, including a newline
      7. re.X == re.VERBOSE: allow you write comments in regular expressions (whitespace within the pattern is ignored)
   2. greedyness: `*?` is the non-greedy pattern compare to `*`; while `.?` is the non-greedy pattern compare to `.`; etc.
   3. matching: 
      1. search() vs. match(): `search()` matches anywhere, `match()` matches beginning
      2. split() vs str.split(): re.split() is a str.split() supports re pattern, but slower!
      3. advanced matching:
         1. lookahead assertion: `(?=...)`, e.g., Isaac (?=Asimov) will match 'Isaac ' only if it’s followed by 'Asimov'
         2. negative lookahead assertion: `(?!...)`, e.g., Isaac (?!Asimov) will match 'Isaac ' only if it’s not followed by 'Asimov'
         3. lookbehind assertion: `(?<=...)`, e.g., (?<=Isaac) Asimov will match ' Asimov' only if it follows 'Isaac'
         4. negative lookbehind assertion: `(?!=...)`, e.g., (?<=Isaac) Asimov will match ' Asimov' only if itd oes not follows 'Isaac'
   4. grouping: 
      1. make group: `(...)` and `(?P<name>...)` both create a new group
      2. access group: use `.group(num)` and `.group(name)` to access Match Object
      3. show group: `.groups`, `.groupdict`
      4. access index: `.start`, `.end` to return the indices of the start and end of the substring matched by group
   5. utils: `escape`: 
      1. help you do the esccaping jobs

3. difflib:
### 数据结构

### 数学运算

### 函数工具

### 路径管理

### 数据转换

### 文件格式

### 加密解密

### 操作系统

### 并发执行

### 上下文变量

### 网络通信

### 网络数据

### 超文本工具

### 网络协议

### 多媒体

### 国际化

### 程序框架

### GUI编程

### 开发工具

### 调试分析

### 打包发布

### Python运行服务

### Python自定义解释器

### 导入模块

### Python语言服务

### MS Windows服务

### Unix服务

### 废弃模块
pass