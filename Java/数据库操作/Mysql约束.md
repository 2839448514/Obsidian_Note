### **约束（Constraints）**

约束是用于确保数据库中数据的完整性和一致性的一种规则。数据库约束主要通过限制数据的有效性和合规性来保证数据的质量。约束可以作用于数据库表的列或整个表。常见的约束有：**`PRIMARY KEY`**、**`FOREIGN KEY`**、**`UNIQUE`**、**`NOT NULL`**、**`CHECK`** 和 **`DEFAULT`**。

---

### **1. `PRIMARY KEY`（主键）**

#### **作用**：

- 主键用于唯一标识表中的每一行记录。
- 主键列的值必须是唯一的，且不能为 `NULL`。
- 一个表只能有一个主键。

#### **特点**：

- 唯一性：表中的每一行数据都必须是唯一的。
- 非空：不能有 `NULL` 值。

#### **示例**：

```sql
CREATE TABLE `user` (
    `id` BIGINT NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL,
    `email` VARCHAR(100) NOT NULL,
    PRIMARY KEY (`id`)
);
```

- `id` 列为主键，确保每个用户的 `id` 唯一且不能为空。

---

### **2. `FOREIGN KEY`（外键）**

#### **作用**：

- 外键用于在两个表之间建立关系。
- 外键列指向另一个表的主键或唯一键，确保数据的一致性和完整性。

#### **特点**：

- 外键约束保证了参照完整性，即参照的值在被引用表中必须存在。
- 如果外键约束表的列的值不存在于主键表中，则会发生错误。
- 外键通常用于一对多或多对多关系。

#### **示例**：

```sql
CREATE TABLE `order` (
    `order_id` BIGINT NOT NULL AUTO_INCREMENT,
    `user_id` BIGINT NOT NULL,
    `order_date` DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`order_id`),
    FOREIGN KEY (`user_id`) REFERENCES `user`(`id`)
);
```

- `user_id` 列是外键，指向 `user` 表中的 `id` 列。
- 这表示每个订单都属于某个用户，且用户必须在 `user` 表中存在。

---

### **3. `UNIQUE`（唯一约束）**

#### **作用**：

- 唯一约束确保列中的所有值都是唯一的。
- 唯一约束允许 `NULL` 值（根据数据库不同，可能有差异），但每个 `NULL` 只能出现一次。
- 可以在多个列上设置 `UNIQUE` 约束。

#### **示例**：

```sql
CREATE TABLE `user` (
    `id` BIGINT NOT NULL AUTO_INCREMENT,
    `email` VARCHAR(100) UNIQUE,
    PRIMARY KEY (`id`)
);
```

- `email` 列设置为唯一约束，确保表中没有两个用户有相同的邮箱地址。

---

### **4. `NOT NULL`（非空约束）**

#### **作用**：

- 非空约束确保列中的数据不能为 `NULL`。
- 当创建表时，可以在某些列上设置 `NOT NULL`，以确保这些列必须包含有效数据。

#### **示例**：

```sql
CREATE TABLE `user` (
    `id` BIGINT NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL,
    `email` VARCHAR(100) NOT NULL,
    PRIMARY KEY (`id`)
);
```

- `name` 和 `email` 列必须提供有效数据，不能是 `NULL`。

---

### **5. `CHECK`（检查约束）**

#### **作用**：

- 检查约束用于限制列中的数据必须满足指定的条件。
- 可以用于数字范围、日期范围或特定的字符模式。

#### **示例**：

```sql
CREATE TABLE `user` (
    `id` BIGINT NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL,
    `age` INT CHECK (`age` >= 18),
    PRIMARY KEY (`id`)
);
```

- `age` 列设置了一个检查约束，确保用户的年龄大于等于 18 岁。

---

### **6. `DEFAULT`（默认值约束）**

#### **作用**：

- 默认值约束用于为列指定默认值，如果插入数据时该列未提供值，则自动使用默认值。
- 适用于所有类型的列（数字、日期、字符串等）。

#### **示例**：

```sql
CREATE TABLE `user` (
    `id` BIGINT NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
);
```

- `create_time` 列默认值为当前时间戳（`CURRENT_TIMESTAMP`），如果插入数据时未提供该列值，则使用当前时间作为默认值。

---

### **7. `INDEX`（索引）**

#### **作用**：

- 索引用于提高查询性能。索引是基于某个列的值建立的数据结构，可以加速查询速度。
- 索引可以是唯一的（`UNIQUE`）或非唯一的。

#### **示例**：

```sql
CREATE INDEX idx_user_email ON `user`(`email`);
```

- 在 `email` 列上创建索引，提升基于 `email` 的查询性能。

---

### **8. 组合约束**

#### **作用**：

- 有时需要对多列组合创建唯一性或检查约束。可以使用 **组合约束** 来确保一组列的组合值是唯一的或符合条件。

#### **示例**：

```sql
CREATE TABLE `order` (
    `order_id` BIGINT NOT NULL AUTO_INCREMENT,
    `user_id` BIGINT NOT NULL,
    `order_date` DATETIME,
    PRIMARY KEY (`order_id`),
    CONSTRAINT `unique_user_order` UNIQUE (`user_id`, `order_date`)
);
```

- `user_id` 和 `order_date` 组合在一起必须唯一，确保一个用户在同一天内不能有多个订单。

---

### **总结**

|约束类型|作用|示例|
|---|---|---|
|`PRIMARY KEY`|确保列的值唯一且不为 `NULL`，用于唯一标识每一行记录。|`PRIMARY KEY (id)`|
|`FOREIGN KEY`|用于建立和其他表的引用关系，确保数据的一致性和完整性。|`FOREIGN KEY (user_id) REFERENCES user(id)`|
|`UNIQUE`|确保列中的所有值唯一。|`UNIQUE (email)`|
|`NOT NULL`|确保列中的数据不能为 `NULL`。|`NOT NULL (name)`|
|`CHECK`|用于限制列的数据必须符合特定条件。|`CHECK (age >= 18)`|
|`DEFAULT`|用于为列设置默认值，如果插入数据时未提供该列的值，则使用默认值。|`DEFAULT CURRENT_TIMESTAMP`|
|`INDEX`|用于提高查询性能，通常基于表中的列建立索引。|`CREATE INDEX idx_user_email ON user(email)`|

---

### **注意事项**：

1. **复合约束**：可以组合多个约束来确保数据的完整性，比如复合主键、复合外键等。
2. **索引与性能**：尽管索引能提高查询速度，但不恰当的使用索引会影响插入、更新和删除操作的性能，因此要根据查询模式合理设计索引。
3. **外键和参照完整性**：外键可以确保数据的一致性，避免数据孤立或不一致。