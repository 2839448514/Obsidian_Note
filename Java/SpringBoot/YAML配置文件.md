YAML（YAML Ain't Markup Language）是一种数据序列化格式，常用于配置文件（如 Spring Boot 配置、Docker Compose 文件等）。在 YAML 中，有多种数据类型可以表示不同的数据结构。下面是 YAML 中常用的数据类型及其解释：

### 1. **字符串（String）**

- 字符串是 YAML 中最常见的数据类型之一。字符串可以通过单引号（`'`）或双引号（`"`）表示，也可以直接表示（不加引号）。

**示例：**

```yaml
name: "John Doe"
city: 'New York'
description: This is a sample description
```

- **多行字符串**： YAML 支持两种多行字符串格式：折叠样式（`>`）和保留样式（`|`）。
    
    - **折叠样式**：去除换行符，换行变为空格。
        
        ```yaml
        folded_example: >
          This is a long
          string that is folded
          into multiple lines.
        ```
        
    - **保留样式**：保留所有的换行符。
        
        ```yaml
        preserved_example: |
          This is a long
          string with preserved
          newlines.
        ```
        

### 2. **数字（Number）**

YAML 支持整数和浮动数字。没有引号时，数字会被自动识别。

**示例：**

```yaml
age: 25        # 整数
height: 5.9     # 浮动数字
```

- **支持科学计数法**：
    
    ```yaml
    large_number: 1.23e4  # 12300
    ```
    

### 3. **布尔值（Boolean）**

布尔值可以用 `true` 或 `false`，也可以使用 `yes` / `no`，`on` / `off` 等表示。

**示例：**

```yaml
is_active: true
is_admin: no
```

### 4. **空值（Null）**

YAML 中的 `null` 表示空值或缺失值。可以使用 `null` 或 `~` 来表示。

**示例：**

```yaml
field1: null
field2: ~
```

### 5. **日期和时间（Date/Time）**

YAML 使用 ISO 8601 格式表示日期和时间。

**示例：**

```yaml
birthday: 1995-10-15
timestamp: 2025-01-16T12:00:00Z
```

- 你可以省略时区或时间部分，只保留日期。

### 6. **数组（Array）**

YAML 支持通过多行或内联方式表示数组。数组的元素由破折号（`-`）或方括号（`[]`）表示。

**示例：**

```yaml
fruits:
  - Apple
  - Banana
  - Orange
  
numbers: [1, 2, 3, 4]
```

- **多维数组**：
    
    ```yaml
    matrix:
      - [1, 2, 3]
      - [4, 5, 6]
      - [7, 8, 9]
    ```
    

### 7. **对象（Object）**

YAML 支持表示字典或对象类型，通常使用缩进来表示层级结构，键值对用冒号（`:`）隔开。

**示例：**

```yaml
person:
  name: John
  age: 25
  address:
    city: New York
    zip: 10001
```

- **内联表示对象**：
    
    ```yaml
    person: {name: John, age: 25, city: New York}
    ```
    

### 8. **锚点和引用（Anchors & Aliases）**

YAML 允许你为一个值创建锚点，并在其他地方引用该值。这对于避免重复数据非常有用。

**示例：**

```yaml
defaults: &defaults
  color: red
  size: large

item1:
  <<: *defaults
  shape: circle

item2:
  <<: *defaults
  shape: square
```

这里，`&defaults` 是一个锚点，`*defaults` 引用该锚点。

### 9. **复杂数据类型（Map of Arrays, Arrays of Maps）**

YAML 支持更复杂的数据结构，数组可以包含字典，字典可以包含数组。

**示例：**

```yaml
users:
  - name: John
    roles: [admin, user]
  - name: Jane
    roles: [user]
```

**示例：数组中嵌套对象**：

```yaml
people:
  - {name: John, age: 30}
  - {name: Jane, age: 25}
```

### 10. **自定义数据类型（Custom Types）**

通过定义自己的数据类型来表示复杂的数据结构（例如，标签和类型）。这通常用于特殊场景，但较少见。

**示例：**

```yaml
!mytype
value: 123
```

### 11. **引用和合并（Merge Key）**

YAML 允许通过合并来简化配置，特别是在多个配置项有重叠时。

**示例：**

```yaml
default: &default
  color: blue
  size: medium

item1:
  <<: *default
  shape: circle

item2:
  <<: *default
  shape: square
```

### 12. **注释（Comment）**

YAML 支持注释，注释以 `#` 开头，直到行尾。

**示例：**

```yaml
# 这是一个注释
name: "John Doe"  # 这也是注释
```

### 小结

- **基础数据类型**：字符串、数字、布尔值、空值、日期时间。
- **数据结构**：数组（列表）、对象（字典）、复杂对象。
- **引用与合并**：锚点和别名、合并键。
- **格式**：支持多行字符串、内联格式和键值对格式。

YAML 的灵活性使得它成为了很多配置文件和数据交换的标准格式，尤其适用于需要简单、可读性强的配置文件。