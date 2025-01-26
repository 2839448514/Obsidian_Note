以下是 Flex 布局的 **容器属性** 和 **项目属性** 的完整详解，包含所有核心方法及其作用：

---

### 一、**容器属性** (Parent Container)

作用于 **Flex 容器**（设置 `display: flex` 的元素）

#### 1. `display`

- **作用**：定义 Flex 容器
- **值**：

  ```css
  .container {
    display: flex;       /* 块级 Flex 容器 */
    display: inline-flex; /* 行内 Flex 容器 */
  }
  ```

#### 2. `flex-direction`

- **作用**：定义主轴方向
- **值**：

  ```css
  .container {
    flex-direction: row;          /* 默认：水平排列（从左到右） */
    flex-direction: row-reverse;  /* 水平反向 */
    flex-direction: column;       /* 垂直排列（从上到下） */
    flex-direction: column-reverse; /* 垂直反向 */
  }
  ```

#### 3. `flex-wrap`

- **作用**：控制是否换行
- **值**：

  ```css
  .container {
    flex-wrap: nowrap;  /* 默认：不换行（可能溢出） */
    flex-wrap: wrap;    /* 自动换行 */
    flex-wrap: wrap-reverse; /* 反向换行 */
  }
  ```

#### 4. `flex-flow` (简写)

- **作用**：合并 `flex-direction` + `flex-wrap`
- **示例**：

  ```css
  .container {
    flex-flow: row wrap; /* 水平排列 + 自动换行 */
  }
  ```

#### 5. `justify-content`

- **作用**：**主轴**对齐方式
- **值**：

  ```css
  .container {
    justify-content: flex-start;   /* 默认：左对齐 */
    justify-content: flex-end;     /* 右对齐 */
    justify-content: center;       /* 居中 */
    justify-content: space-between;/* 两端对齐（项目间距相等） */
    justify-content: space-around; /* 项目两侧间距相等 */
    justify-content: space-evenly; /* 所有间距完全相等 */
  }
  ```

#### 6. `align-items`

- **作用**：**交叉轴**对齐方式（单行）
- **值**：

  ```css
  .container {
    align-items: stretch;    /* 默认：拉伸填满容器高度 */
    align-items: flex-start; /* 顶部对齐 */
    align-items: flex-end;   /* 底部对齐 */
    align-items: center;     /* 垂直居中 */
    align-items: baseline;   /* 基线对齐 */
  }
  ```

#### 7. `align-content`

- **作用**：**交叉轴**对齐方式（多行）
- **值**：（与 `justify-content` 相同）

  ```css
  .container {
    align-content: flex-start;
    align-content: space-between;
    /* ...其他值同上 */
  }
  ```

#### 8. `gap`

- **作用**：定义项目间距
- **示例**：

  ```css
  .container {
    gap: 10px;         /* 行与列间距相同 */
    gap: 20px 30px;    /* 行间距 20px，列间距 30px */
  }
  ```

---

### 二、**项目属性** (Child Items)

作用于 **Flex 项目**（容器的直接子元素）

#### 1. `order`

- **作用**：调整项目排列顺序
- **值**：

  ```css
  .item {
    order: 5; /* 默认 0，数值越小越靠前 */
  }
  ```

#### 2. `flex-grow`

- **作用**：定义项目的放大比例
- **值**：

  ```css
  .item {
    flex-grow: 1; /* 默认 0（不放大） */
  }
  ```

#### 3. `flex-shrink`

- **作用**：定义项目的缩小比例
- **值**：

  ```css
  .item {
    flex-shrink: 0; /* 默认 1（允许缩小） */
  }
  ```

#### 4. `flex-basis`

- **作用**：定义项目的初始尺寸
- **值**：

  ```css
  .item {
    flex-basis: 200px; /* 固定基准值 */
    flex-basis: auto;  /* 默认：根据内容计算 */
    flex-basis: 30%;   /* 百分比 */
  }
  ```

#### 5. `flex` (简写)

- **作用**：合并 `flex-grow` + `flex-shrink` + `flex-basis`
- **常用值**：

  ```css
  .item {
    flex: 1;            /* 等价于 1 1 0% */
    flex: 0 0 200px;    /* 不伸缩，固定 200px */
    flex: auto;         /* 等价于 1 1 auto */
  }
  ```

#### 6. `align-self`

- **作用**：覆盖容器的 `align-items` 设置
- **值**：

  ```css
  .item {
    align-self: auto;     /* 默认：继承容器设置 */
    align-self: flex-end; /* 单独设置底部对齐 */
  }
  ```

---

### 三、**核心概念图示**

| 属性分类      | 关键特性                                                                 |
|---------------|--------------------------------------------------------------------------|
| 容器属性      | 控制整体布局流向、对齐方式、换行行为                                     |
| 项目属性      | 控制单个项目的尺寸、顺序、对齐覆盖                                       |
| **主轴/交叉轴** | 方向由 `flex-direction` 决定，`row` 时主轴水平，`column` 时主轴垂直      |

---

### 四、**经典布局实现**

#### 1. 水平居中 + 垂直居中

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

#### 2. 等分剩余空间

```css
.item { flex: 1 } /* 所有项目等分 */
.item-2 { flex: 2 } /* 此项目占2份 */
```

#### 3. 响应式导航栏

```css
.nav {
  display: flex;
  gap: 20px;
  flex-wrap: wrap; /* 小屏幕自动换行 */
}
```

---

### 五、**注意事项**

1. **浏览器兼容性**：现代浏览器全面支持，IE11 需要 `-ms-` 前缀
2. **性能优化**：避免深层嵌套 Flex 容器
3. **替代方案**：复杂二维布局建议使用 CSS Grid

掌握这些属性和方法后，可以覆盖 90% 的常见布局需求。
