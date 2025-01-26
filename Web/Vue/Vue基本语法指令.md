### 1. `v-bind`

用于动态绑定一个或多个属性，或者一个组件 prop。

```html
<!-- 绑定 href 属性 -->
<a v-bind:href="url">点击这里</a>
```

等价于：

```html
<a :href="url">点击这里</a>
```

### 2. `v-model`

用于创建双向绑定，常用于表单输入元素。

```html
<!-- 创建一个输入框，值与 data 中的 message 绑定 -->
<input v-model="message">
```

### 3. `v-for`

用于遍历数组或对象，渲染列表。

```html
<!-- 渲染一个数组 -->
<ul>
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
</ul>
```

### 4. `v-if`, `v-else-if`, `v-else`

用于条件渲染，根据条件显示或隐藏元素。

```html
<!-- 如果 isVisible 为 true，显示此元素 -->
<div v-if="isVisible">我可见</div>
```

### 5. `v-show`

与 `v-if` 类似，但通过 CSS 来控制显示与隐藏，元素始终会被渲染，只是通过 `display: none` 来隐藏。

```html
<div v-show="isVisible">我可见</div>
```

### 6. `v-on`

用于绑定事件监听器。

```html
<!-- 绑定点击事件 -->
<button v-on:click="doSomething">点击</button>
```

等价于：

```html
<button @click="doSomething">点击</button>
```

### 7. `v-bind:class`

动态绑定类。

```html
<div v-bind:class="className">这是一个 div</div>
```

等价于：

```html
<div :class="className">这是一个 div</div>
```

### 8. `v-bind:style`

动态绑定样式。

```html
<div v-bind:style="styleObject">样式绑定</div>
```

等价于：

```html
<div :style="styleObject">样式绑定</div>
```

### 9. `v-text` 和 `v-html`

分别用于设置元素的文本内容或 HTML 内容。

```html
<!-- 设置文本内容 -->
<div v-text="message"></div>

<!-- 设置 HTML 内容 -->
<div v-html="htmlContent"></div>
```

### 10. `v-pre`

跳过这个元素和它的子元素的编译过程，通常用于显示原始的 HTML 代码。

```html
<div v-pre>{{ rawTemplate }}</div>
```

### 11. `v-cloak`

用于保持元素和子元素在 Vue 实例准备好之前不被渲染出来，通常配合 CSS 使用。

```html
<div v-cloak>{{ message }}</div>
```

### 12. `v-once`

使元素和它的子元素只渲染一次。

```html
<div v-once>只渲染一次</div>
```

