
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



INSERT INTO `blog`.`user1`(`id`, `user_name`, `over`) VALUES (1, '唐僧', '旃檀功德佛');

INSERT INTO `blog`.`user1`(`id`, `user_name`, `over`) VALUES (2, '猪八戒', '净坛使者');

INSERT INTO `blog`.`user1`(`id`, `user_name`, `over`) VALUES (3, '孙悟空', '斗战胜佛');

INSERT INTO `blog`.`user1`(`id`, `user_name`, `over`) VALUES (4, '沙僧', '金身罗汉');


***

CREATE TABLE `user2` (

  `id` int(11) NOT NULL COMMENT '主键',

  `user_name` varchar(255) DEFAULT NULL COMMENT '姓名',

  `over` varchar(255) DEFAULT NULL COMMENT '结局',

  PRIMARY KEY (`id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



INSERT INTO `blog`.`user2`(`id`, `user_name`, `over`) VALUES (1, '孙悟空', '成佛');

INSERT INTO `blog`.`user2`(`id`, `user_name`, `over`) VALUES (2, '牛魔王', '被降服');

INSERT INTO `blog`.`user2`(`id`, `user_name`, `over`) VALUES (3, '蛟魔王', '被降服');

INSERT INTO `blog`.`user2`(`id`, `user_name`, `over`) VALUES (4, '鹏魔王', '被降服');

INSERT INTO `blog`.`user2`(`id`, `user_name`, `over`) VALUES (5, '狮驼王', '被降服');


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




一级标题
==================

二级标题 
---------------

* * *

* List item

