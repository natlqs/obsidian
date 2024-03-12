- 数据库的概念和作用
	- 数据库的概念
	  collapsed:: true
		- 数据库就是以一定格式进行组织的数据的集合，通俗来看数据库就是用户计算机上的一些具有特殊格式的数据文件的集合。
	- 数据库的特点
	  collapsed:: true
		- 持久化存储
		- 读写速度极高
		- 保证数据的有效性
		- 对程序支持性非常好，容易扩展
- 数据库的分类和特点
	- 关系型数据库：采用了关系模型来组织数据的数据库，简单来说，关系模型指的就是二维表格模型。
	  collapsed:: true
		- MySQL采用了双授权政策，它分为社区版和商业版，由于体积小、速度快、总体使用成本低，尤其是开源，一般中小型网站的开发都选择MySQL作为网站数据库。
		- Oracle、SQL Server、MySQL、SQLlite等。
			- Oracle：银行、电信等项目
			- MS SQL Server：在微软的项目中使用
			- SQLlite：轻量级数据库，主要应用在移动平台
			- MySQL：web时代使用最广泛的关系型数据库
	- 非关系型数据库：强调Key-Vaue的方式存储。
	  collapsed:: true
		- MongoDB， Redis
- 数据库管理系统
	- 数据库管理系统（Database Management System，简称DBMS），是为管理数据库而设计的软件系统。
	  collapsed:: true
		- 数据库文件集合：主要是一系列的数据文件，作用是存储数据。
		- 数据库服务器：主要负责对数据文件一级文件中的数据进行管理
		- 数据库客户端：主要负责和服务端通信，向服务端传输数据或者从服务端获取数据。
	- [[assets/python：MySQL数据库基础/49711e07e399c1ad7d65c00244a95909_MD5.jpeg|Open: Pasted image 20240305140409.png]]
![[assets/python：MySQL数据库基础/49711e07e399c1ad7d65c00244a95909_MD5.jpeg]]
	- SQL语句
		- SQL（Structured Query Language)是结构化查询语言，是一种用来操作RDBMS的数据库语言，当前几乎所有的关系型数据库都支持使用SQL语言进行操作，也就是说可以通过SQL操作oracle，sql server， mysql，sqlite等所有的关系型数据库。
		- RDBMS：Relational Database Management System关系型数据库管理系统是专门用来管理关系型数据库的系统。
		- 数据库客户端和数据库服务端怎么通讯呢？数据库客户端通过特殊的语言告诉服务端，客户端想要做什么，这个专门的语言就是SQL语句。
- 关系型数据库中的核心元素
	[[assets/python：MySQL数据库基础/63972b0effbf90627fe4f2afea15bbf8_MD5.jpeg|Open: Pasted image 20240305140506.png]]
![[assets/python：MySQL数据库基础/63972b0effbf90627fe4f2afea15bbf8_MD5.jpeg]]

- MySQL环境搭建-UBUNTU
	- 服务端安装
		- 在终端中输入如下命令
			- `sudo apt-get install mysql-server`
			- Ubuntu默认自带MySQL服务器端，且开机启动。
		- 启动服务
			- `sudo service mysql start`
		- 查看进程中是否存在MySQL服务
			- `ps ajx|grep mysql`
			- ps:查看当前系统进程 -a 显示所有用户进程 -j任务格式显示进程 -x显示无控制终端进程。
	- 客户端安装
		- 在终端中运行如下命令
			- `sudo apt-get install mysql-client`
			- Ubuntu默认自带MySQL客户端，且开机启动
		- 最基本的连接命令如下
			- `mysql-uroot-pmysql`
		- 退出连接：
			- exit或者quit
	- MySQL配置文件
		- 配置文件目录为 `/etc/mysql/mysql.cnf.d`
		- [[assets/python：MySQL数据库基础/6e187fa7966ce756d3339642017eb7c5_MD5.jpeg|Open: Pasted image 20240305140535.png]]
![[assets/python：MySQL数据库基础/6e187fa7966ce756d3339642017eb7c5_MD5.jpeg]]
		- 主要配置选项
			- bind-address 表示服务器绑定的ip，默认为127.0.0.1
			- prot表示端口，默认为3306
			- datadir表示数据库目录，默认为/var/lib/mysql
			- general_log_file表示普通日志，默认为/var/log/mysql/mysql.log
			- log_error表示错误日志，默认为/var/log/mysql/error.log
- Navicat客户端的使用
	- MySQL数据库本身自带有命令行工具，但自带的工具在功能上和易用性上总比不上第三方开发的工具，比如Navicat，使用它几乎可以完成任何针对MySQL的管理任务。
	- Navicat是一套快速、可靠价格适宜的数据库管理工具，Microsoft Widonws, Mac OS X及 Linux。可以用来对本机或远程的MySQL、SQL Server、SQLite、Oracle及PstgreSQL数据库进行管理和开发。转为简化数据库的管理及降低系统管理成本而设，他的设计符合数据库管理员、开发人员及中小企业所需。Navicat是以直觉化的图形用户界面而建的，让你可以以安全并且简单的方式对数据库进行操作。
- MySQL数据类型
	- 整型类型
		[[assets/python：MySQL数据库基础/fa0c67705a863db6e11cadb32f1e6950_MD5.jpeg|Open: Pasted image 20240305140547.png]]
![[assets/python：MySQL数据库基础/fa0c67705a863db6e11cadb32f1e6950_MD5.jpeg]]
	- 浮点型
		- float：单精度型，只保证6位有效数字的准确性
		- double：双精度型，只保证16位有效数字的准确性
		- decimal：定点数，其中decimal(5, 2)代表共5位数字，其中2位是小数，比如：888.88
	- 字符串
		- varchar：适用于经常变化的字段
		- char：适用于知道固定长度的字符串
		- 尽量使用varchar
		- 超过255字节的只能用varchar或者text，能用varchar的地方不用text
		- ![[assets/python：MySQL数据库基础/b3982eb55b0c24a52a3acf58b020cf73_MD5.jpeg]]
		- char：定长字符串，在创建表时，char字段占用硬盘空间的大小就已经固定了。
			- 比如，我们定义name char（10），代表name字段将占10个字符的硬盘空间，那么具体是多少个字节要看是什么编码的，如果是utf-8编码，那么一个字符占3个字节，所以char（10）将占30个字节，无论内容够不够10个字节都占30个字节，比如只存abc三个字符也要占30个字节。
		- varchar：变长字符串，指的是字段占用硬盘空间的大小并不是固定的，而是由内容决定的，等于内容的长度+1个字节（字符串结束'\0')。
			- 比如，我们定义name varchar（10），代表最多保存10个字符，如果表是utf-8编码，则最多占用30个字节，但是如果内容不够10个字节，比如内容是abc，那么只占三个字符9个字节再+1，共占10个字节。
		- text
			- [[assets/python：MySQL数据库基础/277bb9fd5690b7377f5ff5c7b0004e87_MD5.jpeg|Open: Pasted image 20240305140634.png]]
![[assets/python：MySQL数据库基础/277bb9fd5690b7377f5ff5c7b0004e87_MD5.jpeg]]
			- #+BEGIN_QUOTE
			  text与char和varchar不同的是，text不可以有默认值，其最大长度是2的16次方-1
			  #+END_QUOTE
	- 枚举类型enum
		- 在定义字段时就预告规定好固定的几个值，然后插入记录时值只能在这几个固定的值中选择一个。
		- 语法定义：`gender enum('男','女','妖')`
	- 时间类型
			[[assets/python：MySQL数据库基础/99ca83be03221efda12f4676d4237c32_MD5.jpeg|Open: Pasted image 20240305140646.png]]
![[assets/python：MySQL数据库基础/99ca83be03221efda12f4676d4237c32_MD5.jpeg]]

- 数据完整性和约束
	- 数据完整性用于保证数据的正确性，系统在更新、插入或删除等操作时都要检查数据的完整性，核实其约束条件。
	- 参照完整性
		- 参照完整性属于表间规则。在更行、插入或者删除记录时，如果只改其一，就会影响数据的完整性。如删除表2的某记录后，表1的相应记录未删除，只是这些记录成为孤立记录。
	- 约束作用是保证数据的完整性和一致性
		- [[assets/python：MySQL数据库基础/33a16ef9d87fb6ee0ac7b6232d4b57fd_MD5.jpeg|Open: Pasted image 20240305140700.png]]
![[assets/python：MySQL数据库基础/33a16ef9d87fb6ee0ac7b6232d4b57fd_MD5.jpeg]]
- 数据库的操作命令
	- 登录和退出数据库命令
		- [[assets/python：MySQL数据库基础/d987598fc626a5089ace5ba9a05309e6_MD5.jpeg|Open: Pasted image 20240305140719.png]]
![[assets/python：MySQL数据库基础/d987598fc626a5089ace5ba9a05309e6_MD5.jpeg]]
	- 数据库基本操作命令
		- [[assets/python：MySQL数据库基础/80742bc8f67341fd0374e1441def36fc_MD5.jpeg|Open: Pasted image 20240305140728.png]]
![[assets/python：MySQL数据库基础/80742bc8f67341fd0374e1441def36fc_MD5.jpeg]]
	- 数据表基本操作命令
		- 在开发过程中，不需要高频度的操作表结构，所以该章节的目的就是让我们能够在需要的时候根据这些语法写出符合要求的SQL语句即可。
		- [[assets/python：MySQL数据库基础/34d4a0edafefabe08b6150b50a03c36d_MD5.jpeg|Open: Pasted image 20240305140736.png]]
![[assets/python：MySQL数据库基础/34d4a0edafefabe08b6150b50a03c36d_MD5.jpeg]]
		- [[assets/python：MySQL数据库基础/30d00981b05455521d3c1854dc9ef753_MD5.jpeg|Open: Pasted image 20240305140747.png]]
![[assets/python：MySQL数据库基础/30d00981b05455521d3c1854dc9ef753_MD5.jpeg]] [[assets/python：MySQL数据库基础/5d6527463da5fc1a3506f4f8b066fefe_MD5.jpeg|Open: Pasted image 20240305140758.png]]
![[assets/python：MySQL数据库基础/5d6527463da5fc1a3506f4f8b066fefe_MD5.jpeg]]
	- 数据表结构修改命令
		- [[assets/python：MySQL数据库基础/a15e3c34d3b25e4095199081687ca4cc_MD5.jpeg|Open: Pasted image 20240305140812.png]]
![[assets/python：MySQL数据库基础/a15e3c34d3b25e4095199081687ca4cc_MD5.jpeg]]
	- 表数据操作命令
		- 增加
			- [[assets/python：MySQL数据库基础/dc5dc1b485a24370472224daf22518e0_MD5.jpeg|Open: Pasted image 20240305140819.png]]
![[assets/python：MySQL数据库基础/dc5dc1b485a24370472224daf22518e0_MD5.jpeg]]
			- ![[Pasted image 20240305140826.png]]
		- 修改查询表数据
			- [[assets/python：MySQL数据库基础/14be61c7da3d5a53bf30b7d89396c806_MD5.jpeg|Open: Pasted image 20240305140833.png]]
![[assets/python：MySQL数据库基础/14be61c7da3d5a53bf30b7d89396c806_MD5.jpeg]]
			- [[assets/python：MySQL数据库基础/c501659d8d80dc370633f6d258573bec_MD5.jpeg|Open: Pasted image 20240305140838.png]]
![[assets/python：MySQL数据库基础/c501659d8d80dc370633f6d258573bec_MD5.jpeg]]
		- 删除表数据
			- [[assets/python：MySQL数据库基础/56ad64af8e503fc74bbec6fb07882f3e_MD5.jpeg|Open: Pasted image 20240305140852.png]]
![[assets/python：MySQL数据库基础/56ad64af8e503fc74bbec6fb07882f3e_MD5.jpeg]]
			- [[assets/python：MySQL数据库基础/9ade6d70661a6a7a259f78ecdbb5fb04_MD5.jpeg|Open: Pasted image 20240305140858.png]]
![[assets/python：MySQL数据库基础/9ade6d70661a6a7a259f78ecdbb5fb04_MD5.jpeg]]
- 数据库数据查询语句-where
	- 使用where子句对表中的数据筛选，结果为true的记录会出现在结果集中。
	- 条件查询语法：
		- `select * from 表名 where 条件；`
		  例如：`select * from students where id=1;`
	- where后面支持多种运算符，进行条件的处理：
	- where之比较运算查询
		- 等于：=
		- 大于：>
		- 大于等于：>=
		- 小于：<
		- 小于等于：<=
		- 不等于：！= 或<>
		- ![[Pasted image 20240305140905.png]]
	- where之逻辑运算查询
		- and
		- or
		- not
		- [[assets/python：MySQL数据库基础/4f3bbab198e0666f2b63a5bcc1436b01_MD5.jpeg|Open: Pasted image 20240305140913.png]]
![[assets/python：MySQL数据库基础/4f3bbab198e0666f2b63a5bcc1436b01_MD5.jpeg]]
	- where之模糊查询
		- like 后跟：
			- % 表示任意多个任意字符
			- _ 表示一个任意字符
			- [[assets/python：MySQL数据库基础/0de8e36110e3cc3bdcfbbffd31f04140_MD5.jpeg|Open: Pasted image 20240305140920.png]]
![[assets/python：MySQL数据库基础/0de8e36110e3cc3bdcfbbffd31f04140_MD5.jpeg]]
	- where之范围查询
		- between-and 和in的区别
		- 范围查询分为连续范围查询和非连续范围查询：
			- in表示在一个非连续的范围内
			- between ... and ... 表示在一个连续的范围内
			- ![[Pasted image 20240305140927.png]]
	- where之空值判断
		- 判断为空： is null
		- `select * from 表名 where 字段 is null;`
		- 判断为非空： is not null
		- `select * from 表名 where 字段 is not null;`
		- #+BEGIN_QUOTE
		  null 与 ' ' 是不同的
		  #+END_QUOTE
	- order排序查询
		- `select * from 表名 order by 字段1 asc|desc  [, 字段2 asc|desc, ...]`
		- 语法说明：
			- 将行数据按照字段1进行排序，如果某些列1的值相同时，则按照列2排序，以此类推
			- asc从小到大排列，即升序
			- desc从大到小排序，即降序
			- 默认按照列值从小到大排列（即asc关键字）
			- [[assets/python：MySQL数据库基础/5ab0ab599f51e32d249da5195b7bb541_MD5.jpeg|Open: Pasted image 20240305140939.png]]
![[assets/python：MySQL数据库基础/5ab0ab599f51e32d249da5195b7bb541_MD5.jpeg]]
	- 聚合函数
		- 聚合函数作用：聚会函数会把当前所在表当作一个组进行统计
		- [[assets/python：MySQL数据库基础/0d7ca51cd4af1c663fb1c730f83ce538_MD5.jpeg|Open: Pasted image 20240305140944.png]]
![[assets/python：MySQL数据库基础/0d7ca51cd4af1c663fb1c730f83ce538_MD5.jpeg]]
		- 聚合函数的特点：
			- 每个组函数接收一个参数（字段名或者表达式）
			- 统计结果中默认忽略字段为NULL的记录
			- 不允许出现嵌套 比如sum(max(xx))
			- ![[Pasted image 20240305140952.png]]
- 数据库数据查询-group分组查询、limit限制查询、连接查询、子查询
	- group分组查询
		- 分组就是将一个’数据集‘划分成若干个’小区域‘，然后针对若干个’小区域‘进行数据处理。
		- group by：将查询结果按照1个或多个字段进行分组，字段值相同的为一组
		- group by可用于单个字段分组，也可以用于多个字段分组
		- [[assets/python：MySQL数据库基础/869369fd4b516b51b9cf0d921026009c_MD5.jpeg|Open: Pasted image 20240305141001.png]]
![[assets/python：MySQL数据库基础/869369fd4b516b51b9cf0d921026009c_MD5.jpeg]]{:height 448, :width 716}
	- limit限制查询
		- 可以使用limit限制取出记录的数量，但limit要写在sql语句的最后
		- `limit 起始记录，记录数`
		- 说明：
			- 起始记录是指从第几条记录开始取，第一条记录的下标是0
			- 记录数是指从起始记录开始向后依次去的记录数
			- [[assets/python：MySQL数据库基础/5292aa65982c1a5a48ef50043def96d7_MD5.jpeg|Open: Pasted image 20240305141007.png]]
![[assets/python：MySQL数据库基础/5292aa65982c1a5a48ef50043def96d7_MD5.jpeg]]
	- 连接查询 - 内连接
		- 当查询结果的数据来源于多张表时，需要将多张表连接成一个大的数据集进行汇总显示。
		- 内连接的查询结果为两个表符合条件匹配到的数据。
		- `select 字段 from 表1 inner join 表2 on 表1.字段1 = 表2.字段2`
		- 注意：
			- 内连接：根据连接条件取出两个表'交集'
			- on 是连接条件 where是连接后筛选条件
		- [[assets/python：MySQL数据库基础/7e50ec13836bf624a0337160d265cfbb_MD5.jpeg|Open: Pasted image 20240305141017.png]]
![[assets/python：MySQL数据库基础/7e50ec13836bf624a0337160d265cfbb_MD5.jpeg]]
	- 连接查询 - 外连接
		- 语法：
			- `主表 left join 从表 on 连接条件；`
			- `从表 right join 主表 on 连接条件；`
			- 注意：
				- 能够使用连接的前提是，多表之间有字段上的关联
				- 左连接和右连接区别在于主表在SQL语句中的位置，因此实际左连接就可以满足常见需求
			- [[assets/python：MySQL数据库基础/7a2b03b1370b9114924b371849aae2dd_MD5.jpeg|Open: Pasted image 20240305141035.png]]
![[assets/python：MySQL数据库基础/7a2b03b1370b9114924b371849aae2dd_MD5.jpeg]]
	- 连接查询 - 自连接
		- 使用自连接，只需要使用一个表，可以加开查询速度，减少数据表占用空间
		- [[assets/python：MySQL数据库基础/e24053d36bde4786dde5edb759734d25_MD5.jpeg|Open: Pasted image 20240305141040.png]]
![[assets/python：MySQL数据库基础/e24053d36bde4786dde5edb759734d25_MD5.jpeg]]
	- 子查询
		- 把一个查询的结果当作另一个查询的条件
		- 子查询分为三类：
			- 标量子查询：子查询返回的结果是一个数据（一行一列）
			- 列子查询：返回的结果是一列（一列多行）
			- 行子查询：返回的结果是一行（一行多列）
			- [[assets/python：MySQL数据库基础/7c7be8c47f1ac1e18c96b6b0676b61b0_MD5.jpeg|Open: Pasted image 20240305141045.png]]
![[assets/python：MySQL数据库基础/7c7be8c47f1ac1e18c96b6b0676b61b0_MD5.jpeg]]
-