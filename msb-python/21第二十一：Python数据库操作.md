# Python数据库操作

## 一、补充Mysql8的命令

```shell
# 创建用户
CREATE USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';

# 修改用户
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';

# 查询所有的用户
select user,host from mysql.user;

# 给用户授权
GRANT ALL ON *.* TO `wangwei`@`127.0.0.1` WITH GRANT OPTION;

# 执行完授权，需要执行flush privileges 指令，才能使新增权限生效。
flush privileges; //权限刷新

create table ti(
id int auto_increment primary key，
name varchar(20),
age tinyint unsigned
)
#unsigned 从正数开始 例如： tinyint -127 - 127  | tinyint unsigned 0-255
#primary key  设置主键（仅有一个）
#auto_increment 自动变化

#int  
#smallint
#midiumin
#tinyint
#bigint

```

## 二、Python操作数据库的三个库

### 1、MySQLdb

MySQLdb又叫MySQL-python ，是 Python 连接 MySQL 的一个驱动，很多框架都也是基于此库进行开发，遗憾的是它**只支持 Python2.x**，而且安装的时候有很多前置条件，因为它是基于C开发的库，在 Windows 平台安装非常不友好，经常出现失败的情况，现在基本不推荐使用，取代的是它的衍生版本。

### 2、Mysqlclient

由于 MySQLdb 年久失修，后来出现了它的 Fork 版本 mysqlclient，完全兼容 MySQLdb，同时支持 Python3.x，是 Django ORM的依赖工具，如果你想使用原生 SQL 来操作数据库，那么推荐此驱动。Mysqlclient是一个C扩展模块,编译安装可能会导致报各种错误。

```shell
pip install mysqlclient
```

官方下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient

```python

import MySQLdb

db = MySQLdb.connect(host="127.0.0.1", port=3306, db="test1", user="root", password="123123", charset='utf8')
# cursor = db.cursor(MySQLdb.cursors.DictCursor)
cursor = db.cursor()
# 使用cursor()方法获取操作游标

# SQL 插入语句
sql="insert into t_test(name, age) values(%s,%s)"
try:
    # 执行sql语句
    cursor.executemany(sql, [('Smith', 15), ('Mac', 20)])
    # 提交到数据库执行
    db.commit()

	#cursor.execute("select * from t_test")
	#data = cursor.fetchall()

	#print(data, type(data))
except Exception as ex:
    print(ex)
    # Rollback in case there is any error
    db.rollback()
# 关闭数据库连接
db.close()
```

### 3、PyMySQL

PyMySQL是纯 Python 实现的驱动，速度上比不上 MySQLdb，最大的特点可能就是它的安装方式没那么繁琐。

直接使用pip进行安装，基本不会报错。

pip install pymysql

```python
import pymysql

db = pymysql.connect(
    host="localhost", 
    port=3306,
    user='root',    #在这里输入用户名
    password='888888',     #在这里输入密码
    charset='utf8mb4' 
    ) #连接数据库

cursor = db.cursor() #创建游标对象

sql = 'show databases' #sql语句

cursor.execute(sql)  #执行sql语句

one = cursor.fetchone()  #获取一条数据
print('one:',one)

many = cursor.fetchmany(3) #获取指定条数的数据，不写默认为1
print('many:',many)

all = cursor.fetchall() #获取全部数据
print('all:',all)

cursor.close()  
db.close()  #关闭数据库的连接

```

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1664001726018/412c9ffd57a44c46aaa0567f3a26ec93.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1664001726018/7c5943e393da47f58c967ea9f6c87cbd.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1664001726018/6f54dcebf981458a86017372da703805.png)

## 三、基于mysqlclient的封装

```
import logging
import MySQLdb

from MySQLdb import cursors


class MySQLdbUtil(object):

    def __init__(self, host, user, passwd, db, port):
        self.host = host
        self.user = user
        self.passwd = passwd
        self.db = db
        self.port = port
        self.cursor = None
        self.conn = None

    def get_connection(self):
        self.conn = MySQLdb.connect(self.host, self.user, self.passwd, self.db, self.port)

    def select_one(self, sql, args=None):
        try:
            self.get_connection()
            self.cursor = self.conn.cursor(cursors.DictCursor)
            self.cursor.execute(sql, args=args)
            data = self.cursor.fetchone()
            return data
        except Exception as e:
            logging.error(e)
        finally:
            self.close()

    def select_many(self, sql, args=None):
        try:
            self.get_connection()
            self.cursor = self.conn.cursor(cursors.DictCursor)
            self.cursor.execute(sql, args=args)
            data = self.cursor.fetchall()
            return data
        except Exception as e:
            logging.error(e)
        finally:
            self.close()

    def dml_operation(self, sql, args=None):
        try:
            self.get_connection()
            cursor = self.conn.cursor()
            count = cursor.execute(sql, args=args)
            self.conn.commit()
            return count
        except Exception as e:
            logging.error(e)
            self.conn.rollback()
        finally:
            self.close()

    def close(self):
        if self.cursor:
            self.cursor.close()
        if self.conn:
            self.conn.close()


if __name__ == '__main__':
    client = MySQLdbUtil("127.0.1.1", "root", "123123", "test1", 3306)
    # result = client.select_many("select * from t_test;")
    # result = client.select_one("select * from t_test;")
    # result = client.dml_operation("update t_test set name=%s where id=%s", ('张三', 3))
    # result = client.dml_operation("insert into t_test(name, age) values(%s, %s)", ('张三33', 33))
    result = client.dml_operation("delete from t_test where id=%s", (4,))
    print(result)

```
