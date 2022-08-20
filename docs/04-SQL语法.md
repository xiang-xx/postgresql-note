# 04-SQL语法

## 语法结构

标识符
- a-z 开头
- 其他字符可以是下划线，数字，字母，或者 $，sql 标准不支持 $，尽量不要用

关键字
- 大小写混合都能识别 SeLecT...

PostgreSQL 有三种隐式类型常量
- strings
- bit strings
- numbers

```sql
select 'foo\n';
```
0 字节码的字符不能设置到 string 常量。
```sql
select $$foo$$; -- $ 引号，此常量字符内不会 escape，例如 \n 不会生效
select $SomeTag$Dianne's horse$SomeTag$; -- add tag
```
$ 引号不是 sql 标准，别乱用。

Bit-String 常量
- 使用 B 开头的二进制 B'1001'
- 使用 X 开头的 16 进制 X'1FF'

数字常量 eg
- 42
- 3.5
- 4.
- .001
- 5e2
- 1.9e-3

类型转换
```sql
select int '10';
select '10'::int8;
select cast('10' as int);
```

运算符优先级
- . 取 table/colume 标识操作
- :: 类型转换
- [] 数组下标
- + - 一元运算
- ^ 乘方
- * / %
- + -
- 其他运算
- BETWEEN IN LIKE ILIKE SIMILAR
- < > = <= >= <>
- IS ISNULL NOTNULL
- NOT
- AND
- OR


## 值表达式

- correlation.columename 列引用
- $number 位置参数，函数里 $1 是第一个位置的参数
- expression[subscript] 下标，eg table.arraycolumn[4]  $1[10:42]
- 字段选择 expression.fieldname table.*
- 操作符表达式
- 函数调用，eg. sqrt(2)
- 聚合表达式 eg. count(*) arrat_agg(a order by b desc)
- 窗口函数调用
- 类型转换
- Collation 表达式，用于字符串处理 select * from cities order by name collate "C";
- 标量子查询 select name,(select max...) from table;
- 数组构造 SELECT ARRAY[1,2,3+4];
- 行构造 SELECT ROW(1, 2.3, this is a test');

## 函数调用

```sql
CREATE FUNCTION concat_lower_or_upper(a text, b text, uppercase boolean DEFAULT false)
RETURNS TEXT
AS
$$
  SELECT CASE
    WHEN $3 THEN UPPER($1 || ' ' || $2)
    ELSE LOWER($1 || ' ' || $2)
    END;
$$
LANGUAGE SQL IMMUTABLE STRICT;

SELECT concat_lower_or_upper('hello', 'world', true);
SELECT concat_lower_or_upper('hello', 'world', false);
SELECT concat_lower_or_upper('hello', 'world');
SELECT concat_lower_or_upper('hello', 'world', uppercase => true); -- 命名参数只能放在位置参数的后面
```




