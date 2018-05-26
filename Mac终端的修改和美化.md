### 一、安装iTerm2

官网： http://iterm2.com  下载安装即可。

iTerm2拥有语法高亮，命令行tab补全，自动提示符，显示Git仓库状态等功能





----



### 二、配置iTerm2

1. 将iTerm2设置为默认终端：

（菜单栏）iTerm2 —> Make iTerm2 Default Term



2. 打开偏好设置Preference，选中Keys，Hotkey下的Show/hide iTerm2 with a system — wide hotkey，将热键设置为Command + . ，即可使用Command + .全局热键来打开或关闭iTerm2窗口。





----



### 三、配色方案

​	我选用的是 [solarized](http://ethanschoonover.com/solarized)，效果还不错。点开官网，下载，解压，然后打开 iTerm2 下的偏好设置 preference ，点开 profiles 下的colors 选项，点击右下角的 Color Presets 选项，选择import ，导入解压到的 solarized 文件下的Solarized Dark.itermcolors**。**





---



### 四、安装oh-my-zsh

* Mac OSX默认使用bash shell，在耍命令的时候，文件的显示没有带颜色。下面安装 zsh shell+Oh My Zsh 主题，bash shell默认读取的是当前用户下的 .bash_profile 文件，而 zsh shell 默认读取的是当前用户下的 .zshrc 文件



#####1、Oh My Zsh官网地址：http://ohmyz.sh/

##### 2、切换到zsh

Mac OS X默认使用的是bash shell ，使用shell命令切换 

> cash -s /bin/zsh

##### 3、安装zsh

> Github链接：https://github.com/robbyrussell/oh-my-zsh

使用 crul安装

> sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

使用wget安装

> sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

#####4、安装成功后即可显示 oh-my-zsh





----



### 五、设置oh-my-zsh主题



#### 1、设置终端和ls可配色

安装成功后，打开终端iTerm2，然后输入 `.bash_profile` 

添加如下代码

> \#enables colorin the terminal bash shell export export CLICOLOR=1  
>
> 
>
> \#setsup thecolor scheme for list export export LSCOLORS=gxfxcxdxbxegedabagacad   
>
> 
>
> \#sets up theprompt color (currently a green similar to linux terminal) export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\]\$     ' 
>
> 
>
> \#enables colorfor iTerm export TERM=xterm-256color



#### 2、设置vim可配色

继续在终端下输入 `vim .vimrc`

设置如下代码

> syntax on 
>
> set number
>
> set ruler



#### 3、设置Color主题

进入 https://iterm2colorschemes.com/ 下载主题

下载好后打开 iTerm2，打开  `iTerm2 -> Preferences -> Profiles -> Color `  然后选择  `Color Presets -> import`  

解压下载好的主题，将下载好的主题目录下 `schemes` 目录下导入需要设置的主题，导入之后将主题设置成需要的主题。

 



----



###六、配置Oh My Zsh



 #### 1、打开Oh My Zsh配置文件

> vi ~/.zshrc

#### 2、找到主题配置

> ZSH_THEME="amuse"

#### 3、选择Oh My Zsh主题

主题网站：https://github.com/robbyrussell/oh-my-zsh/wiki/Themes

将上述找到的主题配置更改为选择主题名字

* 例如：ZSH_THEME="agnoster"

然后按ESC键后， 输入  `:wq`   即可保存退出

####4、关闭终端，重新打开，即可显示效果





----



### 七、Oh My Zsh的升级

输入如下命令，回车即可升级

> upgrade_oh_my_zsh





----



### 八、Oh My Zsh的卸载

输入如下命令，回车即可卸载

> uninstall_oh_my_zsh





----



### 九、Oh My Zsh设置语法高亮

#### 1、安装HomeBrew，

命令复制到终端即可安装

> ```
> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
> ```



卸载homeview

> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
>
>  
>
> cd `brew –prefix`
>
>  rm -rf Cellar
>
>  brew prune  
>
> rm `git ls-files`  
>
> rm -rf Library .git .gitignore bin/brew 
>
> rm  -rf README.md share/man/man1/brew 
>
> rm -rf Library/Homebrew Library/Aliases 
>
> rm -rf Library/Formula Library/Contributions 
>
> rm -rf ~/Library/Caches/Homebrew



homebrew常用命令

>//查看brew的帮助 brew –help 
>
>//安装软件 brew install git 
>
>//卸载软件 brew uninstall git 
>
>//搜索软件 brew search git 
>
>//显示已经安装软件列表 brew list 
>
>//更新软件，把所有的Formula目录更新，并且会对本机已经安装并有更新的软件用*标明。 brew update 
>
>//更新某具体软件 brew upgrade git 
>
>//查看软件信息 brew [info | home][FORMULA...] 
>
>//删除程序，和upgrade一样，单个软件删除和所有程序老版删除。 brew cleanup git  brew cleanup 
>
>//查看那些已安装的程序需要更新 brew outdated
>
>
>
> //其它Homebrew指令:
>
>brew list   //—列出已安装的软件 
>
>brew update  //—更新Homebrew 
>
>brew home *   //—用浏览器打开 
>
>brew info *   //—显示软件内容信息 
>
>brew deps *    //—显示包依赖 
>
>brew server *  //—启动web服务器，可以通过浏览器访问 http://localhost:4567/ 来同网页来管理包 
>
>brew -h brew   //—帮助



#### 2、安装zsh-syntax-highlighting

通过终端下的命令行安装

>  brew install zsh-syntax-highlighting



#### 3、复制这个存储库并获取脚本

> git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
>
> echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc



#### 4、插入一行配置

> source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh



#### 5、更新配置

> source ~/.zshrc 





---



### 注 1 ：iTerm2快捷键

> ⌘ + 数字 ： 各 tab 标签切换 
>
> ⌘ + f ： 查找 ，所查找的内容会被自动复制 ,输入查找的部分字符，找到匹配的值按tab，即可复制 
>
> ⌘ + d ： 横着分屏 
>
> ⌘ + shift + d ： 竖着分屏 
>
> ⌘ + r = clear ： 换到新一屏，而不是 类似clear ，会创建一个空屏 
>
> ctrl + u ：清空当前行，无论光标在什么位置 
>
> (**) + ⌘ + ; ： [(**) 输入的命令开头字符],会自动列出输入过的命令 
>
> ⌘ + shift + h ： 会列出剪切板历史 
>
> ⌘← / ⌘→ : 到一行命令最左边/最右边  
>
> ⌘ + enter : 全屏