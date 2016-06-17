这些问题可能需要读源码了.
# list 修改
```
a = []
for i in a:
    if i xxxx:
        a.remove(i)
print a

```
# 正则非贪婪匹配失败问题
```
>>> re.findall(r"a(\d+)b", "a23b")
        ['23']
>>> re.findall(r"a(\d+?)b", "a23b")
        ['23']
```
我想让第二个匹配2如何做?

# 编码遇到的问题
[2to3编码](/223)
    
# requests 如何处理参数值为空的url?
```
arg = {"key1":"value1", "key2":"value2"}
requests.get("http://www.baidu.com", params=arg)  # http://www.baidu.com?key1=value1&key2=value2
```
如何处理`http://www.baidu.com?key3&key1=value1&key2=value2`?
当然可以字符串拼接,如何更pythonic?

# remove是否实现取巧了,并没有实时对比真实值?用到了index?
```
>>> a = [i for i in xrange(1,100)]
>>> for i in a:
...     a.remove(i)
... 
>>> a
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48, 50, 52, 54, 56, 58, 60, 62, 64, 66, 68, 70, 72, 74, 76, 78, 80, 82, 84, 86, 88, 90, 92, 94, 96, 98]
```
如果非要这么做(在循环里删除元素)的话,可以用`a[:]`.或者倒着删除(见下)但是上面的原因呢?
问题可能出在`for`上,因为`a`一直在变化,`for`还是按原来的索引走的.
```
>>> for i in range(len(a)-1, -1, -1):
...     del a[i]
... 
>>> a
[]

```

# blog 里那个257的问题

# 元祖逗号
"hi there %s" % (name,)   # supply the single argument as a single-item tuple
是当做元祖了吗?两个地方的说法不一样.

# 解释:
```python
a = [1,2,3]
a[1:1] = [7]
print a
# [1,7,2,3]
```

# 解释:
```python
>>> u'hah' == 'hah'
True
>>> u'哈哈' == '哈哈'
__main__:1: UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode - interpreting them as being unequal
False
```
另外,print的原理是什么?不是显示对象的`__str__()`?但是Unicode对象是没有`__str__()`的,所以是怎么显示的?
```python
>>> b = u'哈哈'
>>> b.__repr__()
"u'\\u54c8\\u54c8'"
>>> b.__str__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
>>> type(b)
<type 'unicode'>
>>> print b
哈哈

```

# lambda 问题
```python
lista = [(0,'s'),(1,'app'),(2,'77822'),(3,'id')]
lista.sort(key=lambda x:len(x[1]))
# 为什么 lambda x:len(x[1]) x 会=lista.items 呢?(没有这个方法..就是这个意思)?
```

## 阅读材料

[Zhihu:Lambda 表达式有何用处？如何使用？](https://www.zhihu.com/question/20125256)   
下面的链接只是说明Lambda的用法....     
[深入理解Lambda](http://blog.csdn.net/lemon_tree12138/article/details/50774827)    
[Python天天美味(35) - 细品lambda](http://www.cnblogs.com/coderzh/archive/2010/04/30/python-cookbook-lambda.html)    
[我的疑问的出处](https://www.v2ex.com/t/270878#reply9)
