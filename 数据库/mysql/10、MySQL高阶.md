## MySQL高阶—事务、触发器、存储过程

### 一、数据库事务

#### （1）**数据库中的事务有四大特性：ACID**

- **原子性**：每个事务都是不可分割的执行，要么全部成功，要么全部失败。
- **一致性**：事务执行之前和执行之后的状态保持一致。比如A有1000元，B有1000元，A给B转账500元之后，A与B的钱数之和应该还是2000元。
- **隔离性**：多个事务同时操作，每个事务之间相互独立，互不影响。
- **持久性**：事务提交成功后对数据库的改变是永久的。

#### （2）**数据库三范式**

​	数据库的范式(规范的数据表示公式)：按照什么方式在数据库中表示（存储）是完全合理是做不到的，只能在数据库设计过程中尽量靠近三范式约束。

- **1NF**：字段不可分割

    数据库中的每一列数据，是不能再拆分的。

- **2NF**：有主键，非主键字段依赖主键。

    数据库中的每一条数据都是唯一的，主键作为数据唯一的描述符。

- **3NF**：非关键字的任何字段属性，不能产生相互的依赖条件	

    不是主键的任何其他字段，不能产生相互的依赖关系。

#### （3）事务的隔离级别

​	事务的隔离级别，指多个事务同时操作数据库时，不同事务之间应该怎么定义他们的操作。

- **读未提交：read uncommitted**

    一个事务中，读取了另一个事务中没有提交的数据，两个事务之间造成了影响。

- **读已提交：read committed**

    一个事务中，读取了另一个事务中提交的数据。

- **可重复读：repeat read**

    在一次完整的事务中，每次读取的数据都是一致的，不会发生变化，所有提交的更新的数据都会在下一个事务中读取到。数据库默认的隔离级别

- **串行化/序列化：serializerable**

    所有的事务操作全部排队，依次执行

### 二、触发器

数据库中提供了特殊的处理方法：自动化操作，本质上是当数据库中发生了一些行为之后，导致一些其他的行为自动触发，类似python开发中的事件驱动开发。

数据库中提供了一种数据库高级对象：触发器；描述的是数据表上一个条件被触发执行的后续行为操作。

#### ① 触发器语法：

create trigger trigger_name trigger_time trigger_event on table_name for each row trigger_stmt end;

描述：在某张表上，发生了一个触发事件，在触发事件发生之前|之后(触发时机)，执行触发器中定义的要执行的程序。

**trigger_time：触发时机，before|after**

**trigger_event：触发事件,insert|update|delete**

#### **②触发器案例**：自动下单功能

- 创建测试数据表

    ```sql
    create table goods(
      gid int auto_increment primary key comment '商品主键',
      gname varchar(20) not null comment '商品名称',
      gprice double not null comment '商品单价',
      gstock int not null comment '商品库存',
    );
    
    create table goods_order(
    	goid int auto_increment primary key comment '订单编号',
        goname varchar(20) comment '购买商品名称',
        goprice double comment '成交单价',
        gocount int comment '购买数量',
        subtotal double comment '小计金额'
    );
    
    
    ```

- 创建触发器

    ```sql
    delimiter $$
    -- 创建一个触发器
    create trigger goods_sale_auto
    	-- 在goods表格修改之后执行触发器
    	after update on goods for each row
    	-- 要执行的程序开始操作
    	begin 
    		-- 声明两个变量
    		declare buycount int;
    		declare subtotal double;
    		-- 判断库存是否更新：更新前old，更新后new
    		if new.gstock<old.gstock
    		then 
    			--获取购买的数量
    			set buycount = old.gstock - new.gstock;
    			set subtotal = buycount * old,gprice;
    			insert into goods_order(gname,goprice,gcount,subtotal) values(old.gname,old,gprice,buycount,subtotal);
    		end if;
    	-- 要执行的程序完结操作
    	end;
    	$$
    	delimiter;  --触发器创建完成后，修改结束符为默认的分号
    ```

    

### 三、存储过程

​	触发器是数据库中根据发生的条件(某张表上发生了INSERT/UPDATE/DELETE操作)自动执行的**数据库程序**，当我们需要自定义程序，并且手工调用时要怎么去做？

​	数据库提供了另外一种高级对象：存储程序，一般称为存储过程，就是一个用户按照规范语法编写的程序代码，可以将项目中的业务逻辑封装在程序中，通过固定的语法方式直接调用执行，类似python中的函数。

#### （1）基本语法结构

​	创建存储过程

```sql
CREATE PROCEDURE proc_name ([proc_parameters]) routing_body
```

create procedure 固定语法：创建存储过程

proc_name：自定义存储过程名称

proc_parameters：存储过程执行需要的参数

routing_body：存储程序要执行的程序代码

#### （2）案例

```sql
delimiter $$
create procedure employee_avg()
begin 
	-- 模拟一行或多行代码
	select AVG(salary) from ex01.employee
end;
$$
-- 调用存储过程
call employee_avg();
```

#### （3）带有返回值的存储过程

```sql
delimiter $$
create procedure my_employee2(out res double)
begin 
	-- 查询数据，并将数据保存到变量中
	select AVG(salary) into res from ex01.employee
end;
$$
-- 调用存储过程
call my_employee2(@res);
--查看返回值的数据
select @res;
```

返回值声明在存储过程名称后面的括号：

**out：**返回数据

**in：**输入数据

**input：**既是输入数据，同时也能返回数据

调用时，需要使用变量接受数据，为了跟其他变量区分，添加@符号：call mey_employee(@res);