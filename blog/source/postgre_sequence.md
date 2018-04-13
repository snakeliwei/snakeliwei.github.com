title: Postgre sequence
tags:
  - Postgre
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-02-26 14:36:19
author: me
cover: "-/images/zsh.png"
---
sequence 是一种特殊的数据库对象，用来产生独一无二的数字ID。Postgres中有一种数据类型serial，和sequence对应: 
![sequence1](http://blog.chinaunix.net/attachment/201311/2/24774106_1383400523Gg17.png)

如果我们插入一笔记录，我们希望系统自动为我们分配一个ID，那么我们需要使用serial type。如果我们创建表的时候，自动创建一个serial类型（包smallserial and bigserial），系统会自动帮我们创建sequence这种数据库对象。
```sql
create table employ(
      id serial,
      name varchar(64),
      department varchar(64)
) ;

manu_db=# create table employ(id serial,name varchar(64),department varchar(128)) ;
NOTICE:  CREATE TABLE will create implicit sequence "employ_id_seq" for serial column "employ.id"
CREATE TABLE
manu_db=# \d
             List of relations
 Schema |     Name      |   Type   | Owner 
--------+---------------+----------+-------
 public | employ        | table    | manu
 public | employ_id_seq | sequence | manu
(2 rows)
manu_db-# \d employ
                                   Table "public.employ"
   Column   |          Type          |                      Modifiers                      
------------+------------------------+-----------------------------------------------------
 id         | integer                | not null default nextval('employ_id_seq'::regclass)
 name       | character varying(64)  | 
 department | character varying(128) | 
```
我们看到，postgres自动帮助我们创建了一个sequence，命名原则是 tablename_columnname_seq.
下面这两条SQL，本质是相等：
```sql
CREATE TABLE employ (
      id serial,
      name varchar(64),
      department varchar(64)
) ;
```
```sql
CREATE SEQUENCE employ_id_seq ;
CREATE TABLE employ (
     id integer NOT NULL DEFAULT nextval('employ_id_seq'),
     name varchar(64),
     department varchar(64)
);

ALTER SEQUENCE employ_id_seq OWNED by employ.id;
```
我们看到，我们用了integer数据类型和serial对应起来，默认用nextal（‘employ_id_seq’）给integer类型的id赋值，那是因为integer范围要比serial的范围大，这样做是安全的。对于bigserial，我们应该用bigint类型，对于smallserial应该何smallint对应。
![sequence2](http://blog.chinaunix.net/attachment/201311/2/24774106_1383403462iqY7.png)    
我们建立数据表和sequence的对应关系，常规的就是这两种方法，要么创建表的时候，某字段是serial类型，要么创建先创建sequence，然后创建表的时候，将某字段和sequence 联系起来（DEFAULT nextvl（‘XXXX_seq’））,有没有第三种方法？假如创建表的时候，没有用DEFAULT nextval建立起来联系，还有没有机会在后面建立这种联系？我为什么纠结这个？ 
```sql
manu_db=# create table employ_copy (like  employ) ;
CREATE TABLE
manu_db=# \d employ_copy 
           Table "public.employ_copy"
   Column   |          Type          | Modifiers 
------------+------------------------+-----------
 id         | integer                | not null
 name       | character varying(64)  | 
 department | character varying(128) | 


manu_db=# \d employ
                                   Table "public.employ"
   Column   |          Type          |                      Modifiers                      
------------+------------------------+-----------------------------------------------------
 id         | integer                | not null default nextval('employ_id_seq'::regclass)
 name       | character varying(64)  | 
 department | character varying(128) | 


manu_db=# 
```
如果业务需要，创建一个schema和employ一模一样的table，我们只能够用like，但是，我们看到，id这个字段并不完全一样，原因就是没有sequence和新建的table对应。这种情况，我们有没有办法，事后创建sequence，建立table中id字段的联系， I mean  id use nextval as default？
答案是肯定的：
```sql
CREATE SEQUENCE employ_copy_id_seq ;
ALTER TABLE employ_copy ALTER COLUMN id SET DEFAULT nextval('employ_copy_id_seq');
ALTER SEQUENCE employ_copy_id_seq  OWNED by employ_copy.id ;
```
    看看效果：  
```sql
manu_db=# CREATE SEQUENCE employ_copy_id_seq ;
CREATE SEQUENCE
manu_db=# ALTER TABLE employ_copy ALTER COLUMN id SET DEFAULT nextval('employ_copy_id_seq');
ALTER TABLE
manu_db=# \d employ_copy
                                   Table "public.employ_copy"
   Column   |          Type          |                        Modifiers                         
------------+------------------------+----------------------------------------------------------
 id         | integer                | not null default nextval('employ_copy_id_seq'::regclass)
 name       | character varying(64)  | 
 department | character varying(128) | 
```

注意，加了alter sequence owned by ,在删除table的时候，会自动的将sequence删掉。如果不建立这种owned by的关系，删除table，不会引发sequence被删除。我就不贴了，可以自己try。

当我们向Postgres插入一笔记录的时候，我们不需要费力告诉DB id的值，因为DB会自动分配一个数值给id：
```sql
manu_db=# insert into  employ (name,department) values('bean', 'DDI');
INSERT 0 1
manu_db=# insert into  employ (name,department) values('albert', 'DDI');
INSERT 0 1
manu_db=# select * from employ ;
 id |  name  | department 
----+--------+------------
  1 | bean   | DDI
  2 | albert | DDI
(2 rows)
```
我们可以用currval 和 nextval取到sequence的当前值和下一个值。注意，currval是不会改变sequence的，但是nextval一旦使用，下个数字就被消耗掉了，哪怕你并没有真正的用于你的table。
```sql
manu_db=# select currval('employ_id_seq');
 currval 
---------
       2
(1 row)

manu_db=# select nextval('employ_id_seq');
 nextval 
---------
       3
(1 row)

manu_db=# select nextval('employ_id_seq');
 nextval 
---------
       4
(1 row)
```
使用setval可以设置下一个当前值，下一次就会从你设置的这个值开始递增：
```sql  
manu_db=# select setval('employ_id_seq',100);
 setval 
--------
    100
(1 row)

manu_db=# select currval('employ_id_seq');
 currval 
---------
     100
(1 row)

manu_db=# select nextval('employ_id_seq');
 nextval 
---------
     101
(1 row)
```
***
### 快速修改id的自增起始数为当前最大的id
- 使用语句: ```select setval('your_table_id_seq',(select max(id) from <tablename>));```
- 如何查看: ```your_table_id_seq?```
- 使用命令: ```\d <tablename>```

>  转载自[Bean_lee 's PostgreSQL之sequence](http://blog.chinaunix.net/uid-24774106-id-3973781.html)

