# Mysql

### 1.启动mysql

* net start mysql
* mysql -uroot -p
* 输入密码（111111）后按enter
* 输入（exit)或者（quit)可退出数据库；

### 2.基本操作

* 显示mysql中有几个数据库：

  ```mysql
  show databases;
  ```

* 创建数据库：

  ```mysql
  create database mytest character set utf8;
  ```

* 删除数据库

  ```mysql
  drop database mytest;
  ```

* 进入一个数据库

  ```mysql
  use mytest;
  ```

* 显示数据库有几个表

  ```mysql
  show tables;
  ```

* 显示表的属性信息

  ```mysql
  desc +表名
  ```

* 显示表的内容

  ```mysql
  select * from test;
  ```

* 给创建的数据库的某个表格改名

  ```mysql
  rename table 数据库名.原表名 to 数据库名.新表名;
  rename table mytest.mytest to mytest.user;
  ```

  

* 创建一个表

  * 并指明主键；并且指明某属性是否能为空值，以及是否有默认值

  ```mysql
  create table moviestar(
      name char(30) primary key,
      address varchar(255),
      gender char(1) not null default 'F',//先说明是否能为空，再说明默认值是什么；也可以指说明其中一项
      birthday date
  );
  ```

  * 设置某一个属性可以自增

    ```mysql
    
    		create table uesrTset(
        -> uId int(11)  not null auto_increment,
        -> uName varchar(255) not null,
        -> uPw varchar(255) not null,
        -> uSchool varchar(255) not null,
        -> uDepartment varchar(255) not null,
        -> primary key(uId)
        -> )engine=Innodb auto_increment=5 default charset=utf8;
    ```

  * 键中有多个属性时，必须使用下面这种声明主键的必须放到末尾

    ```mysql
    create table moviestar(
        name char(30),
        address varchar(255),
        gender char(1),
        birthday date,
        primary key(name)//括号里面可以放多个属性
    );
    ```

  * 创建表时，声明外键的方法

    ```mysql
    Studio(name,address,presc)
    movieExec(name,address,cret,nrtworth)
    create table studio(
        name char(30),
        address varchar(255),
        presc int references movieExec(cret)//引用另一个关系的键作为外键
    ); 
    //也可这样单独声明
    create table studio(
        name char(30),
        address varchar(255),
        presc int ,
        foreign key references movieExec(cret)
    ); 
    ```

  * 向表中增加字段：

    ```mysql
    alter table r(表名)  add  name(字段)  varchar(30)（字段类型）;
    
    //在添加字段的时候，可以设置默认值，以及是否为空
    
    alter table r add name1  varchar(30) not null ;
    
    alter table r add name2  varchar(30) not null default 'Tom' ;
    ```

  * 给表中的某个字段改名

    ```mysql
    ALTER TABLE 表名 CHANGE  旧字段名 新字段名 新数据类型;
    eg:alter table t_user2 change password  pwd  varchar(255);
    ```

  * 向这个表里添加元组

    ```mysql
    insert into +表名（字段1，字段2，字段3...)values (值1，值2，值3)
    insert into user (name,id,pwd) values ('admin',1,123456);//char类型单双引号都行
    ```

### 3.数据库查询语言

