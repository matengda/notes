**include require() include_once require_once 的区别**

- include() 这个函数一般是放在流程控制的处理部分中. PHP程序网页在读到 include() 的文件时, 才将它读进来. 这种方式, 可以把程序执行时的流程简单化, include() 包括并运行指定文件, 在处理失败时, include() 产生一个警告, 被导入的程序代码都会被执行, include() 有返回值


- require() 这个函数通常放在php程序的最前面，PHP程序在执行前，就会先读入 require() 所指定引入的文件，使它变成PHP程序网页的一部份，require() 一个文件存在错误的话，那么程序就会中断执行了，并显示致命错误，require() 没有返回值


- include_once() 的作用和 include_once() 是几乎相同的，唯一的差别在于 include_once() 会先检查要导入的档案是不是已经在该程序中的其它地方被导入过了，如果有的话就不会再次重复导入(这项功能有时候是很重要的，比方说要导入的里面宣告了一些你自行定义好的函数，那么如果在同一个程序重复导入这个文件，在第二次导入的时候便会发生错误讯息，因为PHP不允许相同名称的函数被重复宣告第二次)
---

**用什么办法检查PHP脚本的执行效率(通常是脚本执行时间)**
```php
<?php

// 一般实在要检查的代码开头记录一个时间，结尾记录一个时间，取差值

$timeStart = microtime(TRUE); // 如果给出了 get_as_float 参数并且其值等价于 TRUE, microtime() 将返回一个浮点数

usleep(1000); // 以指定的微秒数延迟执行, seconds = 1000000

$timeEnd = microtime(TRUE);

$runTime = round($timeEnd - $timeStart, 4); // 对浮点数进行四舍五入

echo "$runTime";

```
---

**冒泡排序**
```php 
<?php

$array = array(12,35,68,23,90,35,46,756);
$len = count($array);

for ($i=0; $i < $len; $i++) {
    for ($j = $i + 1; $j < $len; $j++) {
        if ($array[$i] > $array[$j]) {
            $tmp = $array[$i];
            $array[$i] = $array[$j];
            $array[$j] = $tmp;
        }
    }
}

print_r($array);

```
---

**echo 和 print 的区别**
- echo 接受参数列表
- print 仅支持一个参数 
---

**什么是php**
- php是一种开源脚本语言
- php是一种镶嵌在 HTML 中, 运行在服务器端的脚本语言
---

**把字符串转换为大写**
```php
<?php

strtouper($string); // str to upper

```
---

**把字符串转换为小写**
```php
<?php

strtolower($string); // str to lower

```
---

**把字符串首字母转换为大写**
```php
<?php

ucfirst($string); // u c first

```
---

**把字符串的一个字符小写**
```php
<?php

lcfirst($string); // l c first

```
---

**把字符串中的每个单词的首字母转为大写**
```php
<?php

ucwords($string); // u c word s

```
---

**对字符串进行大小写转换**
```php
<?php

mb_convert_case($string);

```
---

**Session Cookie的区别**
- Cookie 数据保存在客户的浏览器上，Session 数据保存在服务器上
- Cookie 不是很安全，别人可能进行 cookie 欺骗，考虑到安全应当使用 Session
- Session 会在一定时间内保存在服务器上，当访问增多，会比较占用服务器性能，考虑到减轻服务器性能方面应当使用 Cookie
- 重要的资料用 Session 不重要的用 Cookie
- 单个 Cookie 体积限制是4k Session 可以通过设置修改
---

**如果禁用了cookie 可不可以用Session**
#+BEGIN_VERSE
可以，通常情况下 Cookie 中记录了 Session 的 ID 所以 Cookie 被禁用了也就意味着 Session 失效了. 不过 Session-ID 还有另外一种传递方式, 就是在URL查询中携带 Session-ID

Session 不一定必须依赖 Cookie Cookie 被禁用或者出现问题时, PHP会自动把 Session-ID 附在URL中, 修改php.ini(session.use_trans_sid = 1)

---

**Session的默认生命周期是多久**

关闭浏览器就失效, 因为 Session-ID 存在于 Cookie 而默认情况下 Cookie 关闭浏览器即失效

---

**如何设置Session生命周期为30分钟**

session.cookie_lifetime = 1800 ; 修改 php.ini

---

**GET POST 提交方法的区别**

- GET POST 理论上没有大小限制, 但是 GET 的长度和 URL 的长度有直接关系 HTTP 协议没有对 URL 长度进行限制 IE 浏览器最 URL 最大限制为 2083 字节 其他浏览器更多, 所以 GET 最好限制在 2083 字节
- GET 放在 URL 因此不安全, 而 POST 传输数据相对安全
- GET 是从服务器上获取数据, POST 是向服务器传送数据
---

**检测一个变量是否有设置的函数**
```php
<?php

isset($variable);
/**
检测变量是否设置, 并且不是 NULL
如果一个变量设置 NULL 返回 FALSE
**/

```
---

**检测一个变量是否为空的函数**
```php
<?php

empty($variable);
/**
判断变量是否为空
如果变量是 非空 非零值, 返回 FALSE
“”, 0, “0”, NULL, FALSE, array(),  变量只声明没定义,  返回 FALSE
**/

```
---

**在PHP中插入一段HTML的办法**
```php 
<?php

echo '<h1>code</h1>';

print <<<EOF
<h1>code</h1>
EOF;
/**
开始标识符 结束标识符 保持一致
标识符行 不可以注释 和 任何 空白字符
结束标识 必须 顶格 独自占一行(即必须从行首开始, 前后不能衔接任何空白和字符)
**/

```
---

**在PHP中, heredoc是一种特殊的字符串, 它的结束标志必须**

heredoc 的语法是用 <<< 加上自己定义成对的标签, 在标签范围内的文字视为一个字符串

---

**类的属性可以序列化后保存到Session中, 从而以后可以回复整个类, 这要用的函数是**
```php
<?php

serialize($mixed); // 产生一个可存储的值的表示
unserialize($string); // 对单一的已序列化的变量进行操作，将其转换回 PHP 的值

```
---

**用php打印出前一天的时间，格式是2006-5-10 22:22:21**
```php
<?php

echo date('Y-m-d H:i:s', strtotime('-1 day')); // strtotime() 将任何字符串的日期时间描述解析为 Unix 时间戳

```
---

**public protected private三种访问控制模式的区别**
- public 公有 任何地方都可以访问
- protected 受保护类型 用于本类和继承类调用
- private 私有类型 只能在本类中使用
---

**写出php权限控制修饰符**
- public
- protected
- private
---

**接口和抽象类的区别是什么**
- 抽象类 是一种不能被实例化的类, 只能作为其他类的父类来使用. 抽象类是通过关键字 abstract 来声明. 抽象类 与普通类相似, 都包含 成员属性 和成员方法, 两者的区别在于, 抽象类中至少要包含一种 抽象方法. 抽象方法 没有方法体(大括号里面的是方法体), 该方法天生就是 要被子类重写的 抽象方法的格式为 abstract function abstractMethod();


- 接口 是通过 interface 关键字来声明 接口中的成员常量和方法都是 public 接口中的方法也是没有方法体. 接口中的方法也天生就是要被子类实现


- 抽象类 和 接口 实现的功能十分相似, 最大的不同 接口能实现多继承


- 子类继承 抽象类 使用 extends 子类实现接口使用 implements
---

**PHP定义和使用常量**
```php 
<?php

define('PI', 3.14);
/**
常量的值必须是一个定值, 不能是变量, 类属性, 数学运算的结果或函数调用
常量名通常要大写, 在PHP7中还允许是个 array 的值
**/

echo PI;

```
---

**类中如何定义常量**

```php
<?php

class example
{
    const PI = 3.14; // 类中的常量也就是成员常量, 常量就是不会改变的量, 是一个恒定的值
}
```

---

**如何类中调用常量, 如何类外调用常量**

无论类内还是类外, 常量的访问和变量不一样, 常量不需要实例化对象, 访问常量的格式都是类型加作用域操作符(双冒号)来调用, className::constantName

---

**__autoload() 函数是如何运作的**

使用这个魔术函数的基本条件是 类文件的文件名 要和 类的名字 保持一致

当程序执行到实例化某个类的时候, 如果在实例化前没有引入这个类文件, 那么自动执行 __autoload()

这个函数会根据实例化的类的名称来查找这个类文件的路径, 当判断这个类文件路径下确实存在这个类文件后就来载入这个类, 然后程序继续执行, 如果这个路径下不存在该文件时就提示错误

---

**哪种OOP设计模式能让类在整个脚本里中实例化一次**

单件模式

---

**借助继承，我们可以创建其他类的派生类, 在PHP中, 子类最多可以继承几个父类**

one

---

**写出你知道的几种设计模式**

- 单例模式
- 工厂模式
---

**执行以下代码，输出的结果是**
```php 
<?php

abstract class example
{
    function __construct()
    {
        echo 'example';
    }
}

    $className = new example(); // 致命错误, 因为类 example 是抽象类, 不能被实例化

```
---

**执行以下代码，输出结果是?**
```php 
<?php

class example
{
    function __construct()
    {
        echo 'classExample';
    }
}

class child extends example
{
    function __construct()
    {
        echo 'classChild';
    }
}

$className = new child();
/**
classChild
类 child 继承类 example, 两个类都定义了构造函数, 由于二者名称相同, 所以子类中的构造函数覆盖了父类的构造函数
要想子类对象实例化时也执行父类的构造函数，需要在子类构造函数中使用 parent::__construct() 来显示调用父类构造函数
**/

```
---

**请定义一个名为 example 的类, 这个类只有一个静态方法 staticMethod **
```php 
<?php

class example
{
    public static function staticMethod()
    {
        //methodBody
    }
}

```
---

**只有该类才能访问该类的私有变量吗**

yes

---

**写出下列程序的输出结果**
```php 
<?php

class example
{
    protected $variable;
    /**
    受保护的, 本类 和 继承类 调用
    在类中定义的变量称之为属性
    常见的属性声明是由关键字 public protected private 开头, 后面跟一个普通的变量声明
    **/

    public function method()
    {
        $this->$variable = 10; // 当前 对象 指针
    }
}
    
class child extends example
{
    public function printVariable()
    {
        return $this->Variable = 20; // 返回 当前对象 指针
    }
}

$className = new child();
echo $className->printVariable(); // 输出结果 20 printVariable() 没有调用 example->method()

```
---

**写出构造函数和析构函数**
- 构造函数 __construct
- 析构函数 __destruct
---

**下面这段代码是什么意思**
```php 
<?php

class example
{
    function encryption($password)
    {
    return md5(md5($password));
    }
}

$classExample = new example();
$result = $classExample->encryption('password');
echo $result;
// 双重 MD5 加密

```
---

**如何声明一个名为 example 的没有方法和属性的类**
```php 
<?php

class example
{
    // 声明 example 类
}

```
---

**如何实例化一个名为 example 的对象**
```php 
<?php

class example
{
    // 声明 example 类
}

$classExample = new example(); // 实例化类

```
---

**在php中 error_reporting() 这个函数有什么作用**
```php
<?php

error_reporting(); // 用于设置PHP的侦听错误的级别

```
---

**php如何限制上传文件的大小**

upload_max_filesize ; 默认是2M
post_max_size ; 默认是8M

---

**数据库中的事务是什么**

事务 是作为一个逻辑单元执行的一系列操作, 一个逻辑工作单元必须有四个属性, 称为 ACID (原子性 一致性 隔离性 持久性)

- 原子性 事务必须是原子工作单元, 对其数据修改, 要么全部执行, 要不全部不执行


- 一致性 事务在完成时, 必须使所有的数据都保持一致状态. 在数据库中, 所有规则都必须应用于事务的修改, 以保持所有数据的完整性 事务结束时, 所有的内部数据(如B树索引或双向链表)都必须是正确的


- 隔离性 由并发事务所作的修改必须与任何其他并发事务所做的修改隔离, 事务查看数据时数据所处的状态, 要么是另一并发事务修改它之前的状态, 要么是另一事务修改它之后的状态, 事务不会查看中间状态的数据, 这称为 可串行性 因为它能够重新装载起始数据, 并且重播一系列事务, 以使数据结束时状态与原始事务执行的状态相同


- 持久性 事务完成之后, 它对于系统的影响是永久性的, 该修改即使出现系统故障也将一直保持
---

**MySQL取得当前时间的函数**
```php sql
SELECT NOW(); -- NOW() 该函数返回当前系统的日期和时间
```
---

**MySQL格式化日期的函数**
```php sql
DATE_FORMAT(NOW(), '%Y %m %%d'); -- DATE_FORMAT($date, $format)
```
---

**如何查看SQL语句执行时间**
```php sql
EXPLAIN SELECT NOW(); -- EXPLAIN
```
---

**请给出SQL语句优化的几种方案**
- 尽量选择较小的列
- 将 WHERE 中用的比较频繁的字段建立索引
- SELECT 子句中避免使用 **
- 避免在索引例上使用计算 NOT IN < > 等操作
- 当只需要一行数据的时候使用 LIMIT 1
- 保证单表数据不超过200w，适时分割表
---

**MySQL NOT IN 用法**
```php sql
DELETE FROM $table_name WHERE $column_id NOT IN (1, 2, 3); -- 删除表中 $column_id 不为 1, 2, 3 的记录
```
---

**MyISAM和InnoDB的区别**
- MyISAM 表在磁盘中有三个文件组成, 分别是 表定义文件(.frm) 数据文件(.MYD) 索引文件(.MYI) 而 InnoDB 的表由 表定义文件(.frm) 表空间数据和日志文件组成
- MyISAM 强调的是性能, 其查询效率较高, 但不支持事务和外键等安全性能方面的功能, 而 InnoDB 支持 事务 外键 等 高级功能, 查询效率稍低
- MyISAM 支持表锁, 而 InnoDB 支持行锁
---

**数据库索引分几类，分别是什么**
- PRIMARY 主索引
- UNIQUE 唯一索引
- INDEX 普通索引
- FULLTEXT 全文索引
---

**MySQL中 主索引 唯一索引 普通索引 全文索引 的区别**
- PRIMARY 主键 **唯一** 且 **不为空**
- UNIQUE 唯一索引 不允许有重复
- INDEX 索引 普通索引
- FULLTEXT 全文索引 用于在一篇文章中, 检索文本信息
---

**什么时候使用索引**

并非所有的数据库都以相同的方式使用索引, 作为通用规则, 只有当经常查询列中的数据时, 才需要在表上创建索引

---

**索引的目的是什么**
- 快速访问数据表中特定的信息, 提高检索速度
- 创建唯一索引, 保证数据库表中每一行数据的唯一性
- 加速表与表之间的连接
- 使用分组和排序子句进行数据检索时, 可以显著减少查询分组和排序的时间
---

**为数据表建立索引的原则有哪些**
- 在最频繁使用的, 用以缩小查询范围的字段上建立索引
- 在频繁使用的, 需要排序的字段上建立索引
---

**索引对数据库负面的影响是什么**
- 创建索引维护索引需要消耗时间, 这个时间随着数据量的增加而增加
- 索引需要占用物理空间, 不光是表需要占用物理空间, 每个索引也需要占用物理空间
- 当对表进行 **增** **删** **改** **查** 的时候索引也需要动态的维护, 这样就降低了数据的维护速度
---

**什么情况下不宜建立索引**
- 对于查询中很少涉及的列或者重复值比较多的列, 不宜建立索引
- 对于一些特殊的据哭类型, 不宜建立索引, 比如文本字段等
---

**内连接和外链接的区别**
- 内连接取两个表的交集, 外连接分左和右, 左连接取左边的全部, 右连接取右边的全部
- 内连接, 进行连接的两个表对应的想匹配的字段完全相同的连接
- 左连接(LEFT OUTER JOIN) 两个表左连接时会返回左边表中的所有行和右边表中与之相匹配的列值, 如果没有则用空值代替
- 右连接(RIGHT OUTER JOIN) 两个表进行右连接时会返回右边表中的所有的行和左边表中与之想匹配的列值, 没有匹配的用空值代替
---

**用PHP打印出前一天的时间, 格式是 2016-5-10 22:21:21 如何实现?**
```php 
<?php

echo date('Y-m-d H:i:s', strtotime('-1 days'));

```
---

**如何实现字符串 Hello World 翻转**
```php 
<?php

$string = strrev('Hello World'); // dlroW olleH

```
---

**请写出实现汉语字符串截取无乱码的方法**
```php 
<?php

echo mb_substr('这样一来, 汉语没有乱码', 0, 7, 'utf-8'); // 空格算一个字符

```
---

**什么是 OAuth2.0 授权?**

OAuth(开放授权) 是一种开放标准, 允许用户授权第三方网站访问他们存储在另外的服务器提供者上的信息, 而不需要将用户名和密码提供给第三方网站或分享他们数据的所有内容

---

**如何打开PHP的错误报告?**

display_errors = On ; 修改php.ini文件

---

**JavaScript表单淡出对话框函数是什么? 获得输入焦点函数是什么?**

```php js
alert(); // 弹出对话框
focus(); // 获得输入焦点
```
---

**下面哪个函数可以打开一个文件, 以对文件进行读和写操作?**

- [ ] fget()
- [ ] file_open()
- [x]  fopen()
- [ ] open_file()
---

**下面哪个选项没有将 admin 添加到 users 数组中?**
- [ ] $users[] = 'admin';
- [x] array_add($users, 'admin'); // PHP 默认没有 array_add() 函数
- [x] array_push($users, 'admin'); // 先将 $users 声明为数组
- [x] $users ||= 'admin'; // 无此写法
---

**写出以下程序的输出结果**
```php 
<?php

$a = 201;
$b = 40;
$c = $a > $b ? 4 : 5; // PHP 三元运算符
/****
 ** experssionOne ? experssionTwo : experssionThree
 ** experssionOne 为 TRUE experssionOne 为 experssionTwo
 ** experssionOne 为 FALSE experssionOne 为 experssionThree
 **/

echo $c; // 4

```
---

**请写出PHP的构造函数和析构函数**
```php
<?php

__construct(); // 构造函数
__destruct(); // 析构函数

```
---

**你知道的加密算法有几种?**
- DES
- AES
- MD5
- RSA
---

**用PHP写出显示客户端IP与服务器IP的代码**
```php 
<?php

echo "客户端ip地址:" . '&nbsp' . '"' . $_SERVER['REMOTE_ADDR'] . '"';
echo '</br>'
echo "服务器ip地址:" . '&nbsp' . '"' . $_SERVER['SERVER_ADDR'] . '"';

```
---

**UTF-8 编码文件的 BOM 头 占几个 byte?**

BOM(Byte Order Mark) 占用 **3** byte

---

**如何修改Session的生存时间**

session.cookie_lifetime = $expiretime ; 修改php.ini

```php
<?php

session_set_cookie_params();
/**
仅在当前脚本执行过程中有效
因此如果要通过此函数修改 Cookie 参数, 因此需要对每个请求都要
调用 session_start() 之前调用 session_set_cookie_params()
**/
```
```php 
<?php

$lifetime = 86400;
session_set_cookie_params($lifetime);
/****
 ** 仅在当前脚本执行过程中有效
 ** 因此, 如果要通过函数修改 cookie 参数
 ** 需要对每个请求都在调用 seesion_start() 函数之前
 ** 调用 session_set_cookie_params()
 **/

session_start();
$_SESSION['example'] = TRUE;

```
---

**谈谈你对MVC理解**

由模型(Model), 视图(View), 控制器(Controller), 完成的应用程序

由模型发出要实现的功能到控制器, 控制器接受组织功能传递给视图
---

**写出发帖数最多的10个人名字的SQL, 利用下表: members(id,username,posts,pass,email)**
```php sql
SELECT username FROM members ORDER BY posts DESC LIMIT 10; -- DESC 按降序排列
```
---

**请说明PHP中传值与传引用的区别**

传值: 函数范围内对值的任何改变在函数外部都会被忽略

传引用: 函数范围内对值的任何改变在函数外部也能反映出这些改变
---

**写一个函数, 给一个日期, 输出该日期的前一天, 输出日期格式为 2014-05-19 14:30:22**
```php 
<?php

function toYesterday($time) {
    $timestamp = strtotime($time);
    $yesterdayTimestamp = $timestamp - 86400;
    return date('Y-m-d H:i:s', $yesterdayTimestamp);
}

```
---

**写一个函数, 从一个标准URL中获取文件的扩展名**
```php 
<?php

function getExtensionName($url) {
    return $extensionName = pathinfo($url, PATHINFO_EXTENSION);
}

```
---

**AJAX是什么? 同步和异步的区别**

AJAX全称为"Asynchronous JavaScript And XML"(异步JavaScript和XML), 是一种创建交互式网页应用的网页开发技术

- 同步: 脚本会停留并等待服务器发出回复然后再继续
- 异步: 脚本允许页面继续其进程并处理可能的回复
---

**HTTP协议中几个状态码的含义: 502 500 403 401 301 200**
- 502: 网关错误
- 500: 服务器内部错误
- 403: 禁止
- 401: 未授权
- 301: 永久移动
- 200: ok
---

**PHP中有集中形式装载代码, 并列举, 以包含同目录下的config.php为例**
```php 
<?php

require('./config.php');


include('./config.php');


require_once('./config.php');


include_once('./config.php');

```
---

**写几个常用的PHP数组函数并简述函数用法**

```php
<?php

count($array); // 计算数组中的单元数目,或对象中的属性个数

current($array); // 返回数组中的当前单元

next($array); // 将数组中的内部指针向前移动一位

prev($array); // 将数组的内部指针倒回一位

implode($array); // 将一个 一维 数组的值转化为字符串

```
---

**写出几个常用的PHP字符串函数并且简述函数用法**
```php
<?php

strlen($string); // 获取字符串长度

substr($string); // 返回字符串的子串

substr_count($string); // 计算字符串出现的次数

strstr($string); // 查找字符串的首次出现

strrev($string); // 反转字符串

```
---

**PHP多行注释**
```php 
<?php

/**
PHP
多行注释
**/

```
---

**PHP块注释**
```php 
<?php

/****
 ** 块注释通常用于提供对 文件 方法 数据机构 算法的描述
 ** 块注释被置于每个文件的开始处以及每个方法之前, 它们也可以用于其他地方
 ** 比如说方法内部, 在功能和方法内裤的块注释应该和它们所描述的代码具有一样的缩进
 ** 块注释首行应该有一个空行, 用于把块注释和代码分开
 **/

```
---

**UTF-8 编码下, 下面程序输出什么, 为什么?**
```php 
echo strlen('Welcome to 学校'); // 17, 因为在 UTF-8 编码下, 一个 英文字 符占用一个字节, 空格 占用一个字节, 一个汉字占用三个字节
```
---

**简述以下Memcache和Redis的区别**
- Redis, Memcache 都是将数据存放在内存中, 都是内存数据库, 不过 Memcache 还可以用于缓存其它东西, 例如图片, 视频等
- Redis 不仅仅支持简单的 K(key)/V(value) 类型的数据
- Redis 当物理内存用时, 可以将一些很久没用到的 Value 交换到磁盘
- Memacache 挂掉后, 数据不可恢复, Redis 数据丢失后可以通过 AOF(Append Only File) 恢复
- Redis 支持数据持久化, 可以将内存中的数据保持在磁盘中, 重启的时候可以再次加载进行使用
---

**谈谈序列化和反序列化的作用**
- 序列化: 指将php中 对象 类 数组 变量 匿名函数等, 转化为字符串
- 反序列化: 将字符串转化为 对象 类 数组 变量 匿名函数
- 序列化在每个编程语言中都存在
- 广义的说, 将一个 Word 保存为 docx, 这就是序列化的过程, 打开 docx 文档, 显示内容, 就是反序列化的过程
- 序列化之后, 会返回一个可存储的字符串, 有利于存储或传递PHP的值, 同时不丢失其数据类型和结构
---

**列举出几个php魔术常量**
- __FILE__ 文件的完整路径和文件名 如果用在被包含文件中, 则返回被包含的文件名
- __LINE__ 文件中的当前行号
- __DIR__ 文件所在的目录 如果用在被包括文件中, 则返回被包括的文件所在的目录, 它等价于 dirname(__FILE__) 除非是根目录, 否则目录中名不包括末尾的斜杠
- __FUNCTION__ 函数名称 返回该函数被定义时的名字（区分大小写）
- __CLASS__ 类的名称 返回该类被定义时的名字（区分大小写）
---

**php是用什么语言编写的**

c

---

**请列出php的8种数据类型**

4种标量类型: boolean(布尔型) integer(整形) float(浮点型,也称作 double) string(字符串)

2种符合类型: array(数组) object(对象)

2种特殊类型: resource(资源) NULL(NULL)
---

**$string = ' MY name IS PHP ' 编程实现将$string中的字符串前后的空格以及中间的空格去掉, 并全部转换为小写字母, 最后输出$string和$string中的字母的个数**
```php 
<?php

$string = ' MY name IS PHP ';
$string = str_replace(' ', '', $string); // 子字符串替换
$string = strtolower($string);
echo $string;
echo strlen($string);

```
---

**$string = 'ilikephp' 编程实现: 将字符串分割为单个字符存放到一个数组中, 并打印数组**
```php 
<?php

$string = 'ilikephp';
$stringLength = strlen($string);

for ($i = 0; $i < $stringLength; $i++) {
    $array[] = substr($string, $i, 1);
    /**
    string substr ( string $string , int $start [, int $length ] )
    返回字符串 string 由 start 和 length 参数指定的子字符串
    **/
}

print_r($array);

```
---

**PHP中单引号和双引号的区别, 哪个速度更快?**

在PHP中, 单引号内的数据不会被解析(任何变量和特殊转义符), 所以更快, 而双引号内的数据会被解析, 如变量($variable)值会被代入字符串中, 特殊转义符也会被解析成特定的单个字

---

**防止SQL注入一般用什么函数?**

addslashes()

---

**sort() asort() ksort() 有什么区别**
```php
<?php

sort($array); // 对数组进行排序, 函数结束时数组单元将被从最低到最高从新安排
asort($array); // 对数组进行排序, 数组的索引保持和单元的关联
ksort($array); // 对数组按照键名排序, 保留键名到数据的关联

```
---

**写出Session的运行机制**

浏览器第一次打开网站页面, 服务器自动生成 Session-ID, 响应给浏览器, 浏览器把这个 Session-ID 保存到 Cookie, 再次打开这个网站页面时将 Cookie 保存的 Session-ID 发送给服务器, Session 变量中储存的值存在服务器中, 浏览器只有 Session-ID 的值, 当浏览器关闭页面或者 Session-ID 超时, Session自动注销

---

**抓取远程图片到本地, 你会用什么函数**

file_get_contents()

---

**var_dump() 和 print_r() 的区别**

- var_dump() 返回表达式的类型与值而
- print_r() 仅返回结果
---

**在PHP中 error_reporting() 这个函数的作用是什么**

设置应该报告何种PHP错误

---

**什么是抽象方法**

我们在类里面定义的 **没有方法体** 的方法就是抽象方法, 所谓的没有方法体指的是, 在方法声明的时候 **没有大括号以及其中的内容**  而是直接在声明时在方法名后加上分号结束, 另外在声明抽象方法时还要加一个关键字 **abstrace** 来修饰

```php
<?php

abstract class abstractClass // 声明 抽象类
{
    abstract function abstractMethod(); // 声明 抽象方法 没有花括号
}

```
---

**受保护的类和私有类什么区别**
- 受保护的继承后可以访问
- 私有的只能在该类中访问, 不会被继承访问
---

**PHP中 this self parent 的区别**
- this 指向当前对象的指针,可以堪称c里面的指针, this 也可以指向 **继承的父类**
- self 指向当前类的指针
- parent 指向父类的指针