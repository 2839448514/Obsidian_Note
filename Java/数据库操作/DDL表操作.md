好的，下面是 **DDL（数据定义语言）** 的详细总结。DDL 是用来定义和管理数据库结构的语言，包括创建、修改、删除数据库和数据表等操作。

### **1. 创建数据库（CREATE DATABASE）**

用于创建一个新的数据库。可以指定字符集、排序规则等选项。

**语法**：

```sql
CREATE DATABASE database_name;
```

**示例**：

```sql
CREATE DATABASE test_db;
```

可以指定字符集和排序规则：

```sql
CREATE DATABASE test_db CHARACTER SET utf8 COLLATE utf8_general_ci;
```

### **2. 删除数据库（DROP DATABASE）**

删除一个已存在的数据库，注意删除操作会删除数据库中的所有内容（包括表、视图等）。

**语法**：

```sql
DROP DATABASE database_name;
```

**示例**：

```sql
DROP DATABASE test_db;
```

### **3. 使用数据库（USE）**

选择当前数据库，之后的操作都会在该数据库中进行。

**语法**：

```sql
USE database_name;
```

**示例**：

```sql
USE test_db;
```

---

### **4. 创建表（CREATE TABLE）**

用于创建一个新表，并定义该表的列、数据类型及约束。创建表时，指定每一列的名称、数据类型、默认值、约束等。

**语法**：

```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    ...
);
```

**示例**：

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    age INT,
    email VARCHAR(100),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

### **5. 删除表（DROP TABLE）**

用于删除一个已有的表，删除后无法恢复，数据也会丢失。

**语法**：

```sql
DROP TABLE table_name;
```

**示例**：

```sql
DROP TABLE users;
```

### **6. 修改表（ALTER TABLE）**

修改已存在的表。可以用来添加、删除或修改列，也可以修改表的其他结构（如增加主键、外键、索引等）。

#### **1. 添加列（ADD）**

```sql
ALTER TABLE table_name ADD column_name datatype;
```

**示例**：

```sql
ALTER TABLE users ADD phone_number VARCHAR(15);
```

#### **2. 删除列（DROP COLUMN）**

```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

**示例**：

```sql
ALTER TABLE users DROP COLUMN phone_number;
```

#### **3. 修改列的数据类型（MODIFY COLUMN）**

```sql
ALTER TABLE table_name MODIFY COLUMN column_name new_datatype;
```

**示例**：

```sql
ALTER TABLE users MODIFY COLUMN email VARCHAR(255);
```

#### **4. 重命名列（RENAME COLUMN）**

```sql
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;
```

**示例**：

```sql
ALTER TABLE users RENAME COLUMN email TO user_email;
```

#### **5. 添加约束（ADD CONSTRAINT）**

```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition;
```

**示例**：

```sql
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);
```

---

### **7. 修改表名（RENAME TABLE）**

重命名表。

**语法**：

```sql
RENAME TABLE old_table_name TO new_table_name;
```

**示例**：

```sql
RENAME TABLE users TO app_users;
```

---

### **8. 查看表结构（DESCRIBE 或 SHOW COLUMNS）**

查看表的结构，包括列名、数据类型、是否允许 NULL、键等信息。

**语法**：

```sql
DESCRIBE table_name;
```

或者：

```sql
SHOW COLUMNS FROM table_name;
```

**示例**：

```sql
DESCRIBE users;
```

---

### **9. 查看数据库列表（SHOW DATABASES）**

列出所有的数据库。

**语法**：

```sql
SHOW DATABASES;
```

---

### **10. 查看表列表（SHOW TABLES）**

列出当前数据库中的所有表。

**语法**：

```sql
SHOW TABLES;
```

---

### **11. 修改数据库（ALTER DATABASE）**

修改现有数据库的选项（如字符集、排序规则等）。

**语法**：

```sql
ALTER DATABASE database_name [OPTIONS];
```

**示例**：

```sql
ALTER DATABASE test_db CHARACTER SET utf8 COLLATE utf8_general_ci;
```

---

### **12. 数据库备份（mysqldump）**

使用 `mysqldump` 命令进行数据库备份，注意这不是 SQL 语法，但在实际应用中非常重要。

**命令**：

```bash
mysqldump -u username -p database_name > backup_file.sql
```

**示例**：

```bash
mysqldump -u root -p test_db > test_db_backup.sql
```

---

### **13. 数据库恢复（mysql）**

使用 `mysql` 命令恢复数据库。

**命令**：

```bash
mysql -u username -p database_name < backup_file.sql
```

**示例**：

```bash
mysql -u root -p test_db < test_db_backup.sql
```

---

### **14. 查看所有约束（SHOW CREATE TABLE）**

查看表的创建语句以及所有约束的详细信息。

**语法**：

```sql
SHOW CREATE TABLE table_name;
```

**示例**：

```sql
SHOW CREATE TABLE users;
```

---

### **总结**

1. **创建数据库**：`CREATE DATABASE`
2. **删除数据库**：`DROP DATABASE`
3. **选择数据库**：`USE`
4. **创建表**：`CREATE TABLE`
5. **删除表**：`DROP TABLE`
6. **修改表结构**：`ALTER TABLE`
	- 添加、删除列
	- 修改列的数据类型
	- 添加/删除约束
7. **查看表结构**：`DESCRIBE` 或 `SHOW COLUMNS`
8. **查看数据库/表列表**：`SHOW DATABASES` 和 `SHOW TABLES`
9. **修改数据库**：`ALTER DATABASE`
10. **备份/恢复数据库**：`mysqldump` 和 `mysql`
11. **查看约束和创建语句**：`SHOW CREATE TABLE`

这些操作帮助管理数据库和数据表的结构，确保数据存储和查询效率，并且保证数据的完整性与安全性。
