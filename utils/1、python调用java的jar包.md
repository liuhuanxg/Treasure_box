## 在python中调用jar包

最近的项目功能需要调用客户的java接口，在调用接口的时候需要使用配套的jar包生成一些参数，但是公司的项目是用django搭建的，经过无数血与泪的尝试，最终终于找到了合适的方法去调用.....

jpype包是一个Python的包，可以在python项目中调用java的jar包，并获取最终的返回值。使用方法很简单，但是安装的过程比较复杂。本文在ubuntu18.0.4系统，以python2以及jdk8为基础，讲解jpype包的使用。

### 1、安装java

首先去官网下载jdk1.8的tar包，放在`/opt`路径下，然后进行安装：

> tar -zxvf jdk-8u261-linux-x64.tar.gz

安装完jdk之后，需要配置环境变量，在/etc/profile文件中添加以下内容：

```
set java environment
JAVA_HOME=/opt/java/jdk1.8.0_261        
JRE_HOME=/opt/java/jdk1.8.0_261/jre     
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
```

重启环境变量：`source /etc/profile`

### 2、安装python的jpype包

找到项目依赖的python环境：

```
sudo find / -name site-packages
```

将下载的jpype包放在`site-packages`路径下。

### 3、使用示例

jpype中主要包含以下方法：

```
jpype.getDefalutJVMPath()    # 获取jvm所在的路径
jpype.startJVM()             # 开启虚拟机
demo = jpype.JClass('hello.Demo')   # 加载jar包中的Demo文件
demo.sayHello()     # 调用 sayHello 方法
jpype.shutdownJVM()  # 关闭虚拟机
```

代码示例：

```python
def run_jar():
    jvm_path = "/opt/java/jdk1.8.0_261/jre/lib/amd64/server/libjvm.so"
	jar_path = os.path.join(os.path.abspath("."), "/home/youzi/ssojar/Hello.jar")
    jpype.startJVM(jvm_path, "-Djava.class.path=%s" % jar_path) # 启动虚拟机
    demo = jpype.JClass('Demo')  # 加载Demo类
    demo.sayHello()				 # 调用sayHello方法
    jpype.shutdownJVM()  # 关闭虚拟机
```

