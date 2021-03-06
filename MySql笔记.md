## DQL：查询表中的记录
	 select* from 表名；
	1.语法
		select 
			字段列表
		from
			表名列表
		where
			条件列表
		group by
			分组字段
		having
			分组之后的条件
		order by
			排序
		limit
			分页限定
	2.基础查询
		2.多字段查询
			select 字段名1，...，from 表名
				*注意：
					*如果查询所有字段，可以使用*代替字段列表
		3.去除重复的结果集
			*使用distinct		
		4.起别名
			*使用as
		5.计算列
			*一般使用四则运算计算列的值
			*如果有null参与运算，计算结果都是null
				*可以使用ifnull()表达式
	3.条件查询
		1.where子句
		2.运算符
			*> < >= <=...
			*like(模糊查询)
				*占位符
					*_:单个任意字符
					*%：多个任意字符
			*is null(因为null值不能用=号判断)
			*between(效果等效and)
			*in(效果等效于or)
			...
### DQL中的排序查询
	1.语法：order by 子句
		* order by 排序字段1....
	2.asc是升序，desc是降序
### 聚合函数
	1.count:计算个数
	2.max,min,sum，avg函数
	3.聚合函数会排除NULL
		*选择不包含空的列计算
		*使用ifnull()
### 分组查询
	1.语法：group by 分组字段
	2.注意：
		*分组以后查询的字段应该是分组字段或者是聚合函数
		*where和having的区别
			*where是分组之前限定，不满足条件则不参与分组；having是在分组之后进行限定，如果不满足，则不会被查询出来
			*where后不能跟聚合函数，having后可以跟聚合函数
### 分页查询
	1.语法：limit 开始的索引，每页查询的条数
	2.公式：开始的索引 = (当前页码-1)*每页显示的条数

### 表的约束
	1.概念：对表中数据进行限定，保证数据的正确的先和完整性。
	2.分类：
		1.主键约束
			*primary key
			*删除语法：alter table 表名 drop primary key
			*自动增长：auto_increment
		2.非空约束
			* NOT NULL(创建表的时候添加，也可以创建完成后添加)
			* 删除语法：alter table 表名 modify 字段名
		3.外键约束
			*foreign key
			*创建语法：
				creat table 表名(
					...
					外键列
					constraint 外键名称 foreign key(外键名称) references 主表名称(主表列名称)
				);
			*删除外键：alter table 表名 drop foreign 外键字段名
			*级联更新：on update cascade
			*级联删除：on delete cascade
		4.唯一约束
			*unique，值不能重复，但可以有多个NULL
			*删除语法:alter table 表名 drop index 字段名
## 数据库的设计

### 多表之间的关系
	1.分类	
		1.一对一
		2.一对多 
		3.多对多
	2.实现：
		1.一对多：在多的一方建立外键关联少的一方的主键
		2.多对多：借助第三张中间表，中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键
		3.一对一：可以在任意一方添加外键指向另一方的主键。

### 数据库设计的范式：数据库设计的规范
	1.各种范式呈递次规范，越高范式，数据冗余度越高。
	2.分类:
		1.第一范式(1NF)：每一列都是不可分割的原子数据项。
		2.第二范式(2NF)：在1NF技术上，非码属性必须完全依赖于码(消除非主属性对主码的部分函数依赖)。
			*几个概念
				1.函数依赖：通过某个属性A值可以确定唯一的属性B值，则B依赖A
				2.完全函数依赖：如果B组属性的确定依赖于A组所有的属性值，称为完全函数依赖
				3.部分函数依赖：如果B组属性的确定依赖于A组某一些属性值，称为部分函数依赖
				4.传递函数依赖：通过A属性值可以唯一确定B属性值，通过B属性值有可以唯一确定C属性值。
				5.码：如果一张表中，一个属性或属性组，被其他所有属性完全依赖，则称这个属性为该表的码
		3.第三范式(3NF)：在2NF基础上，任何非主属性不依赖与其他非主属性。
## 数据库的备份和还原
	1.命令行：
		备份：mysqldump -u用户名 -p密码 > 保存的路径
		还原：
			1.登录数据库
			2.创建数据库	
			3.使用数据库
			4.执行文件。 source 文件路径

## 多表查询
	1.分类：
		1.内连接查询:
			*显式内连接：select 字段列表 from 表名1 [inner] join 表名2 on 条件
			*隐式内连接：使用where清除无用的数据
			*注意：
				*确定从哪些表中查询数据
				*条件是什么
				*查询哪些字段
		2.外连接查询：
			*左外连接
				*语法：select 字段列表 from 表1 left [outer] join 表2  on 条件
				*查询的是左表所有数据以及其交集部分
			*右外连接
				*语法：select 字段列表 from 表1 right [outer] join 表2  on 条件
				*查询的是右表的所欲数据及其交集部分
		3.子查询：
			*概念：查询中嵌套查询，称这样的查询为子子查询
			*查询结果：
				*单行单列：一般使用运算符作为判断
				*多行单列：使用运算符in判断
				*多行多列：
## 事务
	1.事务的基本介绍
		*1.概念：包含多个步骤的业务操作被事务管理，这些操作要么全部完成，要么一个都不执行。
		*2.基本操作：开启事务：start transaction
					提交事务：commit
					回    滚：rollback
		*3.事务提交的两种方式：
			*默认提交：select @@autocommit; 1代表自动提交  0代表手动提交
			*手动提交：set @@autocommit = 0
	2.事务的四大特征
		*1.原子性:要么同时成功，要么同时失败
		*2.持久性：事务提交或者回滚以后，数据库会持久化的保存数据
		*3.隔离性：多个事务之间相互独立
		*4.一致性：事务操作前后，数据总量不变
	3.事务的隔离级别
		*1.概念：事务之间是隔离的，但是多个事务操作同一批数据，则会引起一些问题，设置不同的隔离级别就可以解决
		*2.存在问题：
			1.脏读：一个事务，读取到另一个事务中没有提交的数据
			2.不可重复读(虚读)：在同一个事务中，两次读取的数据不一样。
			3.幻读：一个事务操作数据部表中的所欲数据，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。
		*3.隔离级别：
			*read uncommitted：读未提交
				*存在问题：脏读，不可重复读，幻读
			*read committed:读已提交（oracle）
				*存在的问题:不可重复读，幻读
			*repeatable read：可重复读(MySql)
				*参在的问题：幻读
			*serializable：串行化
				*可以解决所有问题
			注意：
				隔离级别越高，效率越低
				数据库查询隔离级别：  select @@tx_isolation
				数据库设置隔离级别：set global transaction isolation level 级别字符串
## DCL
	1.管理用户：
		1.添加用户：create user '用户名' @'主机名' identify '密码'。
		2.删除用户：drop user '用户名' @'主机名'
		3.修改用户密码：update user set 字段名=新的值 where 条件子句
			*忘记root密码：
				1.使用管理员权限运行cmd，并停止mysql服务
				2.使用无验证方式启动：mysqld --skip-grant-tables
				3.打开新的窗口，直接输入mysql命令登录
				4.use mysql
				5.update user set password=password("新密码") where user = 'root'
				6.关闭窗口
				7.手动结束mysqld.exe进程
				8.启动mysql服务
				9.使用新密码登录
	2.权限管理：
		1.查询权限：
			show grants for '用户名' @'主机名'
		2.授予权限：
			grant 权限列表 on 数据库名.表名 to '用户名' @'主机名'
		3.撤销权限：
			revoke 权限列表 on 数据库名.表名 from '用户名' @'主机名'
