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
```js
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
```js
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
```js
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
```js
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
```js
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


## Oracle对象(Object)
1. `VIEW`
CREATE语法：
```
CREATE [OR REPLACE] [FORCE|NOFORCE] VIEW　<ビュー名>
  AS <SELECT文> [WITH READ ONLY] [WITH CHECK OPTION];
```

| 参数 |	説明 |
| :-- | :-- |
| OR REPLACE |	同名のビューが既に存在した時でも、構わず上書きする場合に指定する |
| FORCE | FORCE：エラーがあっても強制的にVIEWを作成する |   
| NOFORCE | NOFORCE：エラーがあった場合はVIEWを作成しない |
| WITH READ ONLY | 読み取り専用のVIEWを作成する |
| WITH CHECK OPTION | VIEWにより問い合わされた行に対してのみ、ビューを通して挿入や更新を可能とする場合に指定する |

2. `INDEX`   
CREATE INDEX语法说明：
```
CREATE [UNIQUE | BITMAP] INDEX <インデックス名>
  ON <テーブル名>(列名 [ASC|DESC],...)
  [TABLESPACE 表領域名];
```

| 参数 |	説明 |
| :-- | :-- |
| UNIQUE | 通常のINDEXを作成する |
| BITMAP | BITMAP INDEXを作成する |
| TABLESPACE | INDEXを作成する表領域を指定する |

3. `SEQUENCE`
CREATE SEQUENCE语法说明：

```
CREATE [OR REPLACE] SEQUENCE 順序名
  [ START WITH 初期値 ]
  [ INCREMENT BY 増分値 ]
  [ MAXVALUE 最大値 | NOMAXVALUE ]
  [ MINVALUE 最小値 | NOMINVALUE ]
  [ CYCLE | NOCYCLE ]
  [ CACHE キャッシュ数 | NOCYCLE ]
;
```

| パラメータ | 説明 |
| :-- | :-- |
| OR REPLACE | 同名のシーケンスが既に存在した時でも、構わず上書きする場合に指定する |
| START WITH | 初期値の設定  ここで設定した値から采番が開始する |
| INCREMENT BY |	増分の設定  ここで指定した数だけ増えていく |
| MAXVALUE 最大値 / NOMAXVALUE | インクリメントする最大値の設定  NOMAXVALUEを設定すると最大値はナシ。 |
| MINVALUE 最小値 / NOMINVALUE |	増分がマイナスの時の最小値の設定   NOMINVALUEを設定すると最小値はナシ。 |
| CYCLE / NOCYCLE | 最大値に達したときにシーケンスをループするかしないかの設定 |
| CACHE キャッシュ数 / NOCACHE |	シーケンスに高速にアクセスするために、メモリー上に値を保持しておく場合に指定する。（デフォルト値は、キャッシュ数＝20） |

4. `TRIGGER`   
CREATE TRIGGER语法说明：

```
CREATE [ OR REPLACE ] TRIGGER トリガー名
  { BEFORE | AFTER | INSTEAD OF }
  { INSERT | UPDATE [OF 列名,...] | DELETE }
    [OR {INSERT | UPDATE [OF 列名,...] | DELETE }]
    [... ]
      ON テーブル名
    [ FOR EACH ROW ]
    [ WHEN 条件式 ]
      BEGIN
        処理内容
    END
;
```

| 参数 |	说明 |
| OR REPLACE |	同じ名前のトリガーがあった場合は上書きする指定。 |
| BEFORE / AFTER / INSTEAD OF | トリガーを起動させるタイミング。 |
| BEFORE | before：データが操作される前にトリガーを起動する |
| AFTER | after：データが操作された後にトリガーを起動する |
| INSTEAD OF | instead of：データが操作されるSQLが実行された時、そのSQLは実行せずにトリガーだけを起動する |
| FOR EACH ROW | 複数の行のデータが操作された場合、この指定があると、各行ごとにトリガーを起動する   この指定が無いと複数行のデータが操作されるSQLが発行されても１回しかトリガーは起動されない。 |


5. `SYNONYM`
CREATE SYNONYM语法说明：

```
CREATE [OR REPLACE] [PUBLIC] SYNONYM 別名
  FOR スキーマ名.オブジェクト名;
```

| 参数 | 说明 |
| OR REPLACE | 同名のシノニムが既に存在した時でも、構わず上書きする場合に指定する |
| PUBLIC | パブリックシノニムを作成する場合に指定する。  （パブリックシノニムとは全てのユーザがアクセス可能なシノニムの事です。） |

## 参考资料
[SQL CREATE TABLE语句](https://www.runoob.com/sql/sql-create-table.html)
[Oracle创建空间和表](https://www.cnblogs.com/qmfsun/p/3817344.html)
[SQL Style Guide](https://www.sqlstyle.guide/)
