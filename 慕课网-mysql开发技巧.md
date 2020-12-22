
# MYSQL开发技巧  


## 一.如何正确使用Join语句


### 常用的sql语句类型

DDL(数据定义语言)

* create、alter
  
TPL(事务处理语言)

* commit、rollback

DCL(数据控制语言)

* grant、revoke

DML(数据操作语言)

* select、update、insert、delete


<!--  <img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/join%E8%BF%9E%E6%8E%A5%E5%9B%BE.png" width = "508" height = "322" align=center /> -->

### 演示数据库表

CREATE TABLE `user1` (

  `id` int(11) NOT NULL COMMENT '主键',

  `user_name` varchar(255) DEFAULT NULL COMMENT '姓名',

  `over` varchar(255) DEFAULT NULL COMMENT '结局',

  PRIMARY KEY (`id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



INSERT INTO `user1`(`id`, `user_name`, `over`) VALUES (1, '唐僧', '旃檀功德佛');

INSERT INTO `user1`(`id`, `user_name`, `over`) VALUES (2, '猪八戒', '净坛使者');

INSERT INTO `user1`(`id`, `user_name`, `over`) VALUES (3, '孙悟空', '斗战胜佛');

INSERT INTO `user1`(`id`, `user_name`, `over`) VALUES (4, '沙僧', '金身罗汉');


***

CREATE TABLE `user2` (

  `id` int(11) NOT NULL COMMENT '主键',

  `user_name` varchar(255) DEFAULT NULL COMMENT '姓名',

  `over` varchar(255) DEFAULT NULL COMMENT '结局',

  PRIMARY KEY (`id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



INSERT INTO `user2`(`id`, `user_name`, `over`) VALUES (1, '孙悟空', '成佛');

INSERT INTO `user2`(`id`, `user_name`, `over`) VALUES (2, '牛魔王', '被降服');

INSERT INTO `user2`(`id`, `user_name`, `over`) VALUES (3, '蛟魔王', '被降服');

INSERT INTO `user2`(`id`, `user_name`, `over`) VALUES (4, '鹏魔王', '被降服');

INSERT INTO `user2`(`id`, `user_name`, `over`) VALUES (5, '狮驼王', '被降服');


***



### InnerJoin

把两个表中公共部分查询出来

 <img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/innerJoin.png" width = "580" height = "322" align=center /> 

```sql
select  u1.id,u1.user_name,u1.`over`,u2.`over` from user1 u1 inner join user2 u2 on u1.user_name = u2.user_name;
```

|id|username|a.over|b.over|
|--|--|--|--|
|3|孙悟空|斗战胜佛|成佛|


### leftJoin

左外连接，查询左表全部的数据和右表的交集

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/%E5%B7%A6%E5%A4%96%E8%BF%9E%E6%8E%A5.png" width = "520" height = "322" align=center />

```sql
select  a.id,a.user_name,a.`over`,b.`over` from user1 a left join  user2 b on a.user_name=b.user_name;
```

|id|username|a.over|b.over|
|--|--|--|--|
|3|孙悟空|斗战胜佛|成佛|
|1|唐僧|旃檀功德佛|NULL|
|2|猪八戒|净坛使者|NULL|
|4|沙僧|金身罗汉|NULL|

```sql
select  a.id,a.user_name,a.`over`,b.`over` from user1 a left join  user2 b on a.user_name=b.user_name where b.`over` is not null ;
```

|id|username|a.over|b.over|
|--|--|--|--|
|3 |孙悟空|斗战胜佛|成佛|

### RightJoin

和左外连接相反

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/%E5%8F%B3%E5%A4%96%E8%BF%9E%E6%8E%A5.png" width = "500" height = "322" align=center />

```sql
select  b.id,b.user_name,b.`over`,a.`over` from user1 a right join  user2 b on a.user_name=b.user_name;
```

|id|username|b.over|a.over|
|--|--|--|--|
|1|孙悟空|成佛|斗战胜佛|
|2|牛魔王|被降服|NULL|
|3|蛟魔王|被降服|NULL|
|4|鹏魔王|被降服|NULL|
|5|狮驼王|被降服|NULL|

```sql
select  b.id,b.user_name,b.`over`,a.`over` from user1 a right join  user2 b on a.user_name=b.user_name WHERE a.user_name is not null;
```

|id|username|b.over|a.over|
|--|--|--|--|
|1|孙悟空|成佛|斗战胜佛|

### FullJoin

全连接

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/%E5%85%A8%E8%BF%9E%E6%8E%A5.png" width = "550" height = "322" align=center />


**全连接mysql不支持的解决方案**

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/fullJoin%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E5%BC%8F.png" width = "550" height = "322" align=center />

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/fulljoin%E6%9F%A5%E8%AF%A2%E7%BB%93%E6%9E%9C.png" width = "700" height = "322" align=center />

### CrossJoin

交叉连接

```sql
select  a.user_name,a.`over`,b.user_name,b.`over` from user1 a cross join user2 b
```

<img src="https://raw.githubusercontent.com/AireyZXL/imageDepository/main/%E4%BA%A4%E5%8F%89%E8%BF%9E%E6%8E%A5.png" width = "700" height = "322" align=center />

### 使用join更新表

获取两张表中的都存在的数据更新这一条记录

```sql
update user1 set over='齐天大圣' where user1.user_name in (select b.user_name from user1 a join user2 b on a.user_name=b.user_name);
```
error:1093 

**正确更新方式**

```sql
update user1 a join
    (select b.user_name  from user1 a inner join user2 b on a.user_name=b.user_name) b on a.user_name=b.user_name
set a.`over`='齐天大圣' where 1=1
```


一级标题
==================

二级标题 
---------------

* * *

* List item

