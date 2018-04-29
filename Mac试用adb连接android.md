# Mac试用adb连接android

###第一步
* 启动终端
* 进入HOME目录下

  ```
  cd $HOME
  ```
* 打开.bash_profile文件

  ```
  open -e .bash_profile
  ```
* 若该文件不存在时,通过命令创建

  ```
  touch -e .bash_profile
  ```
* 打开.bash_profile文件,对其内容进行编辑,根据SDK的安装目录进行修改保存文件,关闭.bash_profile

  ```
  export PATH=${PATH}:/App/adt-bundle-mac-x86_64-20140702/sdk/platform-tools
  export PATH=${PATH}:/App/adt-bundle-mac-x86_64-20140702/sdk/tools
  ```
* 更新环境变量的配置

  ```
  source .bash_profile
  ```

* 测试连接成功
  	
  ```
  adb devices
  ```

