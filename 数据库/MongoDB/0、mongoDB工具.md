## MongoDB工具

MongoDB在bin目录中提供了一系列有用的工具这些工具提供了MongoDB在运维管理上的方便。

| 工具         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| mongosniff   | mongodb监测工具，作用等同于tcpdump                           |
| mongodump    | mongo数据备份工具<br />mongodump -h dbhost -d dbname -o dbdirectory<br />-h：MongoDB所在的服务器地址<br />-d：需要备份的数据库实例<br />-o：备份的数据存放位置 |
| mongoimport  | mongodb数据导入工具                                          |
| mongoexport  | mongo数据导出工具                                            |
| bsondump     | 将bson格式数据转出为json格式数据                             |
| mongoperf    |                                                              |
| mongorestore | mongodb数据恢复工具                                          |
| mongod.exe   | MongoDB服务启动工具                                          |
| mongofiles   | GridFS管理工具，可实现二进制文件的存取                       |
| mongooplog   |                                                              |
| mongotop     | 跟踪一个MongoDB实例，查看哪些大量的时间花费在读取和写入数据。 |
| mongos       | 分片路由。如果使用了sharding功能，则应用程序连接的是mongos而不会mongod |
| mongo        | 客户端命令行工具，支持js语法                                 |

