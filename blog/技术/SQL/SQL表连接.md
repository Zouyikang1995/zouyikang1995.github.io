# SQL表连接
## 种类
* INNER
* LEFT OUTER
* RIGHT OUTER
* FULL OUTER
* CROSS

## 图文解释
### (INNER) JOIN (INNER可省略)
```java
SELECT <select_list>
FROM table A
INNER JOIN table B
ON A.key = B.key
```
![inner_join](assets/inner_join.jpg)

### LEFT (OUTER) JOIN (OUTER可省略)
```java
SELECT <select_list>
FROM table A
LEFT JOIN table B
ON A.key = B.key
```
![left_fulljoin](assets/left_fulljoin.jpg)
```java
SELECT <select_list>
FROM table A
LEFT JOIN table B
ON A.key = B.key
WHERE B.key IS NULL
```
![left_partjoin](assets/left_partjoin.jpg)

### RIGHT (OUTER) JOIN (OUTER可省略)
```java
SELECT <select_list>
FROM table A
RIGHT JOIN table B
ON A.key = B.key
```
![right_fulljoin](assets/right_fulljoin.jpg)
```java
SELECT <select_list>
FROM table A
RIGHT JOIN table B
ON A.key = B.key
WHERE A.key IS NULL
```
![right_partjoin](assets/right_partjoin.jpg)

### FULL OUTER JOIN
```java
SELECT <select_list>
FROM table A 
FULL OUTER JOIN table B
ON A.key = B.key
```
![full_join](assets/full_join.jpg)
```java
SELECT <select_list>
FROM table A
FULL OUTER JOIN table B
ON A.key = B.key
WHERE A.key IS NULL
OR B.key IS NULL
```
![full_innerjoin](assets/full_innerjoin.jpg)

### CROSS JOIN (`不支持ON语法`)
返回两张表的`笛卡尔积`，也就是不指定结合规则，让两表中的元素直接两两结合。
```java
SELECT <select_list>
FROM table A
CROSS JOIN table B
```