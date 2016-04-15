# \<\<Python 学习笔记\>\> 读书笔记

\<\<Python 学习笔记\>\> 读书笔记 主要记录以前模糊的地方.大部分是节选.偶尔会自己添东西.**自用**.建议完整过一遍这个PDF文件.

## 来源
[Python 学习笔记 第二版.pdf](https://raw.githubusercontent.com/qyuhen/book/master/Python%20%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%20%E7%AC%AC%E4%BA%8C%E7%89%88.pdf)

# 第一部分 基础知识

## 第一章 基本环境
挺有用的..建议多看几遍..总结的很好.
- 所有的内置类型对象都能从 types 模块中找到,以至于 int、long、str 这些关键字可以看做是简短别名。
- 名字空间:我们习惯于将 x 称为变量,但在这里里,更准确的词语是 **"名字"**。
- 和 C 变量名是内存地址别名不同,Python 的名字实际上是**一个字符串对象**,它和所指向的目标对象一起在名字空间(是一个字典)中构成一项 {name: object} 关联。
- Python 有多种名字空间,比如称为 `globals` 的`模块名字空间`,称为 `locals` 的`函数堆栈帧名字空间`(`frame.f_locals`),还有 `class`、`instance` 名字空间。不同的名字空间决定了对象的作用域和生存周期。
- 使用名字空间管理上下文对象,带来无与伦比的灵活性,但也牺牲了执行性能。毕竟从字典中查找对象远比指针低效很多,各有得失。
## 第 2 章 内置类型
- `r"abc\x"`r 前缀定义非转义的 raw-string
- `",".join(["a", "b", "c"])!`结果｀'a,b,c'｀
- `"a,b,c".split(",")`结果`['a', 'b', 'c']`
- `"a\nb\r\nc".splitlines()!`结果`['a', 'b', 'c']`
- `"a\nb\r\nc".splitlines(True)!`结果`['a\n', 'b\r\n', 'c']`
- 格式化:
    - 标记:- 左对⻬齐,+ 数字符号,# 进制前缀,或者用用空格、0 填充。
    - `"%#x, %#X" % (100, 200) `结果`'0x64, 0XC8'`
    - `"{0},{1},{0}".format(1, 2)`field 可多次使用用。`'1,2,1'`
    - `"{0:,.2f}".format(12345.6789)`千分位,带小小数位。`'12,345.68'`
    - `+`操作符:
```python
a = [1,2];b = [3,4];c = a+b
>>> print a,b,c
[1, 2] [3, 4] [1, 2, 3, 4]
>>> print id(a),id(b),id(c)
140518966786888 140518936595128 140518936594696
```
- list
```python
l = list("abcabc")
l.count("b")  # # 统计元素项。
# 2
l.index("a", 2)  # 从指定位置查找项,返回序号。
# 3
```
- 可用用 `bisect` 向有序列表中插入入元素。`bisect.insort(l, "b")`
- [数组](https://docs.python.org/2/library/array.html)
```python
>>> import array
>>> a = array.array("l", range(10))  # 用用其他序列类型初始化数组。
>>> a
array('l', [0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> a.tolist()  # 转换为列表。
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> a = array.array("c")  # 创建特定类型数组。
>>> a.fromstring("abc")  # 从字符串添加元素。
>>> a
array('c', 'abc')

a.fromlist(list("def"))  # 从列表添加元素。
>>> a
array('c', 'abcdef')

>>> a.extend(array.array("c", "xyz"))  # 合并列表或数组。
>>> a
array('c', 'abcdefxyz')
```
- 标准库另提供了特别的 namedtuple,可用名字访问元素项。
- 字典 (dict) 采用开放地址法的哈希表实现。
- 按需动态调整容量。扩容或收缩操作都将重新分配内存,重新哈希。删除元素操作不会立即收缩内存。
- 字典
    - 构造：
    ```python
    dict((["a", 0], ["b", 1]))  # {'a': 0, 'b': 1}
    dict(zip("ab", range(2)))  # {'a': 0, 'b': 1}
    dict(map(None, "abc", range(2)))  # {'a': 0, 'c': None, 'b': 1}
    ```
    - 删除：
    ```python
    >>> d = {"a":1, "b":2}
    >>> del d["b"]  # {'a': 1}
    >>> d.pop("b")
    2
    >>> d.popitem()
    ('a', 1)
    ```
    - 查找：
    ```python
    >>> d = {"a":1, "b":2}
    >>> print d.get("c")
    None
    >>> d.get("d", 123)
    123
    >>> d.setdefault("a", 100)  # 有直接返回值
    1
    >>> d.setdefault("c", 200)  # 没有设置值
    200
    ```
    - defaultdict
    ```python
    >>> from collections import defaultdict
    >>> d = defaultdict(list)
    >>> d["a"].append(1)!
    # key "a" 不存在,直接用用 list() 函数创建一一个空列表作为 value。
    >>> d["a"].append(2)
    >>> d["a"]
    [1, 2]
    ```
    - OrderedDict 顺序字典

- set 存的是key,`add remove discard update pop`
    - 把自定义类型放入set需要添加`hash`和`equal`
    ```python
    class User(object):
        def __init__(self, name):
            self.name = name

        def __hash__(self):
            return hash(self.name)

        def __eq__(self, o):
            if not o or not isinstance(o, User): return False
            return self.name == o.name
    ```

## 第 3 章 表达式
- 除非在函数中使用用关键字 `global`、`nolocal` 指明外部名字,否则赋值语句总是在当前名字空间创建
或修改` {name:object} `关联。
- 条件表达式不能包含赋值语句`if (x = 1) > 0: pass`错的
- `while`可选的` else `分支。如果循环没有被中断(没有`break`),那么` else `就会执行。
- `for `也有`while `用法同上.
-` for `可以多变量赋值.多层展开`for k, v in {"a":1, "b":2}.items(): print k, v`.`for i, (c1, c2) in d: pass`(`d = ((1, ["a", "b"]), (2, ["x", "y"]))`)
- `enumerate() `返回序号:`for index, content in enumerate("abc"):pass`
- 三元表达式的另一个实现:`print x > 0 and "A" or "B"`一般用的是`print "A" if x>0 else "B"`
- 可以用 or 提供默认值:`y = x or 0`
- `>>> eval("[0, 1, 2]")`结果`[0, 1, 2]`
- `vars()`获取指定对象的名字空间(默认` locals` )
- `dir`获取 `locals `名字空间中的所有名字,或指定对象所有可访问成员 (包括基类)。
```python
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'c1', 'c2', 'd', 'i', 'x']
>>> vars()
{'x': -1, 'd': ((1, ['a', 'b']), (2, ['x', 'y'])), '__builtins__': <module '__builtin__' (built-in)>, '__package__': None, 'i': 2, 'c2': 'y', '__name__': '__main__', 'c1': 'x', '__doc__': None}
```
## 第 4 章 函数
- 支持递归调用,但不进行尾递归优化。最大深度 sys.getrecursionlimit()
- 函数形参和内部变量都存储在` locals `名字空间中。
- 除非使用用` global`、`nonlocal `特别声明,否则在函数内部使用赋值语句,总是在` locals `名字空间中新建一一个对象关联。注意:"赋值" 是指名字指向新的对象,而非通过名字改变对象状态。
- 引用用外部变量,那么按 LEGB 顺序在不同作用用域查找该名字。(`locals` -> `enclosing function` -> `globals` -> `__builtins__`)
- 使用闭包,还需注意 "延迟获取" 现象:
```python
>>> def test():
...     for i in range(3):
...             def a():
...                     print i
...             yield a
...
>>> a,b,c = test()
>>> a(),b(),c()
2
2
2
(None, None, None)
```
多加一句显示:
```python
>>> def test():
...     for i in range(3):
...             print "i",i
...             def a():
...                     print i
...             yield a
...
>>> a,b,c = test()
i 0
i 1
i 2
>>> a(),b(),c()
2
2
2
(None, None, None)
```
如果用return呢?
```python
>>> def test():
...     for i in range(3):
...             print "i",i
...             def a():
...                     print i
...             return a
...
>>> d,e,f = test()
i 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'function' object is not iterable
>>> d(),e(),f()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'd' is not defined
```
- ` inspect `模块可以用来查看函数调用内部堆栈细节
- 用 `functools.partial() `可以将函数包装成更简洁的版本。
```python
>>> from functools import partial
>>> def test(a, b, c):
...        print a, b, c
>>> f = partial(test, b = 2, c = 3)
# 为后续参数提供命名默认值。
>>> f(1)
1 2 3
```
## 第 5 章 迭代器
- `__iter__()` 和 `next() `两个方方法。前者返回迭代器对象,后者依次返回数据.
- `yield`实现简单协程:
```python
>>> def coroutine():
...     print "coroutine start..."
...     result = None
...     while True:
...             s = yield result
...             result = s.split(",")
...
>>> c = coroutine()   # 函数返回协程对象。
>>> c.send(None)   # 使用 send(None) 或 next() 启动协程。
coroutine start...
>>> c.send("a,b")     # 向协程发送消息,使其恢复执行。
['a', 'b']
>>> c.send("c,d")
['c', 'd']
>>> c.close()     # 关闭协程,使其退出。或用 c.throw() 使其引发异常。
>>> c.send("e,f")         # 无无法向已关闭的协程发送消息。
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration

```
- yield 消费者生产者模型
```python
>>> def consumer():
...        while True:
...             d = yield
...             if not d: break
...             print "consumer:", d
>>> c = consumer()   # 创建消费者
>>> c.send(None)    # 启动消费者
>>> c.send(1)    # 生生产数据,并提交给消费者。
consumer: 1
>>> c.send(2)
consumer: 2
>>> c.send(3)
consumer: 3
>>> c.send(None)    # 生生产结束,通知消费者结束。
StopIteration
```
- `yield` 改进回调
原函数
```python
>>> def framework(logic, callback):
...          s = logic()
...          print "[FX] logic: ", s
...          print "[FX] do something..."
...          callback("async:" + s)
>>> def logic():
...         s = "mylogic"
...         return s
>>> def callback(s):
...         print s
>>> framework(logic, callback)
[FX] logic: mylogic
[FX] do something...
async:mylogic
```
改进写法:
```python
>>> def framework(logic):
...         try:
...             it = logic()
...             s = next(it)
...             print "[FX] logic: ", s
...             print "[FX] do something"
...             it.send("async:" + s)
...        except StopIteration:
...             pass
>>> def logic():
...         s = "mylogic"
...         r = yield s
...         print r
>>> framework(logic)
[FX] logic: mylogic
[FX] do something
async:mylogic
```
- **`itertools` 库是神!!!**

        - chain 连接多个迭代器。
        ```python
        >>> it = chain(xrange(3), "abc")
        >>> list(it)
        [0, 1, 2, 'a', 'b', 'c']
        ```
        - `combinations `返回指定长度的元素顺序组合序列。
        ```python
        >>> it = combinations("abcd", 2)
        >>> list(it)
        [('a', 'b'), ('a', 'c'), ('a', 'd'), ('b', 'c'), ('b', 'd'), ('c', 'd')]
        >>> it = combinations(xrange(4), 2)
        >>> list(it)
        [(0, 1), (0, 2), (0, 3), (1, 2), (1, 3), (2, 3)]
        ```
        - `combinations_with_replacement `会额外返回同一一元素的组合。
        ```python
        >>> it = combinations_with_replacement("abcd", 2)
        >>> list(it)
        [('a', 'a'), ('a', 'b'), ('a', 'c'), ('a', 'd'), ('b', 'b'), ('b', 'c'), ('b', 'd'),
        ('c', 'c'), ('c', 'd'), ('d', 'd')]
        ```
        - `compress `按条件表过滤迭代器元素。
        ```python
        >>> it = compress("abcde", [1, 0, 1, 1, 0])
        >>> list(it)
        ['a', 'c', 'd']
        ```
        - `count `从起点开始,"无无限" 循环下去。
        ```python
        >>> for x in count(10, step = 2):
        ...         print x
        ...         if x > 17: break
        ```
        - `cycle` 迭代结束,再从头来过。
        ```python
        >>> for i, x in enumerate(cycle("abc")):
        ...         print x
        ...         if i > 7: break
        a
        b
        c
        a
        b
        c
        a
        b
        c
        ```
        - `dropwhile` 跳过头部符合条件的元素。
        ```python
        >>> it = dropwhile(lambda i: i < 4, [2, 1, 4, 1, 3])
        >>> list(it)
        [4, 1, 3]
        ```
        - `takewhile` 则仅保留头部符合条件的元素。
        ```python
        >>> it = takewhile(lambda i: i < 4, [2, 1, 4, 1, 3])
        >>> list(it)
        [2, 1]
        ```
        - groupby 将连续出现的相同元素进行行分组。
        ```python
        >>> [list(k) for k, g in groupby('AAAABBBCCDAABBCCDD')]
        [['A'], ['B'], ['C'], ['D'], ['A'], ['B'], ['C'], ['D']]
        >>> [list(g) for k, g in groupby('AAAABBBCCDAABBCCDD')]
        [['A', 'A', 'A', 'A'], ['B', 'B', 'B'], ['C', 'C'], ['D'], ['A', 'A'], ['B', 'B'], ['C',
        'C'], ['D', 'D']]
        >>> [list(g) for g in groupby('AAAABBBCCDAABBCCDD')]
        [['A', <itertools._grouper object at 0x7fb5d26d4910>], ['B', <itertools._grouper object at 0x7fb5d26d4950>], ['C', <itertools._grouper object at 0x7fb5d26d4990>], ['D', <itertools._grouper object at 0x7fb5d26d49d0>], ['A', <itertools._grouper object at 0x7fb5d26d4a10>], ['B', <itertools._grouper object at 0x7fb5d26d4a50>], ['C', <itertools._grouper object at 0x7fb5d26d4a90>], ['D', <itertools._grouper object at 0x7fb5d26d4ad0>]]
        ```
        - `ifilter `与内置函数` filter()` 类似,仅保留符合条件的元素。
        ```python
        >>> it = ifilter(lambda x: x % 2, xrange(10))
        >>> list(it)
        [1, 3, 5, 7, 9]
        ```
        - `ifilterfalse` 正好相反,保留不符合条件的元素。
        ```python
        >>> it = ifilterfalse(lambda x: x % 2, xrange(10))
        >>> list(it)
        [0, 2, 4, 6, 8]
        ```
        - `imap`与内置函数` map() `类似。
        ```python
        >>> it = imap(lambda x, y: x + y, (2,3,10), (5,2,3))
        >>> list(it)
        [7, 5, 13]
        ```
        - `islice`以切片片的方方式从迭代器获取元素。
        ```python
        >>> it = islice(xrange(10), 3)
        >>> list(it)
        [0, 1, 2]
        >>> it = islice(xrange(10), 3, 5)
        >>> list(it)
        81[3, 4]
        >>> it = islice(xrange(10), 3, 9, 2)
        >>> list(it)
        [3, 5, 7]
        ```
        - `izip `与内置函数` zip() `类似,多余元素会被抛弃。
        ```python
        >>> it = izip("abc", [1, 2])
        >>> list(it)
        [('a', 1), ('b', 2)]
        ```
        要保留多余元素可以用用` izip_longest`,它提供了一一个补缺参数。
        ```python
        >>> it = izip_longest("abc", [1, 2], fillvalue = 0)
        >>> list(it)
        [('a', 1), ('b', 2), ('c', 0)]
        ```
        - `permutations` 与 `combinations` 顺序组合不同,`permutations` 让每个元素都从头组合一一遍。
        ```python
        >>> it = permutations("abc", 2)
        >>> list(it)
        [('a', 'b'), ('a', 'c'), ('b', 'a'), ('b', 'c'), ('c', 'a'), ('c', 'b')]
        >>> it = combinations("abc", 2)
        >>> list(it)
        [('a', 'b'), ('a', 'c'), ('b', 'c')]
        ```
        - `product`让每个元素都和后面面的迭代器完整组合一一遍。
        ```python
        >>> it = product("abc", [0, 1])
        >>> list(it)
        [('a', 0), ('a', 1), ('b', 0), ('b', 1), ('c', 0), ('c', 1)]
        ```
        - `repeat `将一一个对象重复 n 次。
        ```python
        >>> it = repeat("a", 3)
        82>>> list(it)
        ['a', 'a', 'a']
        ```
        - `starmap` 按顺序处理每组元素。
        ```python
        >>> it = starmap(lambda x, y: x + y, [(1, 2), (10, 20)])
        >>> list(it)
        [3, 30]
        ```
        - `tee `复制迭代器。
        ```python
        >>> for it in tee(xrange(5), 3):
        ...
        print list(it)
        [0, 1, 2, 3, 4]
        [0, 1, 2, 3, 4]
        [0, 1, 2, 3, 4]
        ```

## 第 6 章 模块
- 可用 imp.find_module() 获取模块的具体文件信息。
- 将多个模块文件放到独立的目录,并提供初始化文件` __init__.py`,就形成了包 (`package`)。
无论是导入包,还是导入包中任何模块或成员,都会执行初始化文件,且仅执行一次。可用来初始
化包环境,存储帮助、版本等信息。
- 子目录在包外时:`__init__.py `中用用` __path__ `指定所有子目录的全路径即可 `__path__ = ["/home/yuhen/py/test/a", "/home/yuhen/py/test/b"]`

## 第 7 章 类
- 类型对象很特殊,在整个进程中是单例的,是不被回收的。
- 面向对象一个很重要的特征就是封装,它隐藏对象内部实现细节,仅暴露用户所需的接口。因此私有字段是极重要的,可避免非正常逻辑修改。
- 实例方法和函数的最大区别是` self `这个隐式参数。
- 多重继承成员搜索顺序,也就是` mro `(`method resolution order`) 要稍微复杂一点。归纳一下就是:从下到上 (深度优先,从派生生类到基类),从左到右 (基类声明顺序)。

## 第 8 章 异常
- 顺序`try-raise-except-else-finally`无错误`else`执行.`finally`总会执行
- 断言` assert`可加参数`python -O main.py`使断言失效
```python
def test(n):
...     assert n > 0, "n 必须大大于 0"!
...     print n   # 错误信息是可选的。
```

## 第 9 章 装饰器
- functools.wraps 是装饰器的装饰器,它的作用用是将原函数对象的指定属性复制给包装函数对象,默认有 __module__、__name__、__doc__,或者通过参数选择。
- 能干嘛?
    - AOP: 身身份验证、参数检查、异常日日志等等。
    - Proxy: 对⺫目目标函数注入入权限管理等。
    - Context: 提供函数级别的上下文文环境,比比如 Synchronized(func) 同步。
    - Caching: 先检查缓存是否过期,然后再决定是否调用用⺫目目标函数。

## 第 10 章 描述符

## 第 11 章 元类
```python
class = metaclass(...)    # 元类创建类型
instance = class(...)    # 类型创建实例
instance.__class__ is class    # 实例的类型
class.__class__ is metaclass   # 类型的类型
```

# 第二部分 标准库

## 第 12 章 字符串

### re 正则

见[正则](/python/re)
