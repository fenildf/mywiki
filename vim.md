# 模式
vim 有 3 种模式.命令行模式,插入模式,选择模式.
# 配置
## 自动换行
[自动换行](http://979137.com/thread-45-1-1.html)    
配置每行超过 n 个字的时候 vim 自动加上换行符    
:set textwidth=n （n代表数字）    

设置自动换行    
:set wrap 设置自动折行    
    
设置不自动换行    
:set nowrap   

# 搜索    
/pattern 从光标向下搜索     
?pattern 从光标向上搜索    
/(空格)pattern(空格) 向下搜索包含pattern的所有序列(例如搜索place 会出现displace....)?同理    
/^pattern 只搜索行首出现pattern的.pattern$指行尾 (\转义)    
找到后,n向下一个结果,N上一个结果.    