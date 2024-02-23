# [工作在浏览器上人-YangBobin](https://www.cnblogs.com/springsnow)

知识不在广泛，在于精通。知识不在积累，在于消化。 学习不在激情，在于坚持。书不在多，一两本真正看懂就行。书读百遍，其义自现。

随笔 - 893, 文章 - 1, 评论 - 79, 阅读 - 154万

## [C#操作SQLite数据库](https://www.cnblogs.com/springsnow/p/13072915.html)

**目录**

- [一、SQLite介绍](https://www.cnblogs.com/springsnow/p/13072915.html#_label0)
    - [1、SQLite 简介](https://www.cnblogs.com/springsnow/p/13072915.html#_label0_0)
    - [2、为什么要用 SQLite？](https://www.cnblogs.com/springsnow/p/13072915.html#_label0_1)
    - [3、SQLite 局限性](https://www.cnblogs.com/springsnow/p/13072915.html#_label0_2)
    - [4、SQLite GUI客户端列表：](https://www.cnblogs.com/springsnow/p/13072915.html#_label0_3)
- [二、C#操作数据库](https://www.cnblogs.com/springsnow/p/13072915.html#_label1)
    - [1、关于SQLite的connection string：](https://www.cnblogs.com/springsnow/p/13072915.html#_label1_0)
    - [2、C#下SQLite操作](https://www.cnblogs.com/springsnow/p/13072915.html#_label1_1)
- [三、下载地址：](https://www.cnblogs.com/springsnow/p/13072915.html#_label2)

---

## 一、SQLite介绍

### 1、SQLite 简介

SQLite是一个开源、免费的小型RDBMS(关系型数据库)，能独立运行、无服务器、零配置、支持事物，用C实现，内存占用较小，支持绝大数的SQL92标准。

这意味着与其他数据库一样，您不需要在系统中配置。SQLite 引擎不是一个独立的进程，可以按应用程序需求进行静态或动态连接。SQLite 直接访问其存储文件。

SQLite 源代码不受版权限制。SQLite数据库官方主页：[http://www.sqlite.org/index.html](http://www.sqlite.org/index.html)

[![[assets/Csharp操作SQLite数据库/388ff5c2c87a46ae21742dc5159afdb7_MD5.jpg|"image"]]](https://img2020.cnblogs.com/blog/24244/202006/24244-20200610112534316-912147425.png)

### 2、为什么要用 SQLite？

- 不需要一个单独的服务器进程或操作的系统（无服务器的）。
- SQLite 不需要配置，这意味着不需要安装或管理。
- 一个完整的 SQLite 数据库是存储在一个单一的跨平台的磁盘文件。
- SQLite 是非常小的，是轻量级的，完全配置时小于 400KiB，省略可选功能配置时小于250KiB。
- SQLite 是自给自足的，这意味着不需要任何外部的依赖。
- SQLite 事务是完全兼容 ACID 的，允许从多个进程或线程安全访问。
- SQLite 支持 SQL92（SQL2）标准的大多数查询语言的功能。
- SQLite 使用 ANSI-C 编写的，并提供了简单和易于使用的 API。
- SQLite 可在 UNIX（Linux, Mac OS-X, Android, iOS）和 Windows（Win32, WinCE, WinRT）中运行。

### 3、SQLite 局限性

在 SQLite 中，SQL92 不支持的特性如下所示：

[![[assets/Csharp操作SQLite数据库/d00d105f8bc9e30cd43adcacd6843d0f_MD5.jpg|"image"]]](https://img2020.cnblogs.com/blog/24244/202006/24244-20200610112535928-883478624.png)

### 4、SQLite GUI客户端列表：

- SQLite Expert ：[SQLite administration | SQLite Expert](http://www.sqliteexpert.com/download.html)
- SQLite Administrator：[http://download.orbmu2k.de/files/sqliteadmin.zip](http://download.orbmu2k.de/files/sqliteadmin.zip)
- SQLiteStudio：
- SQLite Database Browser：
- Navicat Premium ：[Navicat | 下载 Navicat for SQLite 14 天免费 Windows、macOS 和 Linux 的试用版](https://www.navicat.com.cn/download/navicat-for-sqlite)

[![[assets/Csharp操作SQLite数据库/b764931bb1143dcfd07cac132782a071_MD5.jpg|"image"]]](https://img2020.cnblogs.com/blog/24244/202011/24244-20201130161403038-621379271.png)

## 二、C#操作数据库

### 1、关于SQLite的connection string：

[http://www.connectionstrings.com/sqlite/](http://www.connectionstrings.com/sqlite/)

基本方式：Data Source=c:\mydb.db;Version=3;

### 2、C#下SQLite操作

驱动dll下载：[System.Data.SQLite](http://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki)

也Nuget 直接搜索安装：System.Data.SQLite.Core

[NuGet Gallery | SQLite](https://www.nuget.org/profiles/SQLite)

![[assets/Csharp操作SQLite数据库/0b3264e9682d5d4a19e027a539386280_MD5.jpg]]

C#使用SQLite步骤：

（1）新建一个project

（2）添加SQLite操作驱动dll引用:System.Data.SQLite.dll

（3）使用API操作SQLite DataBase

```
using System;
using System.Data.SQLite;

namespace SQLiteSamples
{
    class Program
    {
        //数据库连接
        SQLiteConnection m_dbConnection;

        static void Main(string[] args)
        {
            Program p = new Program();
        }

        public Program()
        {
            createNewDatabase();
            connectToDatabase();
            createTable();
            fillTable();
            printHighscores();
        }

        //创建一个空的数据库
        void createNewDatabase()
        {
            SQLiteConnection.CreateFile("MyDatabase.db");//可以不要此句
        }

        //创建一个连接到指定数据库
        void connectToDatabase()
        {
            m_dbConnection = new SQLiteConnection("Data Source=MyDatabase.db;Version=3;");//没有数据库则自动创建
            m_dbConnection.Open();
        }

        //在指定数据库中创建一个table
        void createTable()
        {
            string sql = "create table  if not exists highscores (name varchar(20), score int)";
            SQLiteCommand command = new SQLiteCommand(sql, m_dbConnection);
            command.ExecuteNonQuery();
        }

        //插入一些数据
        void fillTable()
        {
            string sql = "insert into highscores (name, score) values ('Me', 3000)";
            SQLiteCommand command = new SQLiteCommand(sql, m_dbConnection);
            command.ExecuteNonQuery();

            sql = "insert into highscores (name, score) values ('Myself', 6000)";
            command = new SQLiteCommand(sql, m_dbConnection);
            command.ExecuteNonQuery();

            sql = "insert into highscores (name, score) values ('And I', 9001)";
            command = new SQLiteCommand(sql, m_dbConnection);
            command.ExecuteNonQuery();
        }

        //使用sql查询语句，并显示结果
        void printHighscores()
        {
            string sql = "select * from highscores order by score desc";
            SQLiteCommand command = new SQLiteCommand(sql, m_dbConnection);
            SQLiteDataReader reader = command.ExecuteReader();
            while (reader.Read())
                Console.WriteLine("Name: " + reader["name"] + "\tScore: " + reader["score"]);
            Console.ReadLine();
        }
    }
}
```
[![[assets/Csharp操作SQLite数据库/8ef6892be1e317dec3948a4e7f2f3845_MD5.jpg|"image"]]](https://img2020.cnblogs.com/blog/24244/202011/24244-20201130161404492-1370159528.png)

## 三、下载地址：

链接：[https://pan.baidu.com/s/1iryCFW05hu-U_ZDZkMFA6A](https://pan.baidu.com/s/1iryCFW05hu-U_ZDZkMFA6A)  
提取码：q5xy
