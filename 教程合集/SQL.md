# SQL Learning

- [SQL Learning](#sql-learning)
  - [检索数据](#检索数据)
  - [排序检索数据](#排序检索数据)
  - [过滤数据](#过滤数据)
  - [高级数据过滤](#高级数据过滤)
  - [通配符过滤](#通配符过滤)
  - [创建计算字段](#创建计算字段)
  - [使用函数处理数据](#使用函数处理数据)
  - [汇总数据](#汇总数据)
  - [分组数据](#分组数据)
  - [使用子查询](#使用子查询)
  - [联结 (join) 表](#联结-join-表)
  - [创建高级联结](#创建高级联结)
  - [组合查询](#组合查询)
  - [插入数据](#插入数据)
  - [更新和删除数据](#更新和删除数据)
  - [创建和操纵表](#创建和操纵表)
  - [使用视图](#使用视图)
  - [使用储存过程](#使用储存过程)
  - [管理事务处理](#管理事务处理)
  - [使用游标](#使用游标)
  - [高级 SQL 特性](#高级-sql-特性)
- [大厂面试题](#大厂面试题)
  - [某音](#某音)

## 检索数据

- Select 被检索对象 From 数据库
- Select Dinstinct 被检索对象 From 数据库 `去重检索`
- Select * From 数据库 `检索全部内容`
- Select TOP 5 prod_name From 数据库 `限制结果`
- `-- 注释内容`
- 全部注释`/*跨行注释*/`

## 排序检索数据

- Select 被检索对象 From 数据库 ORDER BY 依据顺序

```sql
SELECT Cuit
FROM Customers
ORDER BY Cuit;
```

- Select 列1 列2 列3 From 数据库 ORDER BY 列1 列3

```sql
SELECT Cuit, Messi, Dibala
FROM Customers
ORDER BY Cuit, Dibala;
```

- 按相对位置排序

```sql
SELECT Cuit, Messi, Dibala
FROM Customers
ORDER BY 1, 3;
```

但是这个容易定位列不准，不建议常用

- 排序默认升序，降序排列需关键词 DESC 或写全称 DESCENDING

```=sql
SELECT Cuit, Messi, Dibala
FROM Customers
ORDER BY Messi DESC;
```

多个需要降序，每个后面都要写 DESC，否则默认升序。

## 过滤数据

- 筛选特定条件

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

> ORDER BY 位于 Where 之后
操作符：!< 不小于； <> 不等于; BETWEEN AND 介于两个值之间
IS NULL 为 NULL 值

- 单引号用于限定字符串

```sql
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';
```

- 习题答案

```sql
SELECT DISTINCT prod_name, prod_price
From Products
Where prod_price BETWEEN 3 AND 6
ORDER BY prod_price;
```

## 高级数据过滤

- AND 和 OR 操作符, AND 优于 OR, 圆括号优于二者

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```

- IN 操作符用来指定条件范围

```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01','BRS01')
ORDER BY prod_name;
```

- NOT 操作符与其他符号一起使用

- 习题答案

```sql
SELECT order_num, prod_id, quantity
FROM OrderItems
Where quantity >= 100 AND prod_id IN ('BR01', 'BR02', 'BR03')
ORDER BY prod_id, quantity DESC;
```

## 通配符过滤

- %表示任何字符出现任意次数

```sql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '%bean bag%';
```

- _只匹配单个字符

- 方括号[]通配符用来指定一个字符集，它必须匹配指定位置（通配
符的位置）的一个字符。

```sql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%' --J 或 M 开头的内容
ORDER BY cust_contact;
```

- 习题答案

```sql
SELECT prod_name, prod_desc
FROM Products
Where prod_desc NOT LIKE '%toy%'
ORDER BY prod_name
```

## 创建计算字段

- 拼接字段，用+或||

```sql
SELECT vend_name + '(' + vend_country + ')'
FROM Vendors
ORDER BY vend_name;
```

```sql
SELECT vend_name || '(' || vend_country || ')'
FROM Vendors
ORDER BY vend_name;
```

- 去掉输出的空格

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
FROM Vendors
ORDER BY vend_name;
```

```sql
SELECT RTRIM(vend_name) || ' (' || RTRIM(vend_country) || ')'
FROM Vendors
ORDER BY vend_name;
```

- 使用别名（可以认为是提取出来的表的名字），用 as

```sql
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
AS vend_title
FROM Vendors
ORDER BY vend_name;
```

```sql
SELECT RTRIM(vend_name) || ' (' || RTRIM(vend_country) || ')'
AS vend_title
FROM Vendors
ORDER BY vend_name;
```

```mysql
SELECT Concat(RTrim(vend_name), ' (',
RTrim(vend_country), ')') AS vend_title
FROM Vendors
ORDER BY vend_name;
```

- 运算

```sql
SELECT prod_id,
quantity,
item_price,
quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

- 习题答案

```sql
SELECT vend_id,
vend_name AS vname,
vend_address AS vaddress,
vend_city AS vcity
FROM Vendors
ORDER BY vname
```

```sql
SELECT prod_id, prod_price,
prod_price* 0.9 AS sale_price
FROM Products
```

## 使用函数处理数据

- 练习答案

```sql
SELECT cust_id, cust_name,
upper(concat(left(cust_contact,2),left(cust_city,3))) AS user_login
From Customers;
```

```sql
SELECT order_num, order_date
From Orders
Where YEAR(order_date)= 2020 AND MONTH(order_date)= 01
ORDER BY order_date
```

## 汇总数据

- AVG

```sql
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

- COUNT

```sql
SELECT COUNT(*) AS num_cust
FROM Customers;
```

- MAX

```sql
SELECT MAX(prod_price) AS max_price
FROM Products;
```

- MIN

```sql
SELECT MIN(prod_price) AS min_price
FROM Products;
```

- SUM

```sql
SELECT SUM(quantity) AS items_ordered
FROM OrderItems
WHERE order_num = 20005;
```

## 分组数据

- 创建分组 group by

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```

- where 过滤行, having 过滤分组

```sql
SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2; --两个以上订单的那些分组
```

- 分组排序

```sql
SELECT order_num, COUNT(*) AS items
FROM OrderItems
GROUP BY order_num
HAVING COUNT(*) >= 3
ORDER BY items, order_num;
```

- 课后练习答案

```sql
SELECT order_num, COUNT(order_num) AS order_lines
FROM OrderItems
GROUP BY order_num
ORDER BY order_lines
```

## 使用子查询

- 有嵌套

```sql
SELECT cust_name,
cust_state,
(SELECT COUNT(*)
FROM Orders
WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers
ORDER BY cust_name;
```

- 习题答案

```sql
SELECT cust_id
FROM Orders
WHERE order_num IN
(SELECT order_num
FROM OrderItems
WHERE item_price >= 10)
```

```sql
SELECT cust_email
From Customers
where cust_id in (SELECT cust_id FROM Orders where order_num in (SELECT order_num FROM OrderItems where prod_id = 'BR01'))
```

```sql
SELECT cust_id,
       (SELECT SUM(quantity * item_price)
        FROM OrderItems
        WHERE OrderItems.order_num = Orders.order_num) AS total_ordered
FROM Orders
ORDER BY total_ordered DESC;
```

```sql
SELECT prod_name,
       (SELECT SUM(quantity)
        FROM OrderItems
        WHERE OrderItems.prod_id = Products.prod_id) AS quant_sold
FROM Products;
```

## 联结 (join) 表

- 创建联结

```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```

- 内联结

```sql
SELECT vend_name, prod_name, prod_price
FROM Vendors
INNER JOIN Products ON Vendors.vend_id = Products.vend_id;
-- 传递给 ON 的实际条件与传递给 WHERE 的相同。
```

- 联结多个表

```sql
SELECT prod_name, vend_name, prod_price, quantity
FROM OrderItems, Products, Vendors
WHERE Products.vend_id = Vendors.vend_id
AND OrderItems.prod_id = Products.prod_id
AND order_num = 20007;
```

```sql
SELECT cust_name, cust_contact
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num
AND prod_id = 'RGAN01';
```

- 课后练习

```sql
select cust_name,order_num
from Customers
INNER JOIN Orders ON Orders.cust_id=Customers.cust_id
order by cust_name,order_num;
```

```sql
SELECT c.cust_name, os.order_num, SUM(os.quantity * os.item_price) AS OrderTotal
FROM Customers c
JOIN Orders o ON c.cust_id = o.cust_id
JOIN OrderItems os ON o.order_num = os.order_num
GROUP BY c.cust_name, os.order_num
ORDER BY c.cust_name, os.order_num;
```

```sql
select cust_id, order_date
from Orders,
(select order_num
from OrderItems
where prod_id="BR01") t -- t表示子查询，可以相当于内嵌一个查询
where t.order_num = Orders.order_num
order by order_date;
```

```sql
select Customers.cust_email
from Orders
inner join OrderItems using (order_num)
inner join Customers using (cust_id)
where OrderItems.prod_id = "BR01"
```

```sql
select cust_email  from  Customers
where cust_id in (select cust_id from Orders where order_num in 
    (select order_num from OrderItems where prod_id ='BR01'  )
)
```

```sql
select c.cust_name,sum(oi.item_price*oi.quantity) total_price
from OrderItems oi,Orders o,Customers c
where oi.order_num=o.order_num and o.cust_id=c.cust_id
group by c.cust_name
having total_price>=1000
order by total_price;
```

## 创建高级联结

- 使用表别名

```sql
SELECT cust_name, cust_contact
FROM Customers AS C, Orders AS O, OrderItems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RGAN01';
```

- 自然联结

```sql
SELECT C.*, O.order_num, O.order_date,
OI.prod_id, OI.quantity, OI.item_price
FROM Customers AS C, Orders AS O,
OrderItems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RGAN01';
```

- 外联结

```sql
SELECT Customers.cust_id, Orders.order_num
FROM Customers
LEFT OUTER JOIN Orders ON Customers.cust_id = Orders.cust_id;
```

- 课后习题

```sql
SELECT c.cust_name, o.order_num
FROM Customers AS c, Orders AS o
WHERE c.cust_id = o.cust_id
ORDER BY cust_name

select cust_name, order_num
from Customers 
join Orders using(cust_id)
order by 1;

SELECT
    C.cust_name,
    O.order_num
FROM
    Customers AS C
    INNER JOIN Orders AS O ON O.cust_id = C.cust_id
ORDER BY
    C.cust_name;
```

```sql
SELECT c.cust_name, o.order_num
FROM Customers AS c
LEFT  JOIN Orders AS o ON c.cust_id = o.cust_id
ORDER BY cust_name

select cust_name,order_num
from Customers
left join Orders using(cust_id)
order by cust_name;
```

```sql
SELECT p.prod_name, oi.order_num
FROM OrderItems AS oi
Right OUTER JOIN Products AS p ON oi.prod_id = p.prod_id
ORDER BY prod_name

select prod_name,order_num
from Products
left join OrderItems using(prod_id)
order by prod_name
```

```sql
SELECT p.prod_name, COUNT(o.order_num) AS orders
FROM Products AS p
LEFT OUTER JOIN OrderItems AS o ON p.prod_id = o. prod_id
group by prod_name
ORDER BY prod_name
```

```sql
select t.vend_id,count(prod_id) as order_num 
from Vendors t left join Products t1 on t.vend_id = t1.vend_id
group by t.vend_id
order by t.vend_id

select vend_id, count(prod_id) prod_id
from Vendors
left join Products using(vend_id)
group by vend_id
order by vend_id;

SELECT
  v.vend_id vend_id,
  COUNT(p.prod_id) prod_id
FROM
  Vendors v
  LEFT JOIN Products p ON v.vend_id = p.vend_id
GROUP BY
  v.vend_id
ORDER BY
  v.vend_id;
```

## 组合查询

- UNION

```sql
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```

- UNION ALL (不去重)

- 课后练习

```sql
SELECT prod_id, quantity
FROM OrderItems
WHERE quantity = 100
UNION 
SELECT prod_id, quantity
FROM OrderItems
WHERE prod_id like 'BNBG%'
ORDER BY prod_id
```

```sql
SELECT * FROM OrderItems
WHERE quantity = 100 OR prod_id LIKE 'BNBG%'
ORDER BY prod_id
```

```sql
SELECT prod_name
FROM Products
UNION ALL
SELECT *
FROM Customers
ORDER BY prod_name
```

```sql
SELECT cust_name, cust_contact, cust_email 
FROM Customers 
WHERE cust_state = 'MI' 
UNION 
SELECT cust_name, cust_contact, cust_email 
FROM Customers 
WHERE cust_state = 'IL'
ORDER BY cust_name; 
```

## 插入数据

- INSERT (确保有足够权限)

- 某处不需要插入值，则要插入 NULL

```sql
INSERT INTO Customers(cust_id,
cust_contact,
cust_email,
cust_name,
cust_address,
cust_city,
cust_state,
cust_zip)
VALUES(1000000006,
NULL,
NULL,
'Toy Land',
'123 Any Street',
'New York',
'NY',
'11111');
```

- INSERT + SELECT (可以插入多行)

- SELECT INTO (数据复制到新表)

```sql
SELECT *
INTO CustCopy
From Customers
```

- 课后习题

```sql
INSERT INTO exam_record (uid, exam_id, start_time, submit_time, score) VALUES
(1001, 9001, '2021-09-01 22:11:12', '2021-09-01 23:01:12', 90),
(1002, 9002, '2021-09-04 07:01:02', NULL, NULL);
```

```sql
INSERT INTO exam_record_before_2021(uid, exam_id, start_time, submit_time, score)
SELECT uid, exam_id, start_time, submit_time, score
FROM exam_record
WHERE YEAR(submit_time) < '2021';
```

```sql
REPLACE INTO examination_info
VALUES(NULL,9003,'SQL','hard',90,'2021-01-01 00:00:00');
```

[SQL插入语法](https://blog.csdn.net/chengyj0505/article/details/128227483?spm=1001.2014.3001.5502)

## 更新和删除数据

- UPDATE

```sql
UPDATE Customers
SET cust_email = 'kim@thetoystore.com'
WHERE cust_id = 1000000005; --客户 1000000005 现在有了电子邮件地址
```

- 更新多个列

```sql
UPDATE Customers
SET cust_contact = 'Sam Roberts', --使用一个set
cust_email = 'sam@toyland.com'
WHERE cust_id = 1000000006;
```

- 删除某列内容可以 UPDATE 为 NULL

```sql
UPDATE Customers
SET cust_email = NULL
WHERE cust_id = 1000000005;
```

- DELETE (需要一定的权限)

```sql
DELETE FROM Customers
WHERE cust_id = 1000000006;
```

- 课后习题

```sql
UPDATE examination_info
SET tag = 'Python'
WHERE tag = 'PYTHON';
```

```sql
UPDATE exam_record
SET submit_time = '2099-01-01 00:00:00', score = 0
WHERE start_time<'2021-09-01' and submit_time is null
```

```sql
DELETE FROM exam_record
WHERE timestampdiff(minute, start_time, submit_time ) < 5  AND score < 60
```

```sql
DELETE FROM exam_record
WHERE timestampdiff(minute, start_time, submit_time ) < 5  OR submit_time is NULL
ORDER BY start_time
LIMIT 3
```

```sql
TRUNCATE table exam_record --删除表内所有数据行外,还要实现对表结构中自增列的重置

drop table if EXISTS exam_record;
CREATE TABLE IF NOT EXISTS exam_record (
id int PRIMARY KEY AUTO_INCREMENT COMMENT '自增ID',
uid int NOT NULL COMMENT '用户ID',
exam_id int NOT NULL COMMENT '试卷ID',
start_time datetime NOT NULL COMMENT '开始时间',
submit_time datetime COMMENT '提交时间',
score tinyint COMMENT '得分'
)CHARACTER SET utf8 COLLATE utf8_general_ci;
```

[SQL删除数据](https://blog.csdn.net/chengyj0505/article/details/128358817?spm=1001.2014.3001.5502)

## 创建和操纵表

- CREATE TABLE

```sql
CREATE TABLE Products
(
prod_id CHAR(10) NOT NULL,
vend_id CHAR(10) NOT NULL,
prod_name CHAR(254) NOT NULL,
prod_price DECIMAL(8,2) NOT NULL,
prod_desc VARCHAR(1000) NULL --默认是 NULL
);
```

- 指定默认值

```sql
CREATE TABLE OrderItems
(
order_num INTEGER NOT NULL,
order_item INTEGER NOT NULL,
prod_id CHAR(10) NOT NULL,
quantity INTEGER NOT NULL DEFAULT 1,
item_price DECIMAL(8,2) NOT NULL
);
```

- 更新表 ALTER TABLE

```sql
ALTER TABLE Vendors
ADD vend_phone CHAR(20);
```

```sql
ALTER TABLE Vendors
DROP COLUMN vend_phone;
```

- 删除表 DROP TABLE

- 重命名表

- 课后习题

```sql
create table user_info_vip(
id int(11) not null primary key auto_increment comment '自增ID',
uid int(11) not null unique key comment '用户ID',
nick_name varchar(64) comment '昵称',
achievement int(11) default 0 comment '成就值',
level int(11) comment '用户等级',
job varchar(32) comment '职业方向',
register_time datetime default current_timestamp comment '注册时间'
) default charset=utf8
```

```sql
ALTER TABLE user_info ADD school varchar(15) AFTER `level`;
ALTER TABLE user_info CHANGE job profession varchar(10);
ALTER TABLE user_info CHANGE COLUMN achievement achievement int DEFAULT 0;
```

```sql
DROP table if exists exam_record_2011,exam_record_2012,exam_record_2013,exam_record_2014
```

## 使用视图

- 视图用于查看储存在别处的数据，而本身不包含数据。

- CREATE VIEW

```sql
CREATE VIEW ProductCustomers AS --我感觉类似于 Python 中自定义函数
SELECT cust_name, cust_contact, prod_id
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num;

SELECT cust_name, cust_contact
FROM ProductCustomers --使用视图
WHERE prod_id = 'RGAN01';
```

## 使用储存过程

- EXCUTE 执行储存过程

- 书里没怎么讲，还是得看实际运用。

## 管理事务处理

## 使用游标

## 高级 SQL 特性

# 大厂面试题

## 某音

```sql
SELECT a.video_id ,
   round(sum(if(end_time - start_time >= duration, 1, 0))/count(start_time ),3) as avg_comp_play_rate
FROM tb_user_video_log a
LEFT JOIN tb_video_info b
on a.video_id = b. video_id
WHERE year(start_time) = 2021
GROUP BY a.video_id 
ORDER BY avg_comp_play_rate DESC;
```
