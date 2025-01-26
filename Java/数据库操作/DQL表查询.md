MySQL 的 **DQL（Data Query Language）** 操作主要用于数据查询，它包括执行查询、检索、排序、分组和筛选数据等操作。以下是 MySQL 中常见的 DQL 操作及其详解：

```sql
SELECT column1, column2, ...  -- 查询指定的列
FROM table_name               -- 指定查询的表
[WHERE condition]             -- 可选：筛选记录的条件
[GROUP BY column]             -- 可选：按列进行分组
[HAVING condition]            -- 可选：筛选分组后的结果
[ORDER BY column [ASC|DESC]]  -- 可选：按列排序，默认升序（ASC）
[LIMIT number]                -- 可选：限制返回的记录数
```

### 1. **SELECT**

`SELECT` 语句用于从数据库中查询数据。它是 DQL 中最常用的操作。

#### 1.1 **基本查询**

返回表中的所有列和记录。

**示例**：

```sql
SELECT * FROM users;
-- 返回 users 表中的所有列和记录
```

#### 1.2 **选择特定列**

只返回指定的列。

**示例**：

```sql
SELECT name, age FROM users;
-- 返回 users 表中的 name 和 age 列
```

#### 1.3 **条件筛选（WHERE）**

使用 `WHERE` 子句过滤符合条件的记录。

**示例**：

```sql
SELECT * FROM users WHERE age > 18;
-- 返回 age 大于 18 的用户记录
```

#### 1.4 **排序（ORDER BY）**

使用 `ORDER BY` 子句对查询结果进行排序，默认按升序排列。

**示例**：

```sql
SELECT * FROM users ORDER BY age;
-- 按 age 升序排序
```

**示例**：按降序排序

```sql
SELECT * FROM users ORDER BY age DESC;
-- 按 age 降序排序
```

#### 1.5 **限制结果数量（LIMIT）**

使用 `LIMIT` 子句限制返回的记录数量。

**示例**：

```sql
SELECT * FROM users LIMIT 5;
-- 返回前 5 条记录
```

#### 1.6 **组合条件筛选（AND / OR）**

通过 `AND` 或 `OR` 组合多个筛选条件。

**示例**：

```sql
SELECT * FROM users WHERE age > 18 AND gender = 'Male';
-- 返回 age 大于 18 且 gender 为 'Male' 的用户记录
```

#### 1.7 **通配符查询（LIKE）**

使用 `LIKE` 查询符合特定模式的记录。

**示例**：

```sql
SELECT * FROM users WHERE name LIKE 'A%';
-- 返回 name 以 'A' 开头的用户记录
```

#### 1.8 **范围查询（BETWEEN）**

使用 `BETWEEN` 查询字段值在指定范围内的记录。

**示例**：

```sql
SELECT * FROM users WHERE age BETWEEN 18 AND 30;
-- 返回 age 在 18 到 30 之间的用户记录
```

#### 1.9 **NULL 值查询（IS NULL / IS NOT NULL）**

查询字段是否为 `NULL`。

**示例**：

```sql
SELECT * FROM users WHERE age IS NULL;
-- 返回 age 为 NULL 的用户记录
```

#### 1.10 查询时去重（使用 `DISTINCT`）

在查询时，`DISTINCT` 关键字可以用来去除重复的记录。它会确保查询结果中的每一行都是唯一的。

#### 语法格式：

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name
WHERE condition;
```

- `DISTINCT` 作用于一个或多个列，确保返回的结果集中的行是唯一的。
- `DISTINCT` 会对指定的列进行去重。

---

## 2. 条件查询

MySQL条件查询中的运算符非常多，主要分为比较运算符、逻辑运算符、范围查询运算符、模式匹配运算符、集合运算符等。以下是常用的运算符的详细说明：

### 1. **比较运算符**

- `=`：等于。用于比较两个值是否相等。
    
    ```sql
    SELECT * FROM users WHERE age = 30;
    ```
    
- `!=` 或 `<>`：不等于。用于比较两个值是否不相等。
    
    ```sql
    SELECT * FROM users WHERE age != 30;
    ```
    
- `>`：大于。用于判断一个值是否大于另一个值。
    
    ```sql
    SELECT * FROM products WHERE price > 100;
    ```
    
- `<`：小于。用于判断一个值是否小于另一个值。
    
    ```sql
    SELECT * FROM products WHERE price < 100;
    ```
    
- `>=`：大于等于。用于判断一个值是否大于或等于另一个值。
    
    ```sql
    SELECT * FROM products WHERE price >= 100;
    ```
    
- `<=`：小于等于。用于判断一个值是否小于或等于另一个值。
    
    ```sql
    SELECT * FROM products WHERE price <= 100;
    ```
    
- `BETWEEN ... AND ...`：范围查询。判断值是否在一个范围内（包含边界值）。
    
    ```sql
    SELECT * FROM products WHERE price BETWEEN 50 AND 100;
    ```
    

### 2. **逻辑运算符**

- `AND`：与运算。连接多个条件，所有条件必须为真。
    
    ```sql
    SELECT * FROM users WHERE age > 30 AND gender = 'M';
    ```
    
- `OR`：或运算。连接多个条件，只需一个条件为真。
    
    ```sql
    SELECT * FROM users WHERE age < 30 OR age > 60;
    ```
    
- `NOT`：非运算。取反操作，条件为假时返回真。
    
    ```sql
    SELECT * FROM users WHERE NOT age = 30;
    ```
    

### 3. **范围查询运算符**

- `IN`：判断值是否在指定的集合中。
    
    ```sql
    SELECT * FROM users WHERE age IN (20, 25, 30);
    ```
    
- `NOT IN`：判断值是否不在指定的集合中。
    
    ```sql
    SELECT * FROM users WHERE age NOT IN (20, 25, 30);
    ```
    

### 4. **模式匹配运算符**

- `LIKE`：用于模糊匹配字符串，可以使用通配符 `%`（匹配任意多个字符）和 `_`（匹配一个字符）。
    
    ```sql
    SELECT * FROM users WHERE name LIKE 'J%';  -- 姓名以J开头
    SELECT * FROM users WHERE name LIKE '%oe'; -- 姓名以oe结尾
    SELECT * FROM users WHERE name LIKE '_oe'; -- 姓名第二、三位是oe
    ```
    
- `NOT LIKE`：用于模糊匹配，不匹配的条件。
    
    ```sql
    SELECT * FROM users WHERE name NOT LIKE 'J%';
    ```
    

### 5. **空值判断运算符**

- `IS NULL`：判断一个值是否为`NULL`。
    
    ```sql
    SELECT * FROM users WHERE email IS NULL;
    ```
    
- `IS NOT NULL`：判断一个值是否不是`NULL`。
    
    ```sql
    SELECT * FROM users WHERE email IS NOT NULL;
    ```
    

### 6. **集合运算符**

- `ANY`：与子查询结合使用，表示只要满足子查询中的任何一个值。
    
    ```sql
    SELECT * FROM products WHERE price > ANY (SELECT price FROM products WHERE category = 'Electronics');
    ```
    
- `ALL`：与子查询结合使用，表示需要满足子查询中的所有值。
    
    ```sql
    SELECT * FROM products WHERE price > ALL (SELECT price FROM products WHERE category = 'Electronics');
    ```
    
- `EXISTS`：判断子查询是否返回结果。
    
    ```sql
    SELECT * FROM users WHERE EXISTS (SELECT * FROM orders WHERE users.id = orders.user_id);
    ```
    
- `NOT EXISTS`：判断子查询是否不返回结果。
    
    ```sql
    SELECT * FROM users WHERE NOT EXISTS (SELECT * FROM orders WHERE users.id = orders.user_id);
    ```
    

### 7. **位运算符**

- `&`：按位与。
    
    ```sql
    SELECT * FROM table WHERE (flag & 1) = 1;
    ```
    
- `|`：按位或。
    
    ```sql
    SELECT * FROM table WHERE (flag | 1) = 1;
    ```
    
- `^`：按位异或。
    
    ```sql
    SELECT * FROM table WHERE (flag ^ 1) = 1;
    ```
    
- `<<`：左移。
    
    ```sql
    SELECT * FROM table WHERE (flag << 2) = 4;
    ```
    
- `>>`：右移。
    
    ```sql
    SELECT * FROM table WHERE (flag >> 1) = 1;
    ```
    

### 8. **其它常用运算符**

- `DISTINCT`：去除重复值。
    
    ```sql
    SELECT DISTINCT column_name FROM table;
    ```
    
- `LIMIT`：限制查询结果的数量。
    
    ```sql
    SELECT * FROM users LIMIT 10;
    ```
    
- `ORDER BY`：排序运算符，可以按照指定字段升序或降序排列。
    
    ```sql
    SELECT * FROM users ORDER BY age DESC;
    ```
    
- `GROUP BY`：将结果分组，通常与聚合函数（如`COUNT`、`SUM`、`AVG`等）一起使用。
    
    ```sql
    SELECT age, COUNT(*) FROM users GROUP BY age;
    ```
    

这些运算符可以结合在一起使用，根据查询需求来选择最合适的运算符。

### 3. **聚合函数（Aggregate Functions）**

聚合函数用于对查询结果进行汇总，通常与 `GROUP BY` 一起使用。

#### 2.1 **COUNT()**

计算查询结果的行数。

**示例**：

```sql
SELECT COUNT(*) FROM users;
-- 返回 users 表的总记录数
```

#### 2.2 **SUM()**

计算某列的总和。

**示例**：

```sql
SELECT SUM(salary) FROM employees;
-- 返回 employees 表中 salary 列的总和
```

#### 2.3 **AVG()**

计算某列的平均值。

**示例**：

```sql
SELECT AVG(age) FROM users;
-- 返回 users 表中 age 列的平均值
```

#### 2.4 **MAX()**

返回某列的最大值。

**示例**：

```sql
SELECT MAX(age) FROM users;
-- 返回 users 表中 age 列的最大值
```

#### 2.5 **MIN()**

返回某列的最小值。

**示例**：

```sql
SELECT MIN(age) FROM users;
-- 返回 users 表中 age 列的最小值
```

#### 2.6 **GROUP_CONCAT()**

将多行数据合并为一个字符串。

**示例**：

```sql
SELECT GROUP_CONCAT(name) FROM users;
-- 将 users 表中的 name 列合并为一个逗号分隔的字符串
```

---

### 4. **分组查询（GROUP BY）**

`GROUP BY` 子句将查询结果按指定列分组，常与聚合函数配合使用。

#### 3.1 **基本分组查询**

将查询结果按指定字段分组。

**示例**：

```sql
SELECT gender, COUNT(*) FROM users GROUP BY gender;
-- 返回每个 gender 的用户数量
```

#### 3.2 **分组排序**

对分组后的结果进行排序。

**示例**：

```sql
SELECT gender, COUNT(*) FROM users GROUP BY gender ORDER BY COUNT(*) DESC;
-- 返回每个 gender 的用户数量，并按数量降序排序
```

#### 3.3 **过滤分组（HAVING）**

`HAVING` 子句用于对分组后的结果进行过滤，类似于 `WHERE`。

**示例**：

```sql
SELECT gender, COUNT(*) FROM users GROUP BY gender HAVING COUNT(*) > 10;
-- 返回用户数量大于 10 的 gender 分组
```

---

### 5. **连接查询（JOIN）**

`JOIN` 用于将两个或多个表的数据按某个条件组合。

#### 4.1 **内连接（INNER JOIN）**

返回两个表中匹配的记录，不匹配的记录不会出现在结果中。

**示例**：

```sql
SELECT users.name, orders.order_id
FROM users
INNER JOIN orders ON users.user_id = orders.user_id;
-- 返回有匹配记录的用户和订单
```

#### 4.2 **左连接（LEFT JOIN）**

返回左表中的所有记录，以及右表中匹配的记录。如果右表没有匹配的记录，结果中右表部分为 `NULL`。

**示例**：

```sql
SELECT users.name, orders.order_id
FROM users
LEFT JOIN orders ON users.user_id = orders.user_id;
-- 返回所有用户以及他们的订单（没有订单的用户 order_id 为 NULL）
```

#### 4.3 **右连接（RIGHT JOIN）**

返回右表中的所有记录，以及左表中匹配的记录。如果左表没有匹配的记录，结果中左表部分为 `NULL`。

**示例**：

```sql
SELECT users.name, orders.order_id
FROM users
RIGHT JOIN orders ON users.user_id = orders.user_id;
-- 返回所有订单以及对应的用户（没有用户的订单 name 为 NULL）
```

#### 4.4 **全外连接（FULL OUTER JOIN）**

返回左右两表中所有的记录，包括没有匹配的部分。MySQL 不直接支持 `FULL OUTER JOIN`，但可以通过 `UNION` 实现。

**示例**：

```sql
SELECT users.name, orders.order_id
FROM users
LEFT JOIN orders ON users.user_id = orders.user_id
UNION
SELECT users.name, orders.order_id
FROM users
RIGHT JOIN orders ON users.user_id = orders.user_id;
-- 返回左表和右表中所有的记录，包括没有匹配的部分
```

---

### 6. **子查询（Subqueries）**

子查询是嵌套在其他查询中的查询。

#### 5.1 **在 `SELECT` 中使用子查询**

可以在 `SELECT` 子句中使用子查询来返回计算值。

**示例**：

```sql
SELECT name, (SELECT AVG(age) FROM users) AS avg_age FROM users;
-- 返回所有用户的名字以及所有用户的平均年龄
```

#### 5.2 **在 `WHERE` 中使用子查询**

使用子查询筛选满足条件的记录。

**示例**：

```sql
SELECT * FROM users WHERE age > (SELECT AVG(age) FROM users);
-- 返回年龄大于所有用户平均年龄的用户
```

#### 5.3 **在 `FROM` 中使用子查询**

将子查询作为表来查询。

**示例**：

```sql
SELECT * FROM (SELECT name, age FROM users) AS subquery WHERE age > 18;
-- 返回所有年龄大于 18 岁的用户
```

---

### 6. **DISTINCT**

`DISTINCT` 用于返回不同的（唯一的）记录。

**示例**：

```sql
SELECT DISTINCT gender FROM users;
-- 返回 users 表中所有唯一的 gender 值
```

---

### 8. **联合查询（UNION）**

`UNION` 用于合并两个或多个查询的结果集，且默认去重。

#### 7.1 **基本联合查询**

将多个 `SELECT` 结果合并为一个结果集。

**示例**：

```sql
SELECT name FROM users
UNION
SELECT name FROM employees;
-- 返回 users 和 employees 表中的所有唯一名字
```

#### 7.2 **去重与保留重复**

使用 `UNION ALL` 保留重复记录。

**示例**：

```sql
SELECT name FROM users
UNION ALL
SELECT name FROM employees;
-- 返回 users 和 employees 表中的所有名字（包括重复的）
```

---

### 总结：

MySQL 的 DQL 操作主要用于查询和检索数据，包括基本的 `SELECT` 查询、筛选、排序、分组、聚合、连接、子查询、联合查询等。掌握这些操作可以帮助你高效地从数据库中提取所需的信息。
