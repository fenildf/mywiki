# terminal 常识

## terminal快捷键

|按键|	作用|
|---|---|
|Ctrl+c  |终止|
|Ctrl+d	|键盘输入结束或退出终端|
|Ctrl+s	|暂定当前程序，暂停后按下任意键恢复运行|
|Ctrl+z	|将当前程序放到后台运行，恢复到前台为命令fg|
|Ctrl+a	|将光标移至输入行头，相当于Home键|
|Ctrl+e	|将光标移至输入行末，相当于End键|
|Ctrl+k	|删除从光标所在位置到行末|
|Alt+Backspace	|向前删除一个单词|
|Shift+PgUp	|将终端显示向上滚动|
|Shift+PgDn	|将终端显示向下滚动|

|快捷键|	 描述|
|---|---|
|Ctrl+Alt+T	| 启动终端|
|F11|	 全屏切换|
|Ctrl+Shift+C|	 复制|
|Ctrl+Shift+V	| 粘贴|
|Ctrl+Alt+Fn|	 切换到字符界面（n=1...6） 如果需要切换回图形界面，需要使用 Ctrl+Alt+F7 或 Alt+F7|


## ternimal 通配符

|字符|	含义|
|---|---|
|*	|匹配 0 或多个字符|
|?	|匹配任意一个字符|
|[list]	|匹配 list 中的任意单一字符|
|[!list]	|匹配 除list 中的任意单一字符以外的字符|
|[c1-c2]	|匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]|
|{string1,string2,...}	|匹配 sring1 或 string2 (或更多)其一字符串|
|{c2..c2}	|匹配 c1-c2 中全部字符 如{1..10}|

例子:
``` bash
# 创建 love_1_linux.txt ... 到 love_10_linux.txt
touch love_{1..10}_linux.txt
```
## 命令执行顺序

|字符|	含义|
|---|---|
|;	|顺序执行|
|&&	|上一个命令返回0时执行(echo $? 显示上一条命令返回值)|
|\|\|	|上一个命令返回不为0时执行|
|\||匿名管道|

## 数据流重定向
标准输出(/dev/stdout)本身也是一个文件.

|文件描述符	|设备文件	|说明|
|---|-------------|------------|
|0	|/dev/stdin	|标准输入|
|1	|/dev/stdout	|标准输出|
|2	|/dev/stderr	|标准错误|

```bash
# 将标准错误重定向到标准输出，再将标准输出重定向到文件，注意要将重定向到文件写到前面
cat Documents/test.c hello.c >somefile  2>&1
# 或者只用bash提供的特殊的重定向符号"&"将标准错误和标准输出同时重定向到文件
cat Documents/test.c hello.c &>somefilehell
```
## apt 包管理

### apt-get

|工具|	说明|
|---|---|
|install	|其后加上软件包名，用于安装一个软件包|
|update	|从软件源镜像服务器上下载/更新用于更新本地软件源的软件包列表|
|upgrade	|升级本地可更新的全部软件包，但存在依赖问题时将不会升级，通常会在更新之前执行一次update|
|dist-upgrade|	解决依赖关系并升级(存在一定危险性)|
|remove	|移除已安装的软件包，包括与被移除软件包有依赖关系的软件包，但不包含软件包的配置文件|
|autoremove|	移除之前被其他软件包依赖，但现在不再被使用的软件包|
|purge	|与remove相同，但会完全移除软件包，包含其配置文件|
|clean	|移除下载到本地的已经安装的软件包，默认保存在/var/cache/apt/archives/|
|autoclean	|移除已安装的软件的旧版本软件包|

### 搜索
apt-cache 命令则是针对本地数据进行相关操作的工具

```bash
sudo apt-cache search softname
```