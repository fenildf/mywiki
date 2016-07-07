Python 小技巧或者是语言注意点

# 参考链接
[翻译Stack Overflow](https://github.com/wklken/stackoverflow-py-top-qa/blob/master/contents/qa-control-flow.md)

## 字典默认值
```
data.get("key","moren")  # 没有返回值会返回moren
```

## 字符串中部分替换
替换一对字符`str.replace('1', '2')`把1替换成2     
替换多对,可以用`translate()`函数
```
from string import maketrans   

intab = "aeiou"
outtab = "12345"
trantab = maketrans(intab, outtab)

str = "this is string example....wow!!!";
print str.translate(trantab, 'xm');
# 将a替换成1,e替换成2....并删除x和m字符.
```

## list中随机选择
随机选择一个
```python
a = [1,3,5,7,9,3]
from random import choice
print choice(a)
```
随机选择一组
```python
a = [1,3,5,7,9,3]
import random
slice = random.sample(a,5)
print slice
```
## append()与extend()
```
>>> a = [i for i in xrange(1,5)]
>>> a.append([6,7])
>>> a
[1, 2, 3, 4, [6, 7]]
>>> a.extend([8,9])
>>> a
[1, 2, 3, 4, [6, 7], 8, 9]
```

## 去掉换行符
```
经常见到的[:-1]就是来去除换行符的.
或者rstrip()或者strip()或者strip('\n')
```

## 时间显示
```
>>> import datetime
>>> dt = datetime.datetime.now()
>>> 'The {1} is {0:%d}, the {2} is {0:%B}, the {3} is {0:%I:%M%p}.'.format(dt, "day", "month", "time")
'The day is 28, the month is April, the time is 02:21PM.'

```

## 全局变量使用
```python
globvar = 0

def set_globvar_to_one():
    global globvar    # 调整,写入全局变量时声明
    globvar = 1

def print_globvar():
    print globvar     # 读取全局变量值的时候不需要声明

set_globvar_to_one()
print_globvar()       # Prints 1
```

## i+=x 和i=i+x 不一样
`+=`调用`__iadd__`方法(不存在就调用 `__add__`方法),`+`直接调用`__add__`方法.
对数字或者字符串这种不可变对象效果一致.对可变对象,不一致.
```python
a = [1, 2, 3]
b = a
b += [1, 2, 3]
print a  #[1, 2, 3, 1, 2, 3]
print b  #[1, 2, 3, 1, 2, 3]
```
对比一下：
```python
a = [1, 2, 3]
b = a
b = b + [1, 2, 3]
print a #[1, 2, 3]
print b #[1, 2, 3, 1, 2, 3]
```

## 随机生成大写字母和数字组成的字符串
例如:

    6U1S75
    4Z4UKK
    U911K4

```python
import string, random
''.join(random.choice(string.ascii_uppercase + string.digits) for x in range(N))
```
## 小tick 为什么多一个逗号
`"hi there %s" % name`
但当`name`恰好是`(1,2,3)`时，会抛出`TypeError`异常.为了保证总是正确的，你必须这么写
`"hi there %s" % (name,)   #一个单变量的元祖`
## 判断 list 为空
```python
if not a:
    print "List is empty"
# 不要用len(a)来判断,len()在[]为空list时返回0,list为None时抛出异常,上面的方法在为空为None时都输出字符串.
```
## 列表解析式在原列表修改
```python
a = [1,2,3,4,5,6]
b = [i for i in a if i%2==0 ]
# 实际是生成一个新列表
```
```python
a = [1,2,3,4,5,6]
a[:] = [i for i in a if i%2==0 ]
# 在原列表上修改
```
## 所谓引用传递
```python
a = [1,2,3,4,5]
def change(t):  # 返回t的隔一个值,还在原来的t的基础上import sys
sys.stderr.write('spam\n')

print >> sys.stderr, 'spam'

from __future__ import print_function
print('spam', file=sys.stderr)使用同一个内存地址
    t[:] = t[::2]  #这里应该还使用t,或者调用t的内置函数.


change(a)
print a
```
## 打印到stderrimport sys
```python
sys.stderr.write('spam\n')

print >> sys.stderr, 'spam'

from __future__ import print_function
print('spam', file=sys.stderr)
```
## 从stdin 读取
```python
import sys
for line in sys.stdin:
    print line
```
```python
import fileinput
for line in fileinput.input():
```

## 闭包里改外部值:
[Python 闭包不支持修改 upvalue，有什么替代的解决方案？](https://www.v2ex.com/t/251731)
一个意思
```python
# python 3.x
def inc():
    x = 0
    def inner():
        nonlocal x
        x += 1
        print(x)
    return inner
inc1 = inc()
inc2 = inc()

inc1()
inc1()
inc1()
inc2()
```
```python
# Python 2.7
def inc():
    x = [0]
    def inner():
        x[0] += 1
        print(x[0])
    return inner
inc1 = inc()
inc2 = inc()

inc1()
inc1()
inc1()
inc2()
```
## utf-8
utf-8可以根据字的第一个字节移位推出长度的
```
0xxxxxxx # 1个字节
110xxxxx 10xxxxxx  # 2个字节
1110xxxx 10xxxxxx 10xxxxxx  # 3个字节
11110xxx 10xxxxxx 10xxxxxx 10xxxxxx  # 4个字节
```
对于汉字 一部分3字节(5W个),一部分4字节(6W个)

## 创建嵌套字典:
[Python中避免在给多维数组赋值之前判断key是否存在的方法](http://cenalulu.github.io/python/default-for-multi-dimensional-dict/)
```python
# import collections
# tree = lambda: collections.defaultdict(tree)
# some_dict = tree()
# some_dict['colours']['favourite'] = "yellow"
# 或者
from collections import defaultdict
some_dict = defaultdict(dict)
some_dict['colours']
some_dict['colours']['favourite'] = "yellow"

import json
print json.dumps(some_dict)
```

## 字典合并:
[Python中合并两个字典](http://panmax.love/2016/Python%E4%B8%AD%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E5%AD%97%E5%85%B8/)
[How can I merge two Python dictionaries in a single expression?](http://stackoverflow.com/questions/38987/how-can-i-merge-two-python-dictionaries-in-a-single-expression?rq=1)
例子:
```python
user = {'name': "Trey", 'website': "http://treyhunner.com"}

defaults = {'name': "Anonymous User", 'page_name': "Profile Page"}
"""
现在想合并两个字典，得到一个新的字典，要求：

- 如果存在重复的键，user字典中的值应覆盖defaults字典中的值；
- defaults和user中的键可以是任意合法的键；
- defaults和user中的值可以是任意值；
- 在创建context字典时，defaults和user的元素不能出现变化；
- 更新context字典时，不能更改defaults或user字典。

即 结果为:
{'website': 'http://treyhunner.com', 'name': 'Trey', 'page_name': 'Profile Page'}
"""
```
```python
user = {'name': "Trey", 'website': "http://treyhunner.com"}

defaults = {'name': "Anonymous User", 'page_name': "Profile Page"}

# context = {**defaults, **user}  # 解包只能在py3 里用 根据参考链接里的测试,在py3里这个效率最好


# from collections import ChainMap
# context = dict(ChainMap(user, defaults))  # Py3 加入的 目的就是干这个的.前一个参数覆盖后一个
# [What is the purpose of collections.ChainMap?](http://stackoverflow.com/questions/23392976/what-is-the-purpose-of-collections-chainmap)
# 在Python 2 里 from ConfigParser import _ChainMap as ChainMap 即在py2里就有了..只是没独立提出来
# 在py2里 可以:
# import itertools
# z = dict(itertools.chain(x.iteritems(), y.iteritems()))

# context = {}
# context.update(defaults)
# context.update(user)

context = defaults.copy()
context.update(user)
print user, '\n', defaults, '\n', context
```

一些其他答案:
```python
def merge_dicts(*dict_args):
    '''
    Given any number of dicts, shallow copy and merge into a new dict,
    precedence goes to key value pairs in latter dicts.
    '''
    result = {}
    for dictionary in dict_args:
        result.update(dictionary)
    return result
# 用法:
z = merge_dicts(a, b, c, d, e, f, g)   # 适用于Py2 & Py3
```
```python
z = dict(x.items() + y.items())  # 在py2里表现正确且正常,但是在py3里无法运行.
# 因为2里是list 3里是dict_items
# 可以 z = dict(list(x.items()) + list(y.items()))   不推荐
```
```python
z = {k: v for d in (x, y) for k, v in d.items()}
z = dict((k, v) for d in (x, y) for k, v in d.items())  # py 2.6 以前
```





## 随机乱序
```python
random.sample(population, k)  # 随机.返回population中的K个作为一个list
random.shuffle(x[, random])  # 对原x进行重拍,采用random函数
```
## 原生输出
```python
print r"\nwoow"
```
## dict 的 key 必须可hashable
```python
dict3 = {[1,2,3]: “uestc”}   # 不可hashable
```

## math
```python
import math
print math.floor(5.5)  #5.0
```

## 输出结果
```python
class Person:
    def __init__(self):
        pass

    def getAge(self):
        print __name__

p = Person()
p.getAge()  # __main__
```
```python
numbers = [1, 2, 3, 4]

numbers.append([5,6,7,8])

print len(numbers)  # 5
print numbers  # [1, 2, 3, 4, [5, 6, 7, 8]]
```
## 整除,除法,取余
最好用
```python
from __future__ import division
# /除法 //整除 不乱
print divmod(177, 10)  # (17, 7)
```

## 嵌套的列表行列变换：
```python
mat = [[1,2,3], [4,5,6], [7,8,9]]
new_mat = [ [row[i] for row in mat] for i in [0,1,2] ] # 嵌套
print(new_mat)  # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
或者
new_mat = map(list, zip(*mat))
```

## 列表删除
```python
lst = [1,2,3,4,5,6,7,8,9]
del lst[2]    # 删除指定索引项
print(lst)  # [1, 2, 4, 5, 6, 7, 8, 9]
del lst[2:5]  # 删除切片
print(lst)  # [1, 2, 7, 8, 9]
del lst[:]    # 删除整个列表
print(lst)  # []
```

## 浅拷贝（shallow copy）&& 深拷贝（deep copy）
只拷贝容器对象,容器内对象是原容器内对象的引用.
```python
a = [1,2,3]
b = list(a)
print id(a), id(b)
for x, y in zip(a, b):       # 子对象身份相同
    print(id(x), id(y))
```
深拷贝:只有`copy`模块中的`deepcopy`函数。
```python
import copy
a = [1, 2, 3]
b = copy.deepcopy(a)
print id(a), id(b)  # 140298869557512 140298869519856
for x, y in zip(a, b):       # 子对象身份相同,2333 因为1,2,3是不可变对象
    print(id(x), id(y))
    # (34189656, 34189656)
    # (34189632, 34189632)
    # (34189608, 34189608)


import copy
a = [[1],[2],[3]]
b = copy.deepcopy(a)
print id(a), id(b)  # 139930602280504 139930602280288
for x, y in zip(a, b):       # 子对象身份不同
    print(id(x), id(y))
    # (139930602147208, 139930602280576)
    # (139930602146056, 139930602280720)
    # (139930602280216, 139930602280792)
```

## 计算概率:
```python
import random
def random_pick(seq, probabilities):
    x = random.uniform(0, 1)
    cumulative_probability = 0.0
    for item, item_probability in zip(seq, probabilities):
            cumulative_probability += item_probability
            if x < cumulative_probability:
                break
    return item


for i in range(15):
    print random_pick("abc",[0.1,0.3,0.6])
```

## 字典(组成的)列表的排序
[How do I sort a list of dictionaries by values of the dictionary in Python?](http://stackoverflow.com/questions/72899/how-do-i-sort-a-list-of-dictionaries-by-values-of-the-dictionary-in-python)
```python
list_to_be_sorted = [{'name':'Homer', 'age':10, 'sales':100}, {'name':'H1omer', 'age':10, 'sales':200}, {'name':'H2omer', 'age':20, 'sales':300}, {'name':'H3omer', 'age':30, 'sales':300}]
newlist = sorted(list_to_be_sorted, key=lambda k: (k['age'], k['sales']))  # 先按age排序,age相同按sales排序
print newlist
# [{'age': 10, 'name': 'Homer', 'sales': 100}, {'age': 10, 'name': 'H1omer', 'sales': 200}, {'age': 20, 'name': 'H2omer', 'sales': 300}, {'age': 30, 'name': 'H3omer', 'sales': 300}]
# 或者
from operator import itemgetter
newlist = sorted(list_to_be_sorted, key=itemgetter('name'))
```

## 浮点数
`float`精度有限..
```python
print 3 * 0.1 == 0.3  # False
print round(2.675, 2)  # 2.67
```
更精确采用`Decimal`.
```python
from decimal import Decimal
print Decimal('0.1') * 3 == Decimal('0.3')  # True
Decimal('2.675').quantize(Decimal('.01'))  # 2.68
```
## 运行时(后期)绑定
[Lexical closures in Python](http://stackoverflow.com/questions/233673/lexical-closures-in-python)
[Gotcha: Python, scoping, and closures](https://eev.ee/blog/2011/04/24/gotcha-python-scoping-closures/)
```python
flist = []

for i in xrange(3):
    def func(x): return x * i
    flist.append(func)

for f in flist:
    print f(2)  # 4,4,4
```
结果并不如预期.
```python
flist = []

for i in xrange(3):
    def func(x,i=i): return x * i
    flist.append(func)

for f in flist:
    print f(2)  # 0,2,4
```
或者
```python
flist = []

for i in xrange(3):
    def F(i):
        def func(x): return x * i
        return func
    flist.append(F(i))

for f in flist:
    print f(2)  # 4,4,4
```
## 插入自定义包位置
```python
import sys
sys.path.insert(0,'path/to/your/packge')
```
## 'abcdefghigklmnopqrstuvwxyz'切成 ['abc','def','ghi'…]
[v2ex](https://www.v2ex.com/t/271802)
[How do you split a list into evenly sized chunks in Python?](http://stackoverflow.com/questions/312443/how-do-you-split-a-list-into-evenly-sized-chunks-in-python)
[What is the most “pythonic” way to iterate over a list in chunks?](http://stackoverflow.com/questions/434287/what-is-the-most-pythonic-way-to-iterate-over-a-list-in-chunks)
```python
['abcdefghigklmnopqrstuvwxyz'[i:i+3]for i in xrange(0,len('abcdefghigklmnopqrstuvwxyz'),3)]

>>> import re 
>>> re.findall('.{3}','abcdefghigklmnopqrstuvwxyz')  # 最后的'yz'会被丢弃

>>>re.findall('.{1,3}','abcdefghigklmnopqrstuvwxyz')  # 这样最后的'yz'不会被丢弃   
# 这里的逻辑在于正则中,不定次数的限定符总是尽可能的多匹配   

>>>from more_itertools import chunked  # more_itertools 需要安装
>>>list(chunked('abcdefghigklmnopqrstuvwxyz', 3))

# 用函数也可以
def chunks(l, n):
    """Yield successive n-sized chunks from l."""
    for i in range(0, len(l), n):  # xrange in python 2
        yield l[i:i+n]     

import pprint
pprint.pprint(list(chunks('abcdefghigklmnopqrstuvwxyz', 3)))
```