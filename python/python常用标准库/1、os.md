## OS模块

| 函数                         | 说明                                                      |
| ---------------------------- | --------------------------------------------------------- |
| os.getcwd()                  | 获取当前工作路径                                          |
| os.chdir(path)               | 切换到path路径                                            |
| os.listdir(path)             | 获取path路径的所有文件或文件夹列表，文件夹为空时为空列表  |
| os.mkdir(path,permission)    | 创建空文件夹，可以指定权限                                |
| os.makedirs("/home/vc/ss")   | 循环创建文件夹                                            |
| os.removedirs("/home/vc/ss") | 循环删除空文件夹，文件夹不为空时会报错                    |
| os.rename("home","HOME")     | 文件夹重命名                                              |
| os.stat(path)                | 获取文件或文件夹信息                                      |
| os.system("ls -al")          | 执行系统命令                                              |
| result=os.popen("ls -al")    | 执行系统命令并能获取执行结果                              |
| os.getenv('PATH')            | 获取系统环境变量                                          |
| os.putenv('PATH','/home/sy/  | 将一个目录添加到环境变量中                                |
| os.curdir                    | 表示当前文件夹，类似(.)                                   |
| os.pardir                    | 表示上一层文件夹，类似(..)                                |
| os.name                      | 获取操作系统的名字，nt代表windows，posix代表linux或者unix |
| os.sep                       | 获取系统路径间隔符号，windows\                            |
| os.extsep                    | 获取文件名称和后缀的间隔符号                              |
| os.linesep                   | 获取操作系统的幻狼福，windows是“\r\n”，linux是"\n"        |

### os.path子模块的内容

| 函数                           | 说明                                                        |
| ------------------------------ | ----------------------------------------------------------- |
| os.path.abspath("/boys")       | 将相对路径转换为绝对路径                                    |
| os.path.dirname(path)          | 获取完整目录除最后文件夹的其他部分                          |
| os.path.basename(path)         | 获取最后的目录部分                                          |
| os.path.split("/home/sy/boys") | ('/home/sy', 'boys') 将一个完整路径切割成目录部分和主体部分 |
| os.path.join(path1,path2)      | 将两个路径合并成1个                                         |
| os.path.splitext(filepath)     | 将一个文件路径切成文件后缀和其他两部分，用于获取文件后缀名  |
| os.path.getsize(filepath)      | 获取文件的字节数                                            |
| os.path.isfile(path)           | 检测是否是文件                                              |
| os.path.isdir(path)            | 检测是否是文件夹                                            |
| os.path.islink(path)           | 检测是否是链接                                              |
| os.path.getctime(filepath)     | 获取文件的创建时间                                          |
| os.path.getmtime(filepath)     | 获取文件的修改时间                                          |
| os.path.getatime(filepath)     | 获取文件的访问时间                                          |
| os.path.exists(filepath)       | 检测某个路径是否真实存在                                    |
| os.path.isabs(path)            | 检测一个路径是否绝对路径                                    |
| os.path.samefile(path1,path2)  | 检测两个路径是否是同一个文件                                |

