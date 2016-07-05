# linux 工具类
## zsh/oh-my-zsh
[关掉 zsh 对特定命令的自动纠错](http://ror.logdown.com/posts/2013/11/29/turn-off-the-zsh-command-specific-automatic-error-correction)

## tmux

操作	|快捷键
--------|----
查看/切换session	|prefix s
离开Session	|prefix d
重命名当前Session	|prefix $

Window相关操作

操作	|快捷键
--------|-------
新建窗口	|prefix c
切换到上一个活动的窗口	|prefix space
关闭一个窗口	|prefix &
使用窗口号切换	|prefix 窗口号

Pane相关操作

操作|	快捷键
------|------
切换到下一个窗格|	prefix o
查看所有窗格的编号|	prefix q
垂直拆分出一个新窗格|	prefix “
水平拆分出一个新窗格|	prefix %
暂时把一个窗体放到最大|	prefix z

\<\<tmux productive mouse-free development\>\> 可在[此](http://uploads.mitechie.com/books/tmux_p1_1.pdf)下载到pdf版本[对应中文版](http://www.kancloud.cn/kancloud/tmux) 基本下面的东西没脱离这本书

[A tmux Primer](https://danielmiessler.com/study/tmux/)

[youtube : Basic tmux Tutorial - Windows, Panes, and Sessions over SSH](https://www.youtube.com/watch?v=BHhA_ZKjyxo) youtube 上一个10分钟的入门介绍 

[tankywoo关于tmux的wiki](http://wiki.tankywoo.com/tool/tmux.html)

[Tmux - Linux从业者必备利器](http://cenalulu.github.io/linux/tmux/)

[Linux下终端利器tmux](http://kumu-linux.github.io/blog/2013/08/06/tmux/)

记得用 tmuxinator 方便配置     
[tmux最佳伴侣：tmuxinator](http://zuyunfei.com/2013/08/09/tmuxinator-best-mate-of-tmux/)     
[Specify pane percentage in tmuxinator project](http://stackoverflow.com/questions/9812000/specify-pane-percentage-in-tmuxinator-project/9976282#9976282)     

配置时的pane大小配置:[http://stackoverflow.com/questions/9812000/specify-pane-percentage-in-tmuxinator-project/9976282#9976282](stackoverflow)
### 问题
[undefined method `shellescape' 解决方法](https://github.com/capistrano/capistrano/issues/360)

## vim
[自动换行](http://979137.com/thread-45-1-1.html)    
配置每行超过 n 个字的时候 vim 自动加上换行符    
:set textwidth=n （n代表数字）    

设置自动换行    
:set wrap 设置自动折行    
    
设置不自动换行    
:set nowrap   

### 搜索    
/pattern 从光标向下搜索     
?pattern 从光标向上搜索    
/(空格)pattern(空格) 向下搜索包含pattern的所有序列(例如搜索place 会出现displace....)?同理    
/^pattern 只搜索行首出现pattern的.pattern$指行尾 (\转义)    
找到后,n向下一个结果,N上一个结果.    
    
# ssh
## scp 上传/下载
```bash
scp 本地文件 username@hostIP:上传目录
scp username@hostIP:要下载文件目录   本地存放目录
```
-P port 注意是大写的P, port是指定数据传输用到的端口号.
[更多参数信息](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/scp.html)

## 公钥私钥配置
[利用 ssh 的用户配置文件 config 管理 ssh 会话](http://dhq.me/use-ssh-config-manage-ssh-session)
