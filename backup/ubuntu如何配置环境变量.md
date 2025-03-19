1. 首先去官网下载jdk
在终端使用命令进行解压:
```linux
tar -zxf /home/wiki/Download/jdk-8u77-linux-x64.tar.gz
```
可以将其移动到安装目录：
```linux
mv jdk1.8.0.7 /home/wiki/develop
mv jdk1.8.0.7 jdk #改名
```

2. 设置环境变量
编辑文件：
```linux
vim /home/wiki/.bashrc
```
按i进入编辑模式，在头文件中添加环境变量：
```linux
export JAVA_HOME=/home/wiki/develop/jdk
```
如果想添加jre，则：
```linux
export JRE_HOME=${JAVA_HOME}/JRE
```
将jdk的bin目录添加到path环境变量中：
```linux
export PATH=${JAVA_HOME}/bin:$PATH
```

3. 使设置生效
按esc推出编辑模式，然后再ctrl+c推出编辑文件模式
对.bashrc文件运行source命令，使得设置立即生效：
```linux
source /home/wiki/.bashrc
```