## PyMySQL

1. #### pymysql简介

    PyMySQL：是一个使python连接到MySQL的库，是一个纯python的库。

    **环境要求：**

    1. python2.7
    
    2. python version>=3.4
    
    3. 安装PyMYSQL
    
        pip install PyMySQL
    
3. #### 导包

    import pymysql

4. #### 连接数据库

    参数：host,user,password,database,port,

    **cursorclass=pymysql.cursor.DictCursor**不加该参数时表现形式为元组。

    加参数时输出为字典格式。

    ```python
    db = pymysql.connect(host="10.10.101.243",user="root",password="root",database="test",port=3306)
    ```

5. #### 创建游标对象

    游标：游标是处理数据的一种方法，为了查看或者处理结果集中的数据，在结果集中一次一行或者多行前进或向后浏览数据的能力。可以把游标当作一个指针，它可以指定结果中的任何位置，然后允许用户对指定位置的数据进行处理通俗来说，操作数据和获取数据库结果都要通过游标来操作。

    ```python
    cursor = db.cursor()
    ```

6. #### 定义sql语句

    ```python
    sql = "SELECT database()"
    ```

7. #### 执行sql语句，获取返回值

    ```python
    cursor.execute(sql)
    ```

    ```python
    #获取返回值 fetchone()  获取一条数据
    print(cursor.fetchone())
    #获取返回值 fetchall()  获取所有数据
    print(cursor.fetchall())
    #获取返回值 fetchmany()  获取2条数据
    print(cursor.fetchmany(2))
    ```

8. #### 关闭数据库连接

    ```python
    cursor.close()
    db.close()
    ```

    

9. #### 封装MySql类

    ```python
    import pymysql
    class MySql():
    	def __init__(self, host, user, password, database):
            self.host = host
            self.user = user
            self.password = password
            self.database = database
            self.cursorclass = pymysql.cursors.DictCursor
            # self.port=3306
        def connet(self):
            self.db = pymysql.connect(
                self.host, self.user, self.password, self.database)
            self.cursor = self.db.cursor()
        def close(self):
            self.cursor.close()
            self.db.close()
    
        def get_one(self, sql):
            res = None
            try:
                self.connet()
                self.cursor.execute(sql)
                res = self.cursor.fetchone()
                self.close()
            except BaseException as e:
                print("get_one",e)
                print('查询失败。')
            return res
            
        def get_all(self, sql):
            res = ()
            try:
                self.connet()
                self.cursor.execute(sql)
                res = self.cursor.fetchall()
                self.close()
            except BaseException as e:
                print("get_all:",e)
                print('查询失败。')
            return res
    
        def insert(self, sql):
            return self.__edit(sql)
    
        def update(self, sql):
            return self.__edit(sql)
    
        def delete(self, sql):
            return self.__edit(sql)
    
        def __edit(self, sql):
            count = 0
            try:
                self.connet()
                count = self.cursor.execute(sql)
                self.db.commit()
                self.close()
            except BaseException:
                print('事务提交失败。')
                self.db.rollback()
            return count
    ```

    

    