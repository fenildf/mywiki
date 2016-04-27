# Python 错题本

```python
isinstance(type(u"haha"),basestring)  # False
issubclass(unicode, basestring)  # True
len(bin(5))  # 5
callable(1)  # False                           │                                              
直接执行一个Python文件的内置函数是?
例如     execfile("/tmp/a.py")

>>> a = {"a": 1, "b":2}
>>> getattr(a, "c", 4)
# 4

"usa13".isalnum()  # True isapha() False

'www.example.com'.lstrip('cmowz.')  # 'example.com' 

In [2]: "5"._________(10)
Out[2]: '0000000005'   # zfill

一Python文件内容如下:

class A(object):
    attr = 4

class B(A):

    def __init__(self, b):
        self.x = b

class C(B):
    x = 6

print C(9).x

执行该文件的结果为?  # 9

>>> for i in range(3):
...     print i
...     break
... else:
...     print 'end'
_______  # 0

In [29]: "%05.2f" % 1.111
Out[29]: '01.11'  

本题适用Python3, 填空:
>>>num="四"  # num=u"四" 就适用于 python 2 了
>>>num.isdigit()
False
>>>num.isnumeric()
_____  # True

>>> a = [1,2,3]
>>> for i in a:
...     # 做些判断
...     a.remove(i)
... 
>>> a
[2]  # 在循环里并不能删除列表.可以从根本上依次pop.然后判断

```
## 名字记差了的:
```python
divmod()
enumerate
'swapcase'
string.digits  # s...
global
nonlocal
startswith  # s...
```
## 老忘记
map(function, iterable, ...)
Apply function to every item of iterable and return a list of the results

filter(function, iterable)
Construct a list from those elements of iterable for which function returns true. 

reduce(function, iterable[, initializer])
Apply function of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value.