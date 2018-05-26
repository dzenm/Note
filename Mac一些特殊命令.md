

### 一、高德开发版SHA

直接在AndroidStudio的Terminal或者终端输入一下命令即可：

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```





----



### 二、App应用签名



 查询keystore的MD5，可以在终端窗口下，定位到keystore所在的路径

```
keytool -list -v -keystore xxx.store
```





----



###三、Mac上通过adb连接android

- 启动终端

- 进入HOME目录下

  ```
  cd $HOME
  ```

- 打开.bash_profile文件

  ```
  open -e .bash_profile
  ```

- 若该文件不存在时,通过命令创建

  ```
  touch -e .bash_profile
  ```

- 打开.bash_profile文件,对其内容进行编辑,根据SDK的安装目录进行修改保存文件,关闭.bash_profile

  ```
  export PATH=${PATH}:/App/adt-bundle-mac-x86_64-20140702/sdk/platform-tools
  export PATH=${PATH}:/App/adt-bundle-mac-x86_64-20140702/sdk/tools
  ```

- 更新环境变量的配置

  ```
  source .bash_profile
  ```

- 测试连接成功
  	

  ```
  adb devices
  ```





----



### 四、Android  Studio通过Terminal命令行打包



* 打包release版，不完整，还需要使用签名

```
./gradlew assemblerelease 
```



* 打包debug版本

```
./gradlew assembledebug
```





----



### 五、显示隐藏路径的 Terminal 命令

* 方法一：命令行操作

```
显示隐藏路径的 Terminal 命令：defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
取消隐藏路径的 Terminal 命令：defaults write com.apple.finder AppleShowAllFiles -bool false ; killall Finder
```

* 方法二：快捷键操作

  ​	在 macOS Sierra，我们可以使用快捷键`：Command + Shift + .`来快速（在 Finder 中）显示和隐藏隐藏文件了。





---



###六、Mac 10.12找回任何来源

终端下输入如下命令回车即可在设置显示

> sudo spctl --master-disable





---

