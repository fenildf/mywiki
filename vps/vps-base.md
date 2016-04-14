# VPS 基础
　
## 注册和购买
这个我不用说,网上全是这些方面的东西.鼓励你用他的链接购买呗.大家都能有优惠码可拿.好吧.我也发一个我的注册链接好了:
[]().通过这个链接来注册,你有优惠拿,我也有优惠拿.双赢.

## 第一次使用.
购买成功后,注意你的注册邮箱.会收到Host1Plus给你的SSH 连接地址与密码.
### SSH使用
[Ubuntu SSH使用](http://vv15.pbhz.com/2010/11/ubuntu-ssh-vps/)

[阮一峰的网络日志 : SSH 原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html) 阮老师的文章强烈推荐.

[SSH Tricks](https://serversforhackers.com/ssh-tricks)

会给你发一封邮件,格式如下:

    Hostname: aaaaa
    Main IP: 111.111.111.111
    Root Password: xxxxxxxx

#### 1.登陆
这就是有用的部分,执行下面的命令登陆:
```bash
ssh root@111.111.111.111
```
然后输入上面给你的密码.就可以进入VPS了.之后的操作与你本地 Ubuntu 操作一致.

#### 2.改密码加普通用户...提高安全性
创建管理员账户并设定密码
```bash
root@yourvps:~# useradd -s /bin/bash -mr <username>  
root@yourvps:~# adduser <username> sudo
root@yourvps:~# passwd <username>
```
现在可以断开SSH 以`ssh <username>@IP地址 `重新登陆SSH(不过你的密码很弱的话还是容易弄巧成拙= = )
#### 3.修改SSH端口
```bash
root@yourvps:~# vim /etc/ssh/sshd_config
# 将port = 22 改为一个大于1024小于65535的数字
```
重启SSH服务(本次服务还可用,下次需要从新端口连接)
```bash
root@yourvps:~# sudo service ssh restart
```
#### 4.切换到刚刚新建的普通用户
```bash
root@yourvps:~# sudo su -l <username>
<username>@yourvps:~$ 
```
#### 5.生成公钥密钥
```bash
<username>@yourvps:~$ ssh-keygen -t rsa
```
#### 6.SCP 下载私钥
```bash
scp -r -P <设定的端口> <username>@<vps-ip>:<远程文件位置> <本地存放位置>
# 我的示例:
 scp -r -P 24635 root@191.101.13.248:/home/abirdcfly/.ssh ~/sshssh
```

### 图形化 / VPS 面板
如果你不是想折腾,或者是新手,可以装一个图形面板.大概类似下面这种:
[[/vps/pic/vpsmb.jpg]]

### 我的配置
```sh
# 安装 zsh
sudo apt-get install zsh
# 安装 oh-my-zsh 插件
cd ~/.
wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

### 教程
[VPS 教程系列：Ubuntu + LAMP + Nginx + WordPress 配置使用](https://ttt.tt/92/)

[VPS是什么？VPS怎么用？VPS教程（适合新手入门）](http://vv15.pbhz.com/2011/03/vps/)

[vps新手入门全攻略：基础知识，选购，配置环境，日常管理及精品vps主机商推荐](http://www.path8.net/tn/archives/5370)

[VPS环境搭建详解 (Virtualenv+Gunicorn+Supervisor+Nginx)](http://beiyuu.com/vps-config-python-vitrualenv-flask-gunicorn-supervisor-nginx/) Python环境搭建.实用.

[VPS管理百科](http://www.bootf.com/)
