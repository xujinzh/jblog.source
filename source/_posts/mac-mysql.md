---
title: Mac 上安装使用 MySQL
mathjax: true
author: Jinzhong Xu
date: 2021-05-03 17:15:55
tags:
- mac
- mysql
categories:
- technology
- mac
---

如何在 Mac （option + shift + k） 上安装使用 MySQL？这篇介绍在 Mac 上借助 Terminal 安装并使用 MySQL.

<!--more-->

# 下载 MySQL

官网下载地址：[MySQL Community Downloads](https://dev.mysql.com/downloads/mysql/)

我这里选择 **macOS 11 (x86, 64-bit), Compressed TAR Archive**，该压缩文件比较小，下载较快。

下载后是 mysql-8.0.24-macos11-x86_64.tar.gz

# 安装 MySQL

```bash
tar -xzf mysql-8.0.24-macos11-x86_64.tar.gz
mv mysql-8.0.24-macos11-x86_64 /usr/local/mysql
cd /usr/local/mysql

# 初始化，注意保存初始化密码。
# 再注意，如果第一次打开从网上下载该软件，需要在设置隐私中允许该软件运行
# 如果出错，可以删除mysql目录下的data文件夹
sudo bin/mysqld --initialize --user=jinzhongxu
# 查看 mysql 服务状态
sudo ./support-files/mysql.server status
sudo ./support-files/mysql.server start
sudo ./support-files/mysql.server stop

# 添加到环境变量
echo 'export PATH=$PATH:/usr/local/mysql/bin' >> ~/.zshrc
source ~/.zshrc

# 更换新密码，需要输入上面产生的初始化密码
mysqladmin -u root -p password xxx.yyy
```

# 使用 MySQL

```bash
# 输入上面设置的密码 xxx.yyy
mysql -u root -p
# 退出
mysql> exit
Bye

# 设置新用户
$ mysql -u root -p      
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 52
Server version: 8.0.24 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE USER 'jinzhongxu'@'localhost' IDENTIFIED BY 'xxx.zzz';
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT ALL PRIVILEGES ON * . * TO 'jinzhongxu'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye

# 创建新数据库和新表，并添加数据
$ mysql -u jinzhongxu -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 53
Server version: 8.0.24 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.00 sec)

mysql> CREATE DATABASE pytest;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| pytest             |
| sys                |
| test               |
+--------------------+
6 rows in set (0.00 sec)

mysql> USE pytest;
Database changed
mysql> CREATE TABLE user (id VARCHAR(20) primary key, name VARCHAR(20));
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO user (id, name) VALUES ("1", "JinzhongXu");
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM user WHERE id = '1';
+----+------------+
| id | name       |
+----+------------+
| 1  | JinzhongXu |
+----+------------+
1 row in set (0.00 sec)

mysql> exit
Bye


# 登陆数据库时直接进入指定数据库 pytest
$ mysql -h localhost  -u jinzhongxu -p pytest
Enter password: 
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 54
Server version: 8.0.24 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW TABLES;
+------------------+
| Tables_in_pytest |
+------------------+
| USER             |
+------------------+
1 row in set (0.00 sec)

mysql> INSERT INTO user (id, name) VALUES ("2", "WJL");
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM user WHERE id >= '1';
+----+------------+
| id | name       |
+----+------------+
| 1  | JinzhongXu |
| 2  | WJL        |
+----+------------+
2 rows in set (0.00 sec)

mysql> exit
Bye

# 查看用户权限
$ mysql -u jinzhongxu -p                     
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 55
Server version: 8.0.24 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW GRANTS FOR 'jinzhongxu'@'localhost';
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Grants for jinzhongxu@localhost                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, SHUTDOWN, PROCESS, FILE, REFERENCES, INDEX, ALTER, SHOW DATABASES, SUPER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER, CREATE TABLESPACE, CREATE ROLE, DROP ROLE ON *.* TO `jinzhongxu`@`localhost`                                                                                                                                                                                                                    |
| GRANT APPLICATION_PASSWORD_ADMIN,AUDIT_ADMIN,BACKUP_ADMIN,BINLOG_ADMIN,BINLOG_ENCRYPTION_ADMIN,CLONE_ADMIN,CONNECTION_ADMIN,ENCRYPTION_KEY_ADMIN,FLUSH_OPTIMIZER_COSTS,FLUSH_STATUS,FLUSH_TABLES,FLUSH_USER_RESOURCES,GROUP_REPLICATION_ADMIN,INNODB_REDO_LOG_ARCHIVE,INNODB_REDO_LOG_ENABLE,PERSIST_RO_VARIABLES_ADMIN,REPLICATION_APPLIER,REPLICATION_SLAVE_ADMIN,RESOURCE_GROUP_ADMIN,RESOURCE_GROUP_USER,ROLE_ADMIN,SERVICE_CONNECTION_ADMIN,SESSION_VARIABLES_ADMIN,SET_USER_ID,SHOW_ROUTINE,SYSTEM_USER,SYSTEM_VARIABLES_ADMIN,TABLE_ENCRYPTION_ADMIN,XA_RECOVER_ADMIN ON *.* TO `jinzhongxu`@`localhost` |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> quit;
Bye
(base) 

# 其他常用命令
# 更改用户密码
mysqladmin -u jinzhongxu -p password com.xxx
# 删除用户
mysql> DROP USER 'jinzhongxu'@'localhost';
# 删除数据库
mysql> DROP DATABASE pytest;
```

# 重启 MySQL SERVER



```bash
# 这里不用sudo，使用sudo报错：The server quit without updating PID file
/usr/local/mysql/support-files/mysql.server restart

# 添加环境变量
echo 'export PATH=$PATH:/usr/local/mysql/support-files' >> ~/.zshrc
source ~/.zshrc

# 添加环境变量后启动 mysql server
mysql.server status
mysql.server start
mysql.server restart
```



# Python 连接 MySQL 数据库

```bash
# 安装包
pip install mysql-connector-python
pip install sqlalchemy
```



```python
import mysql.connector
from sqlalchemy import Column, String, create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base



def mysql_(check_id, create_p=False, insert_p=False, ids=1, name="JinzhongXu"):
    conn = mysql.connector.connect(user='jinzhongxu', host="localhost", password='yourpassword',
                                   auth_plugin='mysql_native_password',
                                   database='pytest')
    cursor = conn.cursor()
    # 创建 table
    if create_p:
        cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
    # 添加数据
    # cursor.execute('insert into user (id, name) values (%s, %s)', ['1', 'JinzhongXu'])
    if insert_p:
        cursor.execute('insert into user (id, name) values (%s, %s)', [str(ids), name])
    # cursor.rowcount
    conn.commit()
    cursor.close()
    cursor = conn.cursor()
    cursor.execute('select * from user where id = %s', (str(check_id),))
    values = cursor.fetchall()
    print(values)
    cursor.close()
    conn.close()


# 定义User对象:
class User(declarative_base()):
    # 表的名字:
    __tablename__ = 'user'

    # 表的结构:
    id = Column(String(20), primary_key=True)
    name = Column(String(20))


def sqlalchemy_(check_id, insert_p=False, ids=1, name='Jinzhongxu'):
    # 初始化数据库连接:
    engine = create_engine('mysql+mysqlconnector://jinzhongxu:yourpassword@localhost:3306/pytest')
    # 创建DBSession类型:
    DBSession = sessionmaker(bind=engine)
    # 创建session对象:
    session = DBSession()
    if insert_p:
        # 创建新User对象:
        new_user = User(id=str(ids), name=name)
        # 添加到session:
        session.add(new_user)
        # 提交即保存到数据库:
        session.commit()
    # 关闭session:
    session.close()

    # 创建Session:
    session = DBSession()
    # 创建Query查询，filter是where条件，最后调用one()返回唯一行，如果调用all()则返回所有行:
    user = session.query(User).filter(User.id == str(check_id)).one()
    # 打印类型和对象的name属性:
    print('type:', type(user))
    print('name:', user.name)
    # 关闭Session:
    session.close()


def main():
    # mysql_(check_id=1)
    sqlalchemy_(check_id=1)


if __name__ == '__main__':
    main()
```



# 参考链接

1. [How To Create a New User and Grant Permissions in MySQL](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql)

2. [How to Create New MySQL User](https://phoenixnap.com/kb/how-to-create-new-mysql-user-account-grant-privileges)

3. [Creating and Selecting a Database](https://dev.mysql.com/doc/refman/5.7/en/creating-database.html)

   
   
   