# Python 核心编程 第二版 读书笔记

部分章节 标准是[知道创宇研发技能表v3.0](http://blog.knownsec.com/Knownsec_RD_Checklist/)提到的章节.

## 第4章 Python 对象

### List

1. Python使用对象模型来存储数据.

1. 所有Python对象都有三个特性:身份,类型和值.[参考我的另一篇文章](http://abirdcfly.github.io/2016/04/06/Python-Mutation/#扩展)
值可改变.新式类(原文是new-style types and classes 但是我不清楚 new-style types是什么?)类型也可变.

1. 标准数据类型:(确实没有set 也确实有str)
- 数字
    - int
        - bool
        - long
    - float
    - complex
- 字符串
- list
- tuple
- dict


1.  type
```python
>>> class a():pass
...
>>> type(a)
<type 'classobj'>
>>> class b(object):pass
...
>>> type(b)
<type 'type'>
```

1. 通常情况下`obj = eval(repr(obj))`

1. 工厂函数:看起来是函数,实际是类.调用时,实际产生类的一个实例.

1. 其实在Python里一切都是指针.

1. decimal 高精度浮点类型
```python
from decimal import *
```
1. `dir()`不带参数返回当前位置的命名空间( return the list of names in the current local scope) 带参数,试图返回该对象的属性列表(attempt to return a list of valid attributes for that object.)[参考](https://docs.python.org/2/library/functions.html#dir)

1. 没有小浮点数集这说法...不过我很怀疑小整数集的实际作用...参考我的[这篇文章](http://abirdcfly.github.io/2016/04/07/pytthon-q/)的问题...先前是否优化都会被之后覆盖掉....

### Question
Q:  `type(a) == type(b) `与 `type(a) is type(b)`的区别是什么?课后题专门提到这点,除去理解上的差异,`==`表示值比较,`is`表示身份比较.他们两个表达式输出的值总是一致的.......


## 6.8节 Python Unicode

1. 不要使用`string`模块

1. `str()`与`chr()`对应`unicode()`与`unichr()`

1. 公式:
    Python2 ： str–(decode)–> unicode –(encode)–> str
    Python3 ： bytes –(decode)–> str –(encode)–> bytes

参考[这篇](http://abirdcfly.github.io/2016/04/08/python-encode-decode/)

## 8.11节 Python 迭代器

1. itertools 模块需要仔细看看.

1. 简单使用
```python
lista = [123, 'dawdwa', 789]
i = iter(lista)  # 变成迭代器
i.next()  # 可以一直用next()方法直到抛出StopIteration异常
# 用 for 函数自动停止.
```
1. 字典的迭代`for i in dict`里的`i`是`key`

1. 字典的内置方法
- `dict.iterkeys()`迭代`key`
- `dict.itervalues()`迭代`valus`
- `dict.iteritems()`迭代键值对

1. 对于文件下面两者相等.可以粗暴替换
```python
for eachline in filename:
    pass
for eachline in filename.readlines():
    pass
```
