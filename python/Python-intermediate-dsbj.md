# Python 进阶-读书笔记

这是 Python 进阶的读书笔记。

# 说明

感谢开源精神，[原文](https://github.com/yasoob/intermediatePython)及[翻译](https://github.com/eastlakeside/interpy-zh)

- Q表示阅读中的困惑。
- A表示查找资料或者思考后的回答，不一定对，欢迎指导。

# 1.\*args 和 \*\*kwargs

这个清楚，\*args代表不定数目参数，\*\*kwargs代表不定数目键值对。args和kwargs是约定俗成，可以换别的单词。三种参数的顺序是(fargs, \*args, \*\*kwargs)。

**Q**:底层怎么处理调用呢？ \*\*在这里具体表示什么呢？
比如:

``` python
def example(**kwargs):
    for key, value in kwargs.items():
        print "{0}={1}".format(key, value)

example(name='abirdcfly')
# name=abirdcfly

kwargs = {'name':'abirdcfly'}
example(**kwargs)
# name=abirdcfly

```
**A**：[Unpacking Argument Lists](https://docs.python.org/2/tutorial/controlflow.html#unpacking-argument-lists)

简洁的说，解包的作用。比如上面第 9 行代码中的`**`的作用就是解包为一个键值对。

`*`解包的例子：

``` python
args = [3, 6]
range(*args)
# [3, 4, 5]
```

PS：官方文档是大宝库。大而全。

# 2.Debugging

Python debugger(pdb) [官方用法说明](https://docs.python.org/2/library/pdb.html)

# 3.Generators

Iterable：有[\_\_iter__](https://docs.python.org/2/reference/datamodel.html#object.__iter__)或者[\_\_getitem__](https://docs.python.org/2/reference/datamodel.html#object.__getitem__)方法的对象。

Iterator：next或者\_\_next__方法的对象。

Iteration：过程。

iterator：迭代器。
**Q**：前两者区别？

Generators：只能迭代一次的迭代器。运行时生成？（yield）值

许多Python 2里的标准库函数都会返回列表，而Python 3都修改成了返回生成器，因为生成器占用更少的资源。（为什么莫名想笑。。我的迷之笑点。。）

[iter()函数](https://docs.python.org/2/library/functions.html?highlight=iter#iter)

[next()函数](https://docs.python.org/2/library/stdtypes.html#iterator.next)

# 4.Map & Filter

配合lambdas函数
[map](https://docs.python.org/2/library/functions.html?highlight=map#map)

map(function_to_apply, list_of_inputs)

[filter](https://docs.python.org/2/library/functions.html?highlight=filter#filter)

``` python
filter(function, iterable)
# if function is not None
[item for item in iterable if function(item)]
# if function is None
[item for item in iterable if item]
```

# 5.set

[set](https://docs.python.org/2/library/stdtypes.html?highlight=set#set-types-set-frozenset)

这是搜狐面试问过的东西，确实自己没注意过。集合不包含重复的值，集合存储的只是值吗？

回答应该是 set 存储的是 key。我记得我好像答的是value。= = 内部原理用的hash table 哈希表，和dict 差不多，只不过优化了一下底层存储，所有的value都为null。

[Quora: How is set implemented internally in python?](https://www.quora.com/How-is-set-implemented-internally-in-python)

然后进一步发现我之前洋洋得意的用set和deque双倍存储实现有序和去重实在是太low。可以改一改用这个[OrderedSet](http://code.activestate.com/recipes/576694/)。

[Stackoverflow:Does Python have an ordered set?](http://stackoverflow.com/questions/1653970/does-python-have-an-ordered-set)

# 6.三元运算符
对应的是 C++[条件（三元）运算符 (?:)][1]

[1]:https://msdn.microsoft.com/zh-cn/library/zakwfxx4(v=vs.90).aspx：

``` cpp
x>y ? x : y
```

在 Python 里的表示是：

``` python
condition_is_true if condition else condition_is_false
# 或者：
(if_test_is_false, if_test_is_true)[test]
# 利用了test为真为1
# 不 Pythonnic, 不推荐.
```

# 7.Decorators

函数可以嵌套.

函数中可以返回函数.

函数可以作为参数.

装饰器的使用情景:
- 授权
- 日志

可以用装饰器类.

[coolshell : Python修饰器的函数式编程](http://coolshell.cn/articles/11265.html)

# 8.Global & return

函数返回多个变量,可以用 Global 定义全局变量(不推荐)或者返回一个元祖.
``` python
def profile():
    global name
    global age
    name = "Danny"
    age = 30

def profile():
    name = "Danny"
    age = 30
    return (name, age)
```
# 9.Mutation
``` python
foo = ['hi']
print(foo)
# Output: ['hi']

bar = foo
bar += ['bye']
print(foo)
# Output: ['hi', 'bye']
```
是的,可变对象的结果就是这样,虽然你把`foo`赋值给`bar`.但是`bar`的变动还是会同步到`foo`.因为`bar`根本就是`foo`的别名.
``` python
bar = foo
print id(bar), id(foo)
# Output:139943754876184 139943754876184
```
另一个例子:
``` python
def add_to(num, target=[]):
    target.append(num)
    return target

add_to(1)
# Output: [1]

add_to(2)
# Output: [1, 2]

add_to(3)
# Output: [1, 2, 3]
```

如果你希望函数运行结果是下面这样的话:
``` python
add_to(1)
# Output: [1]

add_to(2)
# Output: [2]

add_to(3)
# Output: [3]
```

函数应该是:
``` python
def add_to(num, target=None):
    if target is None:
        target = []
    target.append(num)
    return target
```

这里不止涉及到 mutability ,还有一条:
在 Python 中当函数被定义时，默认参数只会运算一次，而不是每次被调用时都会重新运算。你应该永远不要定义可变类型的默认参数，除非你知道你正在做什么。

# 10.\_\_slots__

[官方文档中对此的解释](https://docs.python.org/2/reference/datamodel.html#slots)

无论新旧类的实例都会有一个`dict`来存储属性.如果你的"小类"(属性不多的类)实例很多,这会占用大量的空间.运用`__slots__`可以避免创造这个字典,节省空间.恩,PyPy会默认做这种优化.
``` python
class MyClass(object):
  __slots__ = ['name', 'identifier']
  # 只能在新式类中使用,在基类中使用,在子类中使用无意义.
  def __init__(self, name, identifier):
      self.name = name
      self.identifier = identifier
      self.set_up()
```
[Stack Overflow一些讨论](http://stackoverflow.com/questions/472000/python-slots) 唔,建议不建议用这个还是有争论.

# 11.virtualenv
针对每个程序创建独立（隔离）的Python环境.

[官网介绍](http://docs.python-guide.org/en/latest/dev/virtualenvs/)清晰明了.

# 12.Collections

[官方文档](https://docs.python.org/2/library/collections.html)

| 名称       |  介绍         | 该版本之后出现  |
| :------------- :|:-------------:|: -----:|
| Counter     | 计数器:dict的子类用来统计hashable对象 | 2.7 |
| deque    | 双端队列[^2]      |  2.4 |
| defaultdict | 支持缺失值的dict子类     |   2.5 |
|namedtuple|带名字的元组|2.6|
|OrderedDict|记录顺序的字典|2.7|

Enum Python 3.4之后的枚举类型.

[^2]: 感觉这个说法不准确,很多中文资料对deque的翻译就是双端队列.确实,你能把他当双端队列使用.但是不止如此,你也能把他当做单端队列,栈.官方文档的描述是"list-like container with fast appends and pops on either end".duque是线程安全的

# 13.enumerate
允许我们遍历数据并自动计数

# 14.introspection
自省是指在运行时来判断一个对象的类型的能力。

|函数名|用途|返回类型
|:-----|:------|-------|
|dir|显示该对象的所有属性方法,无参数则返回当前作用域内所有名字|list|
|type|返回对象类型|type|
|id|返回对象id在CPython实现里是该对象的地址|builtin_function_or_method|
|inspect模块|[官方文档](https://docs.python.org/2/library/inspect.html)| |

# 15.comprehensions
推导式（又称解析式）是 Python 的一种独有特性，可以从一个数据序列构建另一个新的数据序列的结构体。
- 列表解析式:
``` python
multiples = [i for i in range(30) if i % 3 is 0]
# 3的倍数
```
- 字典解析式:
``` python
new_dict = {v: k for k, v in some_dict.items()}
```
- 集合解析式:
``` python
squared = {x**2 for x in [1, 1, 2]}
```
# 16.Exceptions
异常处理:
`try/except`语句中,`except`语句可以并列多条;可以用`except:`捕获所有异常.
`finally`从句会最终执行,可以用来做清理工作.
`try/else`中`else`语句会在`try`语句没有异常时执行,但是`else`语句中的异常讲不会捕获

# 17.lambda
匿名函数
``` python
lambda 参数:操作(参数)
```
例子:
``` python
    a = [(1, 2), (4, 1), (9, 10), (13, -3)]
    a.sort(key=lambda x: x[1])

    print(a)
    # Output: [(13, -3), (4, 1), (1, 2), (9, 10)]
```

# 18.一行式
第一次看到这个概念...我服...
[Powerful Python One-Liners](https://wiki.python.org/moin/Powerful%20Python%20One-Liners)
上代码:
``` python
# 简易局域网服务器
# Python 2
python -m SimpleHTTPServer
# Python 3
python -m http.server
# -m mod : run library module as a script (terminates option list)
```
``` python
# 漂亮的打印( Python REPL 里)
# 很尴尬的状况是, 在 terminal 里, pprint 和 print 是一样的效果 o(╯□╰)o
from pprint import pprint

my_dict = {'name': 'Yasoob', 'age': 'undefined', 'personality': 'awesome'}
pprint(my_dict)
```
``` python
# 格式化显示接送数据
cat file.json | python -m json.tool
```
``` python
# 脚本性能分析
python -m cProfile my_script.py
```
``` python
# CSV转换为json
python -c "import csv,json;print json.dumps(list(csv.reader(open('csv_file.csv'))))"
# -c cmd : program passed in as string (terminates option list)
```
``` python
# 列表辗平
a_list = [[1, 2], [3, 4], [5, 6]]
print(list(itertools.chain.from_iterable(a_list)))
# Output: [1, 2, 3, 4, 5, 6]
# or
print(list(itertools.chain(*a_list)))
# Output: [1, 2, 3, 4, 5, 6]
```
``` python
# 一行式的构造器
class A(object):
    def __init__(self, a, b, c, d, e, f):
        self.__dict__.update({k: v for k, v in locals().items() if k != 'self'})
```
# 19.For - Else
``` python
# 抓捕质数.
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print( n, 'equals', x, '*', n/x)
            break
            # else从句只会在前面的语句正常执行后执行,即不被break.
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```
# 20.使用C扩展
使用C的理由:

1. 你要提升代码的运行速度，而且你知道C要比Python快50倍以上
1. C语言中有很多传统类库，而且有些正是你想要的，但你又不想用Python去重写它们
1. 想对从内存到文件接口这样的底层资源进行访问
1. .......我能想到的还有比如避免多线程GIL

方法有三:
- ctypes
- SWIG
- Python/C API

## ctypes:简单,清晰同时受限

大概的顺序是把原生`.c`文件编译成`.so`文件( win 下为 DLL 文件)然后在 Python 文件中调用.

原文的例子:
``` c
//sample C file to add 2 numbers - int and floats

#include <stdio.h>

int add_int(int, int);
float add_float(float, float);

int add_int(int num1, int num2){
    return num1 + num2;

}

float add_float(float num1, float num2){
    return num1 + num2;

}
```
接下来将C文件编译为.so文件(windows下为DLL)。下面操作会生成adder.so文件:
``` bash
#For Linux
$  gcc -shared -Wl,-soname,adder -o adder.so -fPIC add.c

#For Mac
$ gcc -shared -Wl,-install_name,adder.so -o adder.so -fPIC add.c
```
现在在你的Python代码中来调用它:
``` python
from ctypes import *

#load the shared object file
adder = CDLL('./adder.so')

#Find sum of integers
res_int = adder.add_int(4,5)
print "Sum of 4 and 5 = " + str(res_int)

#Find sum of floats
a = c_float(5.5)
b = c_float(4.1)

add_float = adder.add_float
add_float.restype = c_float
print "Sum of 5.5 and 4.1 = ", str(add_float(a, b))
```
## SWIG:高效而复杂繁琐
实际上 SWIG 不止支持 Python. 几乎支持所有脚本语言.
[官网介绍](http://www.swig.org/tutorial.html)
[SWIG使用介绍](http://tristan-xi.org/?p=101)
具体例子可见官网的例子..没太懂...不上例子了.

## Python/C API:最广泛使用,简单,而且可以在C代码中操作你的Python对象
[原译文内容](https://eastlakeside.gitbooks.io/interpy-zh/content/c_extensions/python_c_api.html)没太懂..稍后再看看.

# 21.open
推荐的写法:
``` python
with io.open('summary.txt', 'w', encoding='utf-8') as outf:
    outf.write(text % len(jpgdata))
```
详细的解释是.`open`会返回一个文件句柄.用`with`可以确保最后归还句柄.
参数中`r`读.`w`写.`a`加.`+`可写.`b`二进制打开

# 22.同时兼容 Python 2 &  Python 3
- Future

Future模块可以在 Python 2 中导入 Python 3 的特性
``` python
from __future__ import print_function
print(print)
```
- 模块重命名
``` python
try:
    import urllib.request as urllib_request  # for Python 3
except ImportError:
    import urllib2 as urllib_request  # for Python 2
```

[Porting Python 2 Code to Python 3](https://docs.python.org/3/howto/pyporting.html)

# 23.协程
# 24.Function caching
# 25.Context managers
