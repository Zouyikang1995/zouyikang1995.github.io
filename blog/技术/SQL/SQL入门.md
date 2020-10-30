# SQL入门
## SQL基本语法
### Oracle基本数据类型
| 类型 | 含义 |
| :-- | :-- |
| CHAR(length) | 存储固定长度的字符串。参数length指定了长度，如果存储的字符串长度小于length，用空格填充。默认长度是1，最长不超过2000字节。 |
| VARCHAR2(length) | 存储可变长度的字符串。length指定了该字符串的最大长度。默认长度是1，最长不超过4000字符。|
| NUMBER(p，s) | 既可以存储浮点数，也可以存储整数，p表示数字的最大位数（如果是小数包括整数部分和小数部分和小数点，p默认是38为），s是指小数位数。可存负数 |
| DATE | 存储日期和时间，存储纪元、4位年、月、日、时、分、秒，存储时间从公元前4712年1月1日到公元后4712年12月31日。 |
| TIMESTAMP | 不但存储日期的年月日，时分秒，以及秒后6位，同时包含时区。 |
| CLOB | 存储大的文本，比如存储非结构化的XML文档 |
| BLOB | 存储二进制对象，如图形、视频、声音等。|

### 重要的SQL命令
| 命令 | 含义 |
| :-- | :-- |
| SELECT | 从数据库中提取数据 |
| UPDATE | 更新数据库中的数据 |
| DELETE | 从数据库中删除数据 |
| INSERT INTO | 向数据库中插入新数据 |
| CREATE DATABASE | 创建新数据库 |
| ALTER DATABASE | 修改数据库 |
| CREATE TABLE | 创建新表 |
| ALTER TABLE | 变更（改变）数据库表 |
| DROP TABLE | 删除表 |
| CREATE INDEX | 创建索引（搜索键） |
| DROP INDEX | 删除索引 |

### SQL书写规范
1. 合理地使用空格和缩进来增强可读性。SELECT和FROM等关键字，都右对齐，而实际的列名和实现细节都左对齐。
2. SQL关键字大写，表名、列名、查询内容等小写。
3. 使用自然集合术语的复数形式来给表命名，例如employees；使用单数形式给列命名，例如employee。多个单词时避免使用驼峰命名法，用下划线连接，例如tb_employees。
4. 在等号`=`前后，逗号`,`后，成对的单引号`'`前后加入空格。
```c#
(SELECT f.species_name,
        AVG(f.height) AS average_height, AVG(f.diameter) AS average_diameter
   FROM flora AS f
  WHERE f.species_name = 'Banksia'
     OR f.species_name = 'Sheoak'
     OR f.species_name = 'Wattle'
  GROUP BY f.species_name, f.observation_date)

  UNION ALL

(SELECT b.species_name,
        AVG(b.height) AS average_height, AVG(b.diameter) AS average_diameter
   FROM botanic_garden_flora AS b
  WHERE b.species_name = 'Banksia'
     OR b.species_name = 'Sheoak'
     OR b.species_name = 'Wattle'
  GROUP BY b.species_name, b.observation_date);
```
5. Joins语句以及Subqueries子查询语句注意缩进对齐，并且在必要时添加换行。
```c#
SELECT r.last_name
  FROM riders AS r
       INNER JOIN bikes AS b
       ON r.bike_vin_num = b.vin_num
          AND b.engine_tally > 2

       INNER JOIN crew AS c
       ON r.crew_chief_last_name = c.last_name
          AND c.chief = 'Y';
```
*****
```c#
SELECT r.last_name,
       (SELECT MAX(YEAR(championship_date))
          FROM champions AS c
         WHERE c.last_name = r.last_name
           AND c.confirmed = 'Y') AS last_championship_year
  FROM riders AS r
 WHERE r.last_name IN
       (SELECT c.last_name
          FROM champions AS c
         WHERE YEAR(championship_date) > '2008'
           AND c.confirmed = 'Y');
```
6. 创建表格时注意缩进对齐，列名进行一次缩进，列名约束进行二次缩进。
```c#
CREATE TABLE staff (
    PRIMARY KEY (staff_num),
    staff_num      INT(5)       NOT NULL,
    first_name     VARCHAR(100) NOT NULL,
    pens_in_drawer INT(2)       NOT NULL,
                   CONSTRAINT pens_in_drawer_range
                   CHECK(pens_in_drawer BETWEEN 1 AND 99)
);
```
## 创建表 Create Table
### SQL CREATE TABLE语法
```c#
CREATE TABLE table_name
(
    column_name1 data_type(size),
    column_name2 data_type(size),
    column_name3 data_type(size),
    ....
);
```
*语法参数的含义*

| 参数 | 含义 |
| :-- | :-- |
| table_name | 参数中规定表的名字 |
| column_name | 参数规定表中列的名称 |
| data_type | 参数规定列的数据类型 |
| size | 参数规定表中列的最大长度 |

### 创建表的实例

## 表的增删改查 Insert/Delete/Update/Select
### 增加 Insert 

### 删除 Delete

### 修改 Update 

### 查找 Select

## 参考资料
[SQL CREATE TABLE语句](https://www.runoob.com/sql/sql-create-table.html)
[Oracle创建空间和表](https://www.cnblogs.com/qmfsun/p/3817344.html)
[SQL Style Guide](https://www.sqlstyle.guide/)
