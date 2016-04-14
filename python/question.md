这些问题可能需要读源码了.    

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

# 解释
```python
lista = [(0,'s'),(1,'app'),(2,'77822'),(3,'id')]
lista.sort(key=lambda x:len(x[1]))
# 为什么 lambda x:len(x[1]) x 会=lista.items 呢?(没有这个方法..就是这个意思)?
```
