# Mac上安装mysql教程

####安装mysql
下载完成后点击安装mysql,一直点确认。安装完成后启动mysql

进入系统偏好设置，最下边一行，找到mysql打开，点击"Start MySQL Server"，启动mysql

* 打开终端,先运行下面两条命令

```
alias mysql=/usr/local/mysql/bin/mysql
alias mysqladmin=/usr/local/mysql/bin/mysqladmin
```

这两条命令是为了方便直接打开 iTerm 就可以运行mysql命令，而不是必须进入mysql安装目录才能运行。

* 重置密码：

```
mysqladmin -u root -p password newpass
```

newpass 是你的新密码,然后输入临时密码,密码修改成功

* 新密码登录

```
mysql -u root -p
```

####环境变量配置

* 打开终端,输入：

```
cd ~
```
会进入~文件夹

* 然后输入：

```
touch .bash_profile
```

回车执行后，

* 再输入：

```
open -e .bash_profile
```

会在TextEdit中打开这个文件（如果以前没有配置过环境变量，那么这应该是一个空白文档）。如果有内容，请在结束符前输入，如果没有内容，请直接输入如下语句：

```
export PATH=${PATH}:/usr/local/mysql/bin
```

然后，保存，退出TextEdit（一定是退出），关闭终端并退出。
