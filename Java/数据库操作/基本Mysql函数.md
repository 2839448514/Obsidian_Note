MySQL 提供了大量内置函数，用于数据操作、转换、处理等。以下是 MySQL 中最常用的基本函数分类及详细说明：

### 1. **字符串函数（String Functions）**

用于字符串操作，如连接、查找、替换、截取等。

#### 1.1 `CONCAT(str1, str2, ...)`

连接多个字符串。

**示例**：

```sql
SELECT CONCAT('Hello', ' ', 'World!');
-- 结果: 'Hello World!'
```

#### 1.2 `SUBSTRING(str, start, length)`

截取字符串的一部分。

**示例**：

```sql
SELECT SUBSTRING('Hello World', 1, 5);
-- 结果: 'Hello'
```

#### 1.3 `LENGTH(str)`

返回字符串的字节数。

**示例**：

```sql
SELECT LENGTH('Hello');
-- 结果: 5
```

#### 1.4 `CHAR_LENGTH(str)`

返回字符串的字符数（忽略字符的字节数，适用于多字节字符）。

**示例**：

```sql
SELECT CHAR_LENGTH('你好');
-- 结果: 2
```

#### 1.5 `UPPER(str)`

将字符串转换为大写字母。

**示例**：

```sql
SELECT UPPER('hello');
-- 结果: 'HELLO'
```

#### 1.6 `LOWER(str)`

将字符串转换为小写字母。

**示例**：

```sql
SELECT LOWER('HELLO');
-- 结果: 'hello'
```

#### 1.7 `REPLACE(str, old_str, new_str)`

替换字符串中的某个子串。

**示例**：

```sql
SELECT REPLACE('Hello World', 'World', 'MySQL');
-- 结果: 'Hello MySQL'
```

#### 1.8 `TRIM([remstr FROM] str)`

移除字符串两端的空格或指定字符。

**示例**：

```sql
SELECT TRIM(' ' FROM '  Hello World  ');
-- 结果: 'Hello World'
```

#### 1.9 `CONCAT_WS(separator, str1, str2, ...)`

使用指定分隔符连接多个字符串。

**示例**：

```sql
SELECT CONCAT_WS('-', '2025', '01', '14');
-- 结果: '2025-01-14'
```

#### 1.10 `INSTR(str, substring)`

返回子串在字符串中首次出现的位置。

**示例**：

```sql
SELECT INSTR('Hello World', 'World');
-- 结果: 7
```

---

### 2. **数字函数（Numeric Functions）**

用于数字的数学运算和处理。

#### 2.1 `ABS(number)`

返回数字的绝对值。

**示例**：

```sql
SELECT ABS(-5);
-- 结果: 5
```

#### 2.2 `ROUND(number, decimal_places)`

对数字进行四舍五入，保留指定的小数位数。

**示例**：

```sql
SELECT ROUND(3.14159, 2);
-- 结果: 3.14
```

#### 2.3 `CEIL(number)`

返回大于或等于数字的最小整数。

**示例**：

```sql
SELECT CEIL(3.14);
-- 结果: 4
```

#### 2.4 `FLOOR(number)`

返回小于或等于数字的最大整数。

**示例**：

```sql
SELECT FLOOR(3.14);
-- 结果: 3
```

#### 2.5 `RAND()`

返回一个随机浮点数值，范围从 0 到 1。

**示例**：

```sql
SELECT RAND();
-- 结果: 0.345678 (每次结果不同)
```

#### 2.6 `MOD(x, y)`

返回 `x` 除以 `y` 的余数。

**示例**：

```sql
SELECT MOD(10, 3);
-- 结果: 1
```

#### 2.7 `GREATEST(val1, val2, ...)`

返回多个值中的最大值。

**示例**：

```sql
SELECT GREATEST(3, 5, 2, 7);
-- 结果: 7
```

#### 2.8 `LEAST(val1, val2, ...)`

返回多个值中的最小值。

**示例**：

```sql
SELECT LEAST(3, 5, 2, 7);
-- 结果: 2
```

---

### 3. **日期与时间函数（Date and Time Functions）**

用于日期和时间的处理。

#### 3.1 `CURDATE()`

返回当前日期。

**示例**：

```sql
SELECT CURDATE();
-- 结果: '2025-01-14'
```

#### 3.2 `NOW()`

返回当前的日期和时间。

**示例**：

```sql
SELECT NOW();
-- 结果: '2025-01-14 15:30:00'
```

#### 3.3 `DATE_ADD(date, INTERVAL value unit)`

对日期加上指定的时间间隔。

**示例**：

```sql
SELECT DATE_ADD('2025-01-14', INTERVAL 10 DAY);
-- 结果: '2025-01-24'
```

#### 3.4 `DATE_SUB(date, INTERVAL value unit)`

从日期减去指定的时间间隔。

**示例**：

```sql
SELECT DATE_SUB('2025-01-14', INTERVAL 10 DAY);
-- 结果: '2025-01-04'
```

#### 3.5 `YEAR(date)`

返回日期中的年份部分。

**示例**：

```sql
SELECT YEAR('2025-01-14');
-- 结果: 2025
```

#### 3.6 `MONTH(date)`

返回日期中的月份部分。

**示例**：

```sql
SELECT MONTH('2025-01-14');
-- 结果: 1
```

#### 3.7 `DAY(date)`

返回日期中的天部分。

**示例**：

```sql
SELECT DAY('2025-01-14');
-- 结果: 14
```

#### 3.8 `DATEDIFF(date1, date2)`

返回两个日期之间的天数差。

**示例**：

```sql
SELECT DATEDIFF('2025-01-14', '2025-01-01');
-- 结果: 13
```

#### 3.9 `STR_TO_DATE(str, format)`

将字符串转换为日期类型。

**示例**：

```sql
SELECT STR_TO_DATE('14-01-2025', '%d-%m-%Y');
-- 结果: '2025-01-14'
```

---

### 4. **聚合函数（Aggregate Functions）**

用于汇总数据的函数，通常与 `GROUP BY` 一起使用。

#### 4.1 `COUNT()`

返回查询结果中的记录数。

**示例**：

```sql
SELECT COUNT(*) FROM users;
-- 结果: 5
```

#### 4.2 `SUM()`

返回某列数值的总和。

**示例**：

```sql
SELECT SUM(price) FROM orders;
-- 结果: 1500
```

#### 4.3 `AVG()`

返回某列数值的平均值。

**示例**：

```sql
SELECT AVG(age) FROM users;
-- 结果: 27.5
```

#### 4.4 `MAX()`

返回某列的最大值。

**示例**：

```sql
SELECT MAX(salary) FROM employees;
-- 结果: 8000
```

#### 4.5 `MIN()`

返回某列的最小值。

**示例**：

```sql
SELECT MIN(salary) FROM employees;
-- 结果: 3000
```

---

### 5. **条件判断函数（Conditional Functions）**

用于执行条件判断。

#### 5.1 `IF(condition, true_value, false_value)`

如果条件为真，返回 `true_value`，否则返回 `false_value`。

**示例**：

```sql
SELECT IF(age > 18, 'Adult', 'Minor') FROM users;
-- 结果: 'Adult', 'Minor', ...
```

#### 5.2 `CASE`

执行类似于 `if-else` 的逻辑判断，支持多条件判断。

**示例**：

```sql
SELECT CASE
    WHEN age < 18 THEN 'Minor'
    WHEN age BETWEEN 18 AND 60 THEN 'Adult'
    ELSE 'Senior'
END AS age_group
FROM users;
```

---

### 6. **其他常用函数**

#### 6.1 `COALESCE()`

返回第一个非 `NULL` 的参数值。

**示例**：

```sql
SELECT COALESCE(NULL, 'Hello', 'World');
-- 结果: 'Hello'
```

#### 6.2 `NULLIF()`

如果两个表达式相等，返回 `NULL`，否则返回第一个表达式。

**示例**：

```sql
SELECT NULLIF(5, 5);
-- 结果: NULL
```

---

### 总结：

MySQL 提供了丰富的内置函数，帮助开发者在数据库层面进行各种数据处理和转换。常用的函数包括字符串函数、数字函数、日期时间函数、聚合函数等，熟练掌握这些函数能够大大提高数据处理的效率。
