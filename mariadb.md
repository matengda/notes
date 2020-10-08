修改root用户密码

```bash
mysqladmin -uroot -p$pass $newPassword
```



严格模式

```mariadb
SET GLOBAL sql_mode = 'STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
-- 在标准 SQL 中，字符串使用 单引号
```



连接到数据库

```bash
mysql [-h $host] [-P $port] -u $user -P$passwd
# -P$passwd 没有空格
# -h --host=name 默认值 localhost
# -P --port 默认值 3690
```



退出mariadb shell

```mariadb
\q
```

```mariadb
quit
```



结果垂直输出

```mariadb
\G
/*
\G 垂直显示结果
\G 可以替代 ;
\G 是 Mariadb monitor command
*/
```

定界符delimiter

```php
$PDO->query("SELECT * FROM `$databaseName` . `$tableName`"); // 此分号是php的
// 在编程语言的sql语句中省略 定界符(;)
```



默认定界符

```mariadb
;
```

```mariadb
/*
\g 发送命令到Mariadb服务器 等同于 ;
\g 命令结束 可以不写 ; 写 \g
\g 是 Mariadb monitor 命令
\g 请勿在查询语句中使用 Mariadb monitor
\g $PDO->query();
*/
```



设置定界符

```mariadb
delimiter $delimiterName -- 无 ;
```

```mariadb
\d $delimiterName -- 无 ;
```



显示所有数据库

```mariadb
SHOW DATABASES;
```



显示所有表

```mariadb
SHOW TABLES FROM `$databaseName`;
-- 不是单引号, 是 重音符; 重音符非必须
```



创建数据库

```mariadb
CREATE DATABASE [IF NOT EXISTS] `$databaseName` [DEFAULT] CHARACTER SET [=] utf8mb4;
```



创建表

```mariadb
CREATE TABLE [IF NOT EXISTS] `$databaseName` . `$tableName` ( -- 重音符为可选项
    /*
    如果使用 USE 选择数据库, 则不必重复数据库名
    CREATE TABLE [IF NOT EXISTS] `$tableName`
    */
    `$columnName` $TYPE [UNSIGNED | NOT NULL | DEFAULT $defaultValue | AUTO_INCREMENT],
    `$columnName` $TYPE [DEFAULT $functionName ON UPDATE $functionName | DEFAULT $funcationName ON DELETE $funcationName],
    /*
    $functionName 不是函数也可以
    DEFAULT 如果被省略, 默认插入 NULL
    DEFAULT ON UPDATE NOW() -- 会在更新数据的时候自动更新时间
    */
    PRIMARY KEY (`$columnName`)
) [ENGINE=$engineName | DEFAULT CHARACTER SET [=] utf8mb4];
```



选择数据库

```mariadb
USE `$databaseName`;
```

```mariadb
\u [`]$databaseName[`] -- 不能有 ; 
/*
\u 是 MariaDb monitor command
MariaDB monitor command 不加 delimiter
*/
```



显示创建数据库语法

```mariadb
SHOW CREATE DATABASE `$databaseName`;
```



显示创建表语法

```mariadb
SHOW CREATE TABLE `$databaseName` . `$tableName`;
```



删除数据库

```mariadb
DROP DATABASE `$databaseName`;
```



删除表

```mariadb
DROP TABLE `$databaseName` . `$tableName`;
```



修改数据库字符

```mariadb
ALTER DATABASE `$databaseName` [DEFAULT] CHARACTER SET [=] utf8mb4;
```

```mariadb
ALTER DATABASE `$databaseName` [DEFAULT] CHARSET [=] utf8mb4;
```



显示创建表结构

```mariadb
{EXPLAIN | DESCRIBE | DESC} `$databaseName` . `$tableName`;
```



设置字符集

```mariadb
SET NAMES utf8mb4;
-- 设置客户端将用于将SQL语句发送到服务器的字符集
```



添加列

```mariadb
ALTER TABLE `$databaseName` . `$tableName`
    ADD [COLUMN] [IF NOT EXISTS] `$columnName` $TYPE,
    ADD [COLUMN] [IF NOT EXISTS] `$columnName` $TYPE FIRST, -- 把列放到最前面
    ADD [COLUMN] [IF NOT EXISTS] `$columnName` $TYPE AFTER `$fieldName`; -- 把列放到$fieldName后面
```



删除列

```mariadb
ALTER TABLE `$databaseName` . `$tableName`
    DROP [COLUMN] [IF EXISTS] `$columnName`,
    DROP [COLUMN] [IF EXISTS] `$columnName`;
```



改变列名和数据类型

```mariadb
ALTER TABLE `$databaseName` . `$tableName`
    CHANGE [COLUMN] `$oldColumnName` `$newColumnName` $TYPE,
    CHANGE [COLUMN] `$oldColumnName` `$newColumnName` $TYPE; -- CHANGE [COLUMN] 必须同 字段类型 一起修改
```



改变列类型

```mariadb
ALTER TABLE `$databaseName` . `$tableName`
    MODIFY [COLUMN] [IF EXISTS] `$columnName` $TYPE,
    MODIFY [COLUMN] [IF EXISTS] `$columnName` $TYPE;
```



改变表

```mariadb
ALTER TABLE `$databaseName` . `$tableName` ENGINE [=] $engineName; -- DEFAULT 不是可选项
```



改变表字符集

```mariadb
ALTER TABLE `$databaseName` . `$tableName` [DEFAULT] CHARACTER SET [=] utf8mb4;
```

```mariadb
ALTER TABLE `$databaseName` . `$tableName` CONVERT TO CHARACTER SET utf8mb4; -- = 不是可选项
```



改变表名字

```mariadb
ALTER TABLE `$databaseName` . `$oldTableName` RENAME TO `$databaseName` . `$newTableName`;
```

```mariadb
RENAME TABLE `$databaseName` . `$oldTableName` TO `$databaseName` . `$newTableName`;
```



显示所有存储引擎

```mariadb
SHOW ENGINES;
```



改变表校对规则

```mariadb
ALTER TABLE `$databaseName` . '$tableName` [DEFAULT] COLLATE [=] $collateName;
-- 校对规则就是一种字符集的字符的比较顺序。每一种字符集，都有一个默认的比较规则。因此，校对规则一般不用指定
```



显示所有字符集和校对规则

```mariadb
SHOW CHARACTER SET;
```



拷贝表结构和数据

```mariadb
CREATE TABLE $tableName SELECT `$columnName` FROM `$databaseName` . `$tableName`;
-- 以上方法复制表结构和数据, 但不会复制主键和自动增长
```



手动添加主键和自动增长

```mariadb
ALTER TABLE `$databaseName` . `$tableName` MODIFY [COLUMN] `$columnName` $TYPE AUTO_INCREMENT PRIMARY KEY;
```



删除表主键

```mariadb
ALTER TABLE `$databaseName` . `$tableName` DROP PRIMARY KEY;
```



拷贝表结构

```mariadb
CREATE TABLE `$databaseName` . `$tableName` LIKE `$databaseName` . `$tableName`
-- 复制表的结构和主键包括自动增长, 不会复制数据
```

```mariadb
INSERT INTO `$databaseName` . `$tableName` SELECT * FROM `$databaseName` . `$tableName`
-- 手动从其它表中插入数据, 列数要匹配
```



显示系统字符集设置

```mariadb
SHOW VARIABLES LIKE 'character%'; -- % 匹配任意多个字符
```





什么是关系型数据库

`关系型数据库`又称`为关系型数据库管理系统`(RDBMS),它是利用数据概念实现对数据处理的算法，达到对数据及其快速的增删改查操作。

既然被称为`关系型`数据库，那么它的关系在哪里体现呢？

举一个例子吧。

比如我现在有`表单A` 和 `表单B`

其中:

`表单A` 中有一个名为user_id的字段

`表单B` 中也有一个名为user_id的字段

现在我把他们建立一种联系，当我去修改`表单A`的user_id的值时，`表单B` 中的user_id的值也会自动进行修改，因为他们建立的一种`关系`，因为这种关系，`使得数据具有一致性。` 千万数据中，获取有数条直接，在运维或者开发哥哥的神操作下，他们冥冥中被安排的明明白白。

 

`非关系型数据库` 正如它的名字，每条数据间都是独立存在的，没撒子关系哩。