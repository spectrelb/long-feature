# 备忘基础sql查询


##### 洗数据常用到的一些语法
```mysql
INSERT INTO `order`(
`id`,
`type`,
`status`,
`create_time`,
`update_time`
) SELECT
`id`,
case type
when 'a' then '李四'
else '张三'
end,
IF(status = 1, 0, 1) as status,
date_add(create_time, interval 7 day),
`update_time`
FROM `order_copy`;
```

##### 简单查询
```mysql
-- 查询 users 表包含所有列的所有行
SELECT * FROM `users`;

-- 查询 users 表包含 id 和 name 两列的所有行
SELECT `id`, `name` FROM `users`;

-- 查询 users 表 name 和 email 都不重复的行
SELECT DISTINCT `name`, `email` FROM `users`;
``` 

##### 限制查询
```mysql
-- 查询 users 表中的前 5 条数据
SELECT * FROM `users` LIMIT 5;

-- 查询 users 表中从第五行起的 5 条数据（主要用于分页）
SELECT * FROM `users` LIMIT 5 OFFSET 5;
```

##### 排序查询

```mysql
-- 查询 users 表中以 id 列升序排列的所有行
SELECT * FROM `users` ORDER BY `id`;

-- 查询 users 表中以 id 列降序排列的所有行
SELECT * FROM `users` ORDER BY `id` DESC;

-- 多列排序查询
-- 查询 users 表中以 id 列降序排列后以 name 列为升序排列的所有行
SELECT * FROM `users` ORDER BY `id` DESC, `name`;
```

##### 格式查询
```mysql
-- 查询 users 表中的 name 列并将其用括号括起而且设置列别名为 newName
SELECT Concat('(', `name`, ')') AS `newName` FROM `users`;

-- 查询 users 表中的 name 列并清除其左右的空格而且设置列别名为 newName
SELECT TRIM(`name`) AS `newName` FROM `users`;

-- 查询 users 表中的 name 列并将其改为大写而且设置列别名为 newName
SELECT UPPER(`name`) AS `newName` FROM `users`;

-- 查询 users 表中的 name 列的内容长度而且设置列别名为 nameLen
SELECT LENGTH(`name`) AS `newName` FROM `users`;

-- 查询 products 表中 price 与 number 的乘积并设为别名 total
SELECT `price` * `number` AS `total` FROM `products`;
```

##### 条件查询
```mysql
-- 查询 users 表中 id 为 1 的行
SELECT * FROM `users` WHERE `id` = 1;

-- 查询 users 表中 id 为 1 到 10 之间的行
SELECT * FROM `users` WHERE `id` BETWEEN 1 AND 10;

-- 查询 users 表中 name 为 NULL 值的行
SELECT * FROM `users` WHERE `name` IS NULL;

-- 查询 users 表中 name 为 zane、panda、pi 之一的所有行
SELECT * FROM `users` WHERE `name` IN ('zane', 'panda', 'pi');

-- 查询 users 表中 name 不为 zane、panda、pi 的所有行
SELECT * FROM `users` WHERE NOT `name` IN ('zane', 'panda', 'pi');

-- 查询 users 表中 name 包含 zane 字符串的行
SELECT * FROM `users` WHERE `name` LIKE '%zane%';

-- 查询 users 表中 email 以 5 开头，@qq.com 结尾的行
SELECT * FROM `users` WHERE `email` LIKE '5%@qq.com';

-- 多条件查询
-- 查询 users 表中 height 在 170 到 180 之间或 weight 在 60 到 75 之间的所有行
SELECT * FROM `users` WHERE (`height` BETWEEN 170 AND 180) OR (`weight` BETWEEN 60 AND 75);
```

##### 汇总查询
```mysql
-- 查询 products 表中 price 的平均值并设为别名 avgPrice
SELECT AVG(`price`) AS `avgPrice` FROM `products`;

-- 查询 users 表中的总行数并设为别名 userNum
SELECT COUNT(*) AS `userNum` FROM `users`;

-- 查询 users 表中 email 列非空值的总行数并设为别名 emailNum
SELECT COUNT(email) AS `emailNum` FROM `users`;

-- 查询 users 表中 id 列的最大值并设为别名 maxId
SELECT MAX(id) AS `maxId` FROM `users`;

-- 查询 users 表中 id 列的最小值并设为别名 minId
SELECT MIN(id) AS `minId` FROM `users`;

-- 查询 users 表中 name 列非空不重复值的总行数并设为别名 uniqueUserNameNum
-- 注意在 COUNT(*) 中使用 DISTINCT 无效
SELECT COUNT(DISTINCT `name`) AS `uniqueUserNameNum` FROM `users`;
```

##### 分组查询
```mysql
-- 查询 products 表中以 provider 为分组的行数
SELECT `provider`, COUNT(*) AS `providerProductNum` FROM `products` GROUP BY `provider`;

-- 查询 products 表中以 provider 为分组并过滤掉 COUNT(*) <= 2 的分组
SELECT `provider`, COUNT(*) AS `num` FROM `products` GROUP BY `provider` HAVING COUNT(*) > 2
```


##### 子查询
```mysql
-- 查询 products 表中 purchaser 列为 users 表中 id 为 1的行
SELECT * FROM `products` WHERE `purchaser` IN (SELECT id FROM `users`);
```


##### 联结查询
```mysql
-- 内联结查询 orders users 表中的 name start 字段（简单格式）
SELECT users.name, orders.start FROM `users`, `orders` WHERE users.id = orders.user_id;

-- 内联结查询 orders users 表中的 name start 字段（标准格式）
SELECT users.name, orders.start FROM `users` INNER JOIN `orders` ON users.id = orders.user_id;

-- 内联结查询多个表
SELECT users.name, orders.start, activities.title FROM `users`, `orders`, `activities` WHERE users.id = orders.user_id AND users.id = activities.leader_id;

-- 自联结查询 products 表中 name 为 computer 的 provider 的所有行
SELECT * FROM `products` WHERE `provider` = (SELECT `provider` FROM `products` WHERE `name` = "computer");

-- 外联结查询 customers 表对应 orders 表的行
SELECT customers.id orders.num FROM customers LEFT OUTER JOIN orders ON customers.id = orders.custom_id；
```


##### 联合查询
```mysql
-- 联合查询 products 表和 users 表的 name 列，UNION 默认会清除内容重复的行
SELECT `name` FROM `products` UNION SELECT `name` FROM `users`;

-- 联合查询 products 表和 users 表的 name 列，不清除内容重复的行
SELECT `name` FROM `products` UNION ALL SELECT `name` FROM `users`;
```


##### 插入
```mysql
-- 插入完整一行至 users 表
INSERT INTO `users` VALUES ('000001', 'email', 'username', 'password');

-- 指定列完整插入一行至 users 表
INSERT INTO `users`(`id`, `email`, `name`, `password`) VALUES ('000001', 'qq@qq.com', 'qq', '123');

-- 指点部分列插入一行至 users 表
INSERT INTO `users`(`email`, `name`, `password`) VALUES ('qq@qq.com', 'qq', '123');

-- 将 oldUsers 表中的数据插入到 users 表
INSERT INTO `users`(`email`, `name`, `password`) SELECT `email`, `name`, `password` FROM `oldUsers`;

-- 将 users 表的数据复制到 usersCopy 表
SELECT * INTO `usersCopy` FROM `users`;

-- 创建新表 usersCopy 并将 users 复制过去
CREATE TABLE `usersCopy` AS SELECT * FROM `users`;
```


##### 修改
```mysql
-- 更新 users 表中 id 为 1 的行的 name 字段为 zane
UPDATE `users` SET `name` = 'pi' WHERE id = 1;

-- 更新多个字段
UPDATE `users` SET `name` = 'zane', `email` = 'pi@0php.net' WHERE id = 2;

-- 更新 users 表中所有行 price + 1 
UPDATE `products` SET `price` = `price` + 1;

-- 更新 users 表中 provider 为 APPLE 的行的 price + 1 
UPDATE `products` SET `price` = `price` + 1 WHERE `provider` = 'APPLE';


-- 更新 users 表中 id 为 order 的id
update `users` as a, `order` as b set a.id = b.id 
where a.id = b.id;
```


##### 删除
```mysql
-- 删除 users 表中的所有数据
DELETE FROM `users`;

-- 删除 users 表中 id 为 5 的行
DELETE FROM `users` WHERE `id` = 5;

-- 快速删除 users 表中所有行
TRUNCATE TABLE `users`
```

##### 创建表
```mysql
CREATE TABLE `orders` (
	`id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(32) NOT NULL COMMENT '用户姓名',
  `id_card` varchar(18) NOT NULL COMMENT '身份证号',
  `mobile` varchar(16) DEFAULT NULL COMMENT '手机号',
  `create_time` datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6) COMMENT '创建时间',
  `update_time` datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6) COMMENT '更新时间',
  PRIMARY KEY(id)
);
```

##### 更新表
```mysql
-- 添加列
ALTER TABLE `orders` ADD COLUMN `name` varchar(32) NOT NULL COMMENT '姓名' AFTER `id`;


-- 修改列
ALTER TABLE `orders` CHANGE `name` `newProduct` VARCHAR(65) NOT NULL;

-- 修改字段类型，长度
ALTER TABLE `orders` MODIFY COLUMN `name` varchar(64) DEFAULT NULL COMMENT '用户编号';

-- 删除字段
ALTER TABLE `orders` DROP COLUMN `user_id`;
```

##### 删除表
```mysql
DROP TABLE `orders`;
```

##### 重命名表
```mysql
RENAME TABLE `users` TO `newUsers`;
```

##### 添加索引
```mysql
ALTER TABLE `order` ADD INDEX `idx_name`(`name`) USING BTREE;
```

##### 删除索引
```mysql
ALTER TABLE `order` DROP `idx_name`;
```
