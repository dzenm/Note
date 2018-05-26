##教程地址：https://yalv.me/aria2/



### 一、安装Aria2

下载地址：https://yalv.me/download/aria2/aria2-1.19.3-osx-darwin.dmg

下载后点击&nbsp; `aria2.pkg`&nbsp;，按照步骤一步步安装，中间需要输入用户密码。





### 二、配置Aria2

1. 下载配置文件：https://yalv.me/download/aria2/aria2.conf
2. 用文本编辑器打开该配置文件：aria2.conf
3. 修改第二行  `dir=/Users/XXX/Downloads`  ，将XXX换成你的用户名，如下图

<img src="https://source.yalv.me/wp-content/uploads/2017/10/aria2-2.png?imageslim|imageView2/2/w/1200/interlace/1" width="100" height="100"/>

4. 打开Terminal（终端）

5. 输入：`mkdir ~/.aria2`  ，即在用户目录下新建文件夹  `.aria2`

6. 将刚才配置好的文件放在该目录下

   * 方法一：使用命令行操作，在刚才的基础上，输入  `cd .aria2`  ，即可进入到该目录。然后使用移动命令  `mv aria2.conf路径 aria2.conf  `  ，aria2.conf路径可以直接将该文件拖入到终端

   

   * 方法二：打开显示隐藏路径的命令：  `defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder`  ，取消隐藏路径的命令： `defaults write com.apple.finder AppleShowAllFiles -bool false ; killall Finder`  。macOS Sierra使用快捷键 `Command + Shift + .`  



7. 下载Aria2应用文件：https://yalv.me/download/aria2/aria2c.zip

8. 将下载好的&nbsp; `Aria2C`  解压然后拖到Application文件夹下

9. 运行Aria2，在Terminal中输入  `aria2c`  

   若提示  `command not found  `  ，在终端下输入  `/usr/local/bin`   ， `sudo ln -s ../aria2/bin/aria2c aria2c `   就行了

10. 检查Aria2是否正在运行，在Terminal中输入：`ps aux|grep aria2c`  ，正常如图显示或直接下载模式

    ![aria2-3](https://source.yalv.me/wp-content/uploads/2017/10/aria2-3.png?imageslim|imageView2/2/w/1200/interlace/1)

    每次下载完成后，生成与文件同名的的.aria2后缀文件。针对这个问题，添加一条配置可以解决。

    在&nbsp;&nbsp;`~/.aria2/aria2.conf`&nbsp;&nbsp;配置文件下面，加上一条命令&nbsp;&nbsp;`on-download-complete="rm -f "$3.aria2"`&nbsp;&nbsp;并将&nbsp;&nbsp;`force-save`&nbsp;&nbsp;更改&nbsp;&nbsp;`force-save=false`&nbsp;&nbsp;
    然后重启 aria2 命令行&nbsp;&nbsp;`aria2c restart`&nbsp;&nbsp;
    如果还存在该问题，重启电脑即可。

11. 进入UI页面操作模式：https://ziahamza.github.io/webui-aria2/

12. 设置连接

    *  `设置-连接设置-主机`&nbsp; 更改为&nbsp; `localhost`
    * `端口`&nbsp;&nbsp;改为&nbsp;&nbsp;`6800`
    * 取消勾选&nbsp;`SSL/TLS 加密`

    ![aria2-setting](https://source.yalv.me/wp-content/uploads/2017/10/aria2-setting.png?imageslim|imageView2/2/w/1200/interlace/1)

13. 将百度网盘任务添加到Aria2：在谷歌应用商店搜索插  `BaiduExporter`  添加即可

14. 如果开启Adblock Plus等插件，需要关闭

