DML（数据操作语言）是数据库操作的核心，主要用于操作数据表中的数据。DML 主要包含以下四种基本操作：`INSERT`、`UPDATE`、`DELETE` 和 `SELECT`。这些操作可以帮助我们插入数据、更新数据、删除数据以及查询数据。以下是详细总结：

### 1. **插入数据（INSERT）**

`INSERT` 语句用于向表中插入数据。插入数据时，可以插入单条记录，也可以插入多条记录。

#### **1.1 插入单条记录**

**语法**：

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

**示例**：

```sql
INSERT INTO users (id, username, age, email)
VALUES (1, 'Alice', 25, 'alice@example.com');
```

#### **1.2 插入多条记录**

**语法**：

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...),
       (value1, value2, value3, ...),
       ...;
```

**示例**：

```sql
INSERT INTO users (id, username, age, email)
VALUES (2, 'Bob', 30, 'bob@example.com'),
       (3, 'Charlie', 22, 'charlie@example.com');
```

#### **1.3 插入数据时不指定列名**

如果插入的值顺序与表定义的列顺序一致，可以省略列名，直接插入所有列。

**示例**：

```sql
INSERT INTO users
VALUES (4, 'David', 28, 'david@example.com');
```

---

### 2. **更新数据（UPDATE）**

`UPDATE` 语句用于修改表中已经存在的记录。通常，`UPDATE` 会与 `WHERE` 子句一起使用，指定更新的条件，避免修改所有记录。

#### **2.1 更新单条或多条记录**

**语法**：

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**示例**：

```sql
UPDATE users
SET age = 26, email = 'alice_new@example.com'
WHERE id = 1;
```

#### **2.2 更新多个列**

可以在一个 `UPDATE` 语句中同时更新多个列。

**示例**：

```sql
UPDATE users
SET age = 28, email = 'bob_new@example.com'
WHERE id = 2;
```

**注意**：没有 `WHERE` 条件时，整个表的记录都会被更新，所以在使用 `UPDATE` 时一定要加上条件，防止全表更新。

---

### 3. **删除数据（DELETE）**

`DELETE` 语句用于删除表中的记录。和 `UPDATE` 一样，`DELETE` 通常与 `WHERE` 子句配合使用，用来指定删除条件，避免删除全部记录。

#### **3.1 删除单条或多条记录**

**语法**：

```sql
DELETE FROM table_name
WHERE condition;
```

**示例**：

```sql
DELETE FROM users
WHERE id = 1;
```

#### **3.2 删除所有记录**

如果没有 `WHERE` 子句，`DELETE` 会删除表中的所有记录，但表结构依然存在。

**示例**：

```sql
DELETE FROM users;
```

**注意**：`DELETE` 是一个永久性的操作，一旦删除数据，不能直接恢复。所以在删除数据时要非常小心。

---

### 4. **查询数据（SELECT）**

`SELECT` 语句用于从表中查询数据。`SELECT` 是最常用的 DML 操作之一，支持多种条件和排序方式。它可以检索表中的单列、多列、单行或多行数据。

#### **4.1 查询所有列的数据**

**语法**：

```sql
SELECT * FROM table_name;
```

**示例**：

```sql
SELECT * FROM users;
```

#### **4.2 查询指定列的数据**

**语法**：

```sql
SELECT column1, column2, ... FROM table_name;
```

**示例**：

```sql
SELECT username, email FROM users;
```

#### **4.3 使用 WHERE 进行条件查询**

`WHERE` 子句用于指定查询的条件，可以进行条件过滤。

**语法**：

```sql
SELECT * FROM table_name
WHERE condition;
```

**示例**：

```sql
SELECT * FROM users
WHERE age > 25;
```

#### **4.4 排序查询结果（ORDER BY）**

`ORDER BY` 用于对查询结果进行排序。可以指定排序列，并指定升序（`ASC`）或降序（`DESC`）。

**语法**：

```sql
SELECT * FROM table_name
ORDER BY column_name [ASC|DESC];
```

**示例**：

```sql
SELECT * FROM users
ORDER BY age DESC;
```

#### **4.5 使用 LIMIT 限制返回结果数量**

`LIMIT` 用于限制查询返回的记录数量，常用于分页查询。

**语法**：

```sql
SELECT * FROM table_name
LIMIT number;
```

**示例**：

```sql
SELECT * FROM users
LIMIT 5;
```

#### **4.6 聚合查询（GROUP BY）**

`GROUP BY` 用于对数据进行分组，常配合聚合函数（如 `COUNT`、`SUM`、`AVG`）一起使用。

**语法**：

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;
```

**示例**：

```sql
SELECT age, COUNT(*) AS count
FROM users
GROUP BY age;
```

#### **4.7 使用 HAVING 过滤分组后的结果**

`HAVING` 用于在 `GROUP BY` 分组后过滤结果集，类似于 `WHERE`，但 `WHERE` 是对行数据进行过滤，`HAVING` 是对分组后的结果进行过滤。

**语法**：

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**示例**：

```sql
SELECT age, COUNT(*) AS count
FROM users
GROUP BY age
HAVING COUNT(*) > 1;
```

#### **4.8 使用 JOIN 联接查询**

`JOIN` 用于将多个表的数据联合在一起。常见的有 `INNER JOIN`、`LEFT JOIN`、`RIGHT JOIN` 和 `FULL JOIN`。

**语法**：

```sql
SELECT column1, column2, ...
FROM table1
JOIN table2
ON table1.column_name = table2.column_name;
```

**示例**：

```sql
SELECT users.username, orders.order_id
FROM users
JOIN orders
ON users.id = orders.user_id;
```

#### **4.9 子查询（Subquery）**

子查询是嵌套在另一个查询中的查询，通常用于处理更复杂的查询。

**语法**：

```sql
SELECT column_name
FROM table_name
WHERE column_name = (SELECT column_name FROM table_name WHERE condition);
```

**示例**：

```sql
SELECT username
FROM users
WHERE id = (SELECT user_id FROM orders WHERE order_id = 1);
```

---

### **DML 操作总结**

- **INSERT**：用于向表中插入数据，支持单条或多条插入，且可以指定列名。
- **UPDATE**：用于更新表中已有数据，通常与 `WHERE` 子句一起使用，以避免全表更新。
- **DELETE**：用于删除表中的数据，通常与 `WHERE` 子句一起使用，以避免删除整个表的数据。
- **SELECT**：用于查询数据，支持条件查询、排序、限制返回数量、分组查询、聚合函数、联接查询等复杂操作。

### **附加说明**

- `SELECT` 是最常用的 DML 操作，它不仅用于基本的数据查询，还可用于复杂的查询，如聚合、排序、联接、多表查询等。
- DML 操作一般会影响表中的数据，因此要谨慎使用，尤其是 `UPDATE` 和 `DELETE` 操作，确保条件明确，以防误操作。
