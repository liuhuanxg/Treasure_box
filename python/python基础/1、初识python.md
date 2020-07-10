## 初识python

### 一、安装python

1. #### windows安装

    在python官网下载对应的解释器版本：https://www.python.org/。双击运行解释器进行安装。推荐在安装时不要安装在C盘。

2. #### Linux安装

    Linux环境中自带了Python 2.x版本，想更新到3.x版本需要去官网下载对应的解释器，以Centos安装python示例：

    1. **安装依赖库**

        ```
        yum -y install wget gcc zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
        ```

    2. **下载Python源码并解压到指定目录**

        ```
        wget https://www.python.org/ftp/python/3.7.6/Python-3.7.6.tar.xz
        xz -d Python-3.7.6.tar.xz
        tar -xvf Python-3.7.6.tar
        ```

    3. **切换到Python源代码目录并执行下边的命令进行配置和安装**

        ```
        cd Python-3.7.6
        ./configure --prefix=/usr/local/python37 --enable-optimizations
        make && make install
        ```

    4. **修改用户主目录下名为.bash_profile的文件，配置PATH环境变量。**

        ```
        cd ~
        vim .bash_profile
        ```

        ```
        export PATH=$PATH:/usr/local/python37/bin
        ```

        

    5. **激活环境变量**

        ```
        source .bash_profile
        ```

3. #### mac安装

    macOS也自带了Python 2.x版本，可以通过[Python的官方网站](https://www.python.org/)提供的安装文件（pkg文件）安装Python 3.x的版本。默认安装完成后，可以通过在终端执行`python`命令来启动2.x版本的Python解释器，启动3.x版本的Python解释器需要执行`python3`命令。

### 二、运行python程序

1. #### 确认Python版本

    - 可以在windows的命令提示符中键入：`python --version`

    - Linux或者MacOS系统在终端输入：`python3 --version`

    - 也可以输入python或者这python3进入交互模式:

        ```python
        import sys
        print(sys.version_info)
        print(sys.version)
        ```

2. #### 编写python源代码

    可以用文本编辑工具（推荐使用[Sublime](https://www.sublimetext.com/)、[Visual Studio Code](https://code.visualstudio.com/)等高级文本编辑工具）编写Python源代码并用**hello.py**作为文件名保存该文件，代码内容如下所示：

    **注意：在Windows中需要显示文件扩展名。**

    ```python
    print("hello")
    ```

    python中时候换行符标识语句结束，多个python语句示例：

    ```python
    print("hello")
    print("world")
    print("this is ths first python project")
    ```

    **对代码的说明：**

    ​	print是python内置的函数，用于在终端输出信息，经常用于代码调试，需要牢记。

3. #### 运行程序

    在windows的命令行（linux 的终端）切换到**文件所在的文件夹**，执行以下命令：

    ```
    python hello.py
    ```

4. #### 代码中的注释

    **注释**是编程语言的一个重要组成部分，用于在源代码中解释代码的作用从而增强程序的可读性和可维护性，当然也可以将源代码中不需要参与运行的代码段通过注释来去掉，这一点在调试程序的时候经常用到。注释在随源代码进入预处理器或编译时会被移除，不会在目标代码中保留也不会影响程序的执行结果。

    python中的注释分为两种：

    - **单行注释**（使用#）

    - **多行注释（使用""""""或者''''''）**

        ```python
        """
        @File: hello.py
        @Time: 2020/6/6 13:50
        @user：liuhuan   
        多行注释
        """
        # 单行注释
        print("hello")
        ```

5. #### Python开发工具

    工欲善其事，必先利其器。开发工具在开发展过程中占据着重要的一环。python有自带的idea开发工具，但是在开发大型项目时不太适合。推荐的开发工具有sublime，editplus，pycharm，visual code等，其中pycharm是为python量身打造的开发工具，在开发大型项目时很方便。

6. #### python之禅

    在python交互模式中输入：`import this`查看python之禅

    ```python
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    ```

    