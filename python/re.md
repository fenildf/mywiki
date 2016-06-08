# 正则表达式

## 通用表格

|字符|含义|
|:----:|-------|
|转义|. ^ $ * + ? { } [ ] \ | ( )|
|||
|\d |数字,相当于 [0-9]。|
|\D |非非数字字符,相当于 [^0-9]。|
|\s |空白字符,相当于 [ \t\r\n\f\v]。|
|\S |非空白字符。|
|\w |字母或数字,相当于 [0-9a-zA-Z]。|
|\W |非字母或数字。|
|. |任意字符。|
||| 或。|
|^ |非,或者开始位置标记。|
|$ |结束位置标记。|
|\b |单词边界。|
|\B |非非单词边界。|
|||
|重复:||
|* |0 或任意多个字符。添加 ? 后缀避免贪婪匹配。|
|? |0 或一个字符。|
|+ |1 或多个字符。|
|{n}| n 个字符。|
|{n,}| 最少 n 个字符。|
|{,m} |最多 m 个字符。|
|{n, m}| n 到 m 个字符。|
|||
|编译标志:|可以直接在表达式前部添加 "(?iLmsux)" 标志|
|s| 单行。"." 匹配包括换行行符在内的所有字符。|
|i |忽略大小写。|
|L |让 \w 匹配本地字符,对中文支持不好。|
|m |多行。|
|x |忽略多余的空白字符。让表达式更易阅读。|
|u |Unicode。|

## Python re 模块
### re 函数
- `match()`: 匹配字符串开始位置。返回 `MatchObject` 对象
- `search()`: 扫描字符串,找到第一一个位置。返回 `MatchObject` 对象
- `findall()`: 找到全部匹配,以**列表**返回。
- `finditer()`: 找到全部匹配,以**迭代器**返回。返回 `MatchObject` 对象
`match `和` search` 仅匹配一一次,匹配不到返回 `None`。

上原文例子
```python
>>> import re
>>> s = "12abc345ab"
>>> m = re.match(r"\d+", s)
>>> m.group(), m.span()
('12', (0, 2))
>>> m = re.match(r"\d{3,}", s)
>>> m is None
True
>>> m = re.search(r"\d{3,}", s)
>>> m.group(), m.span()
('345', (5, 8))
>>> m = re.search(r"\d+", s)
>>> m.group(), m.span()
('12', (0, 2))
```
### MatchObject方法:
- `group():` 返回匹配的完整字符串。
- `start(): `匹配的开始位置。
- `end():` 匹配的结束位置。
- `span()`: 包含起始、结束位置的元组。
- `groups()`: 返回分组信息。
- `groupdict()`: 返回命名分组信息。

见例子:
```python
>>> m = re.match(r"(\d+)(?P<letter>[abc]+)", s)
>>> m.group()
'12abc'
>>> m.start()
0
>>> m.end()
5
>>> m.span()
(0, 5)
>>> m.groups()
('12', 'abc')
>>> m.groupdict()
{'letter': 'abc'}
```
`group()`、`start()`、`end() `和 `span()` 都能接收分组序号。序号 0 表示整体匹配结果。
```python
>>> m.group(0)
'12abc'
>>> m.group(1)
'12'
>>> m.group(2)
'abc'
>>> m.group(1,2)
('12', 'abc')
>>> m.group(0,1,2)
('12abc', '12', 'abc')

>>> m.start(0), m.end(0)
(0, 5)
>>> m.start(1), m.end(1)
(0, 2)
>>> m.start(2), m.end(2)
(2, 5)
>>> m.span(0)
(0, 5)
>>> m.span(1)
(0, 2)
>>> m.span(2)
(2, 5)
```
### 编译标志

|编译标志:|可以直接在表达式前部添加 "(?iLmsux)" 标志|
|:-----:|------------------------------|
|s| 单行。"." 匹配包括换行行符在内的所有字符。|
|i |忽略大小写。|
|L |让 \w 匹配本地字符,对中文支持不好。|
|m |多行。|
|x |忽略多余的空白字符。让表达式更易阅读。|
|u |Unicode。|


```python
>>> re.findall(r"[a-z]+", "%123Abc%45xyz&")
['bc', 'xyz']
>>> re.findall(r"[a-z]+", "%123Abc%45xyz&", re.I)
['Abc', 'xyz']
>>> re.findall(r"(?i)[a-z]+", "%123Abc%45xyz&")
['Abc', 'xyz']

>>> pattern = r"""
... (\d+) # number
... ([a-z]+) # letter
... """
>>> re.findall(pattern, "%123Abc\n%45xyz&", re.I | re.S | re.X)
[('123', 'Abc'), ('45', 'xyz')]
```
### 组操作
- 命名组:`(?P<name>...)`
```python
>>> for m in re.finditer(r"(?P<number>\d+)(?P<letter>[a-z]+)", "%123Abc%45xyz&", re.I):
...         print m.groupdict()
...

{'number': '123', 'letter': 'Abc'}
{'number': '45', 'letter': 'xyz'}
```
- 无捕获组:`(?:...)`,作为匹配条件,但不返回。
```python
>>> for m in re.finditer(r"(?:\d+)([a-z]+)", "%123Abc%45xyz&", re.I):
...         print m.groups()
...
('Abc',)
('xyz',)
```
- 反向引用用:`\<number> `或 `(?P=name)`,引用前面的组。

```python
>>> for m in re.finditer(r"<a>\w+</a>", "%<a>123Abc</a>%<b>45xyz</b>&"):
...         print m.group()
...
<a>123Abc</a>

>>> for m in re.finditer(r"<(\w)>\w+</(\1)>", "%<a>123Abc</a>%<b>45xyz</b>&"):
...         print m.group()
...
<a>123Abc</a>
<b>45xyz</b>

>>> for m in re.finditer(r"<(?P<tag>\w)>\w+</(?P=tag)>", "%<a>123Abc</a>%<b>45xyz</
b>&"):
...          print m.group()
...
<a>123Abc</a>
<b>45xyz</b>
```

正声明 `(?=...)`:组内容必须出现在右侧,不返回。
负声明 `(?!...)`:组内容不能出现在右侧,不返回。
反向正声明 `(?<=)`:组内容必须出现在左侧,不返回。
反向负声明` (?<!)`:组内容不能出现在左侧,不返回。

```python
>>> for m in re.finditer(r"\d+(?=[ab])", "%123Abc%45xyz%780b&", re.I):
...         print m.group()
...
123
780

>>> for m in re.finditer(r"(?<!\d)[a-z]{3,}", "%123Abc%45xyz%byse&", re.I):
...         print m.group()
...

byse
```
### 修改
- `split`: 用 `pattern `做分隔符切割字符串。如果用 "(pattern)",那么分隔符也会返回。

```python
>>> re.split(r"\W", "abc,123,x")
['abc', '123', 'x']
>>> re.split(r"(\W)", "abc,123,x")
['abc', ',', '123', ',', 'x']
```
- `sub`: 替换子子串。可指定替换次数。
```python
>>> re.sub(r"[a-z]+", "*", "abc,123,x")
'*,123,*'
142>>> re.sub(r"[a-z]+", "*", "abc,123,x", 1)
'*,123,x'
```
- `subn()` 和 `sub()` 差不多,不过返回 "(新字符串,替换次数)"。
```python
>>> re.subn(r"[a-z]+", "*", "abc,123,x")
('*,123,*', 2)
```
还可以将替换字符串改成函数,以便替换成不同的结果。
```python
>>> def repl(m):
... print m.group()
... return "*" * len(m.group())
...
>>> re.subn(r"[a-z]+", repl, "abc,123,x")
abc
x
('***,123,*', 2)
```

## 参考资料和链接
[python 正则表达式操作指南](http://wiki.ubuntu.org.cn/Python正则表达式操作指南)

\<\<Python 学习手册 第二版\>\> Python re库代码例子

\<\<精通正则表达式\>\>

[正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex.htm)

[进阶正则表达式](http://www.cnblogs.com/hustskyking/p/how-regular-expressions-work.html)

[深入浅出之正则表达式（一）](http://dragon.cnblogs.com/archive/2006/05/08/394078.html)

[深入浅出之正则表达式（二）](http://dragon.cnblogs.com/archive/2006/05/09/394923.html)

[regex](http://regex.alf.nu/) : 在线挑战正则表达式

[上面的答案](http://felixc.at/regex.alf.nu)

[reddit上的一些探讨](https://www.reddit.com/r/programming/comments/1tb0go/regex_golf/)
