### **MySQL 数据类型及其作用和特点**

MySQL 数据库支持多种数据类型，这些数据类型可以分为几类：**数值类型**、**日期和时间类型**、**字符串类型**、**二进制类型**、**JSON 类型**等。每种数据类型都有其特定的用途和特点，下面详细介绍这些常见的 MySQL 数据类型。

---

### **1. 数值类型（Numeric Data Types）**

数值类型用于存储整数和小数数据。它们主要用于数值计算、计数和定量分析。

#### **整数类型**

- **`TINYINT`**

	- 存储范围：`-128 to 127`（有符号）或 `0 to 255`（无符号）
	- 存储大小：1 字节
	- **用途**：适用于存储非常小的整数，如状态标志、布尔值等。
- **`SMALLINT`**

	- 存储范围：`-32,768 to 32,767`（有符号）或 `0 to 65,535`（无符号）
	- 存储大小：2 字节
	- **用途**：适用于存储中小范围的整数。
- **`MEDIUMINT`**

	- 存储范围：`-8,388,608 to 8,388,607`（有符号）或 `0 to 16,777,215`（无符号）
	- 存储大小：3 字节
	- **用途**：适用于存储较大范围的整数。
- **`INT`（或 `INTEGER`）**

	- 存储范围：`-2,147,483,648 to 2,147,483,647`（有符号）或 `0 to 4,294,967,295`（无符号）
	- 存储大小：4 字节
	- **用途**：常用于存储常规的整数值，如数量、用户 ID 等。
- **`BIGINT`**

	- 存储范围：`-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807`（有符号）或 `0 to 18,446,744,073,709,551,615`（无符号）
	- 存储大小：8 字节
	- **用途**：用于存储极大整数，如大范围的计数器、时间戳等。

#### **浮动类型**

- **`FLOAT`**

	- 存储范围：`-3.402823466E+38 to 3.402823466E+38`
	- 存储大小：4 字节
	- **用途**：用于存储单精度浮点数，适合存储大范围的浮动数据。
- **`DOUBLE`**

	- 存储范围：`-1.7976931348623157E+308 to 1.7976931348623157E+308`
	- 存储大小：8 字节
	- **用途**：用于存储双精度浮点数，精度比 `FLOAT` 高，适用于需要更高精度的计算。
- **`DECIMAL`（或 `NUMERIC`）**

	- 存储范围：可以根据精度和小数位来指定
	- 存储大小：根据精度和小数位的不同，存储大小变化
	- **用途**：用于存储精确的小数值，常用于财务数据、货币计算等要求精确的情况。可以指定数字的总位数和小数位数，如 `DECIMAL(10,2)`。

---

### **2. 日期和时间类型（Date and Time Data Types）**

用于存储日期、时间、日期时间等信息。

- **`DATE`**

	- 存储格式：`YYYY-MM-DD`
	- 存储大小：3 字节
	- **用途**：用于存储日期，不包含时间部分。
- **`DATETIME`**

	- 存储格式：`YYYY-MM-DD HH:MM:SS`
	- 存储大小：8 字节
	- **用途**：用于存储日期和时间信息，适合于需要记录日期和时间的场景。
- **`TIMESTAMP`**

	- 存储格式：`YYYY-MM-DD HH:MM:SS`
	- 存储大小：4 字节
	- **用途**：与 `DATETIME` 类似，但 `TIMESTAMP` 依据 UTC 时间戳存储，并且在插入或更新时会自动更新，适合记录创建和修改时间。
- **`TIME`**

	- 存储格式：`HH:MM:SS`
	- 存储大小：3 字节
	- **用途**：用于存储时间，不包含日期部分。
- **`YEAR`**

	- 存储格式：`YYYY`
	- 存储大小：1 字节
	- **用途**：用于存储年份。

---

### **3. 字符串类型（String Data Types）**

用于存储文本数据，通常为字符或字符集类型。

- **`CHAR`**

	- 存储格式：固定长度的字符串
	- 存储大小：1 到 255 字节
	- **用途**：存储定长的字符串，如固定格式的编码、ID 等。
- **`VARCHAR`**

	- 存储格式：变长字符串
	- 存储大小：1 到 65,535 字节
	- **用途**：存储变长字符串，适合存储可变长度的文本数据，如名字、地址等。
- **`TEXT`**

	- 存储格式：长文本数据
	- 存储大小：最多 65,535 字节
	- **用途**：适用于存储大段文本，如文章内容、用户评论等。
- **`MEDIUMTEXT`**

	- 存储格式：更长的文本数据
	- 存储大小：最多 16,777,215 字节
	- **用途**：适用于存储非常大的文本数据，如小说、日志等。
- **`LONGTEXT`**

	- 存储格式：极长的文本数据
	- 存储大小：最多 4,294,967,295 字节
	- **用途**：用于存储超大文本数据。
- **`BLOB`**（Binary Large Object）

	- 存储格式：二进制数据
	- 存储大小：最多 65,535 字节
	- **用途**：存储二进制数据，如图片、音频、视频等。
- **`ENUM`**

	- 存储格式：从预定义的值集中选择一个
	- 存储大小：1 到 2 字节
	- **用途**：适用于列出有限个选项的数据，如性别、状态等。
- **`SET`**

	- 存储格式：可选多个预定义值
	- 存储大小：1 到 8 字节
	- **用途**：适用于存储多个选项的组合，如一个人可能有多个兴趣爱好。

---

### **4. 二进制类型（Binary Data Types）**

用于存储二进制数据，如文件、图片、音频等。

- **`BINARY`**

	- 存储格式：定长的二进制数据
	- 存储大小：1 到 255 字节
	- **用途**：存储固定长度的二进制数据。
- **`VARBINARY`**

	- 存储格式：变长的二进制数据
	- 存储大小：1 到 65,535 字节
	- **用途**：存储变长的二进制数据。
- **`BLOB`**

	- 存储格式：二进制大型对象
	- 存储大小：最多 65,535 字节
	- **用途**：存储大容量的二进制数据，如图片、音频文件等。

---

### **5. JSON 类型**

- **`JSON`**
	- 存储格式：用于存储 JSON 格式的数据。
	- 存储大小：根据数据大小而定
	- **用途**：存储 JSON 数据，适用于需要存储键值对结构的数据。

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    attributes JSON
);
```

---

### **总结**

|数据类型|用途|特点|
|---|---|---|
|`TINYINT`|小范围整数|1 字节，-128 到 127（有符号）|
|`INT`|常规整数|4 字节，适用于大部分整数数据|
|`BIGINT`|超大整数|8 字节，适用于极大整数|
|`FLOAT`|单精度浮点数|4|

字节，用于存储小数 | | `DECIMAL` | 精确的小数 | 存储精确的小数，如财务数据 | | `DATE` | 日期 | 3 字节，仅存储日期 | | `DATETIME`| 日期和时间 | 8 字节，存储日期和时间 | | `VARCHAR` | 变长字符串 | 字符串类型，适合存储可变长度的文本数据 | | `TEXT` | 长文本数据 | 用于存储大文本，如文章、评论等 | | `ENUM` | 枚举值 | 存储固定的有限选项 | | `BLOB` | 二进制大对象 | 存储大二进制数据，如文件、图片、视频等 | | `JSON` | JSON 格式数据 | 存储 JSON 格式的数据，适合灵活的数据结构 |

通过合理选择数据类型，可以确保数据存储高效、准确，同时提高数据库的查询性能和稳定性。
