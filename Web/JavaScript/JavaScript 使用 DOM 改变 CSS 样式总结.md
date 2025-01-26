### JavaScript 使用 DOM 改变 CSS 样式总结

JavaScript可以通过DOM操作动态修改HTML元素的CSS样式。可以直接修改内联样式，也可以通过操作元素的类来修改样式。此外，JavaScript还支持通过动态插入或修改CSS规则来实现页面样式的调整。

#### 1. 直接修改内联样式

可以通过 `style` 属性直接修改元素的内联样式。通过这种方式，修改的样式会直接应用到元素上。

- **修改单一样式属性**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.style.backgroundColor = "red";  // 修改背景色为红色
    element.style.fontSize = "20px";        // 修改字体大小为20px
    element.style.marginTop = "10px";       // 修改上外边距为10px
    ```
    
- **修改多个样式属性**： 可以连续修改多个样式：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.style.cssText = "background-color: blue; font-size: 18px; color: white;";
    ```
    

> **注意**：修改的样式是内联样式，优先级高于外部或内部样式表的规则。

#### 2. 使用 `classList` 修改类名

通过操作元素的 `classList` 可以动态添加、删除或切换CSS类。这种方式常用于根据条件动态切换样式。

- **添加类**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.classList.add("newClass");  // 添加 "newClass" 类
    ```
    
- **删除类**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.classList.remove("oldClass");  // 删除 "oldClass" 类
    ```
    
- **切换类**： `toggle()` 方法会根据当前类名是否存在，自动添加或删除该类。
    
    ```javascript
    let element = document.getElementById("myElement");
    element.classList.toggle("active");  // 如果有 "active" 类，则删除，否则添加
    ```
    
- **检查类是否存在**：
    
    ```javascript
    let element = document.getElementById("myElement");
    let hasClass = element.classList.contains("active");  // 判断是否含有 "active" 类
    ```
    

#### 3. 通过 `setAttribute()` 修改样式

如果元素没有直接的 `style` 属性，也可以使用 `setAttribute()` 方法来动态修改样式。

```javascript
let element = document.getElementById("myElement");
element.setAttribute("style", "background-color: green; color: white; font-size: 18px;");
```

> 这种方式会覆盖原有的 `style` 属性，因此要注意合并样式。

#### 4. 修改外部或内部 CSS 样式表

JavaScript不仅能修改内联样式，还可以修改整个页面的CSS样式表。通过操作 `document.styleSheets` 可以动态插入、删除或修改CSS规则。

- **修改现有规则**： 假设有一个外部或内部CSS文件，其中定义了一个类 `.myClass`，可以通过JS动态修改该类的样式。
    
    ```javascript
    let stylesheet = document.styleSheets[0];  // 获取第一个样式表
    let rules = stylesheet.cssRules || stylesheet.rules;  // 获取规则
    for (let rule of rules) {
      if (rule.selectorText === ".myClass") {
        rule.style.backgroundColor = "yellow";  // 修改背景色
      }
    }
    ```
    
- **添加新的CSS规则**： 使用`insertRule()`方法可以动态向样式表中添加新规则。
    
    ```javascript
    let stylesheet = document.styleSheets[0];
    stylesheet.insertRule(".newClass { color: red; font-size: 16px; }", stylesheet.cssRules.length);
    ```
    
- **删除现有的CSS规则**： 使用`deleteRule()`方法删除样式表中的特定规则。
    
    ```javascript
    let stylesheet = document.styleSheets[0];
    stylesheet.deleteRule(0);  // 删除第一个规则
    ```
    

#### 5. 动态加载或修改外部样式表

JavaScript还可以动态加载外部CSS文件或修改当前的样式表链接：

- **添加新的外部CSS文件**：
    
    ```javascript
    let link = document.createElement("link");
    link.rel = "stylesheet";
    link.href = "styles.css";  // 指定新的CSS文件
    document.head.appendChild(link);  // 添加到页面头部
    ```
    
- **替换现有的CSS文件**： 可以通过修改 `link` 元素的 `href` 属性来动态切换CSS文件。
    
    ```javascript
    let link = document.getElementById("stylesheet");
    link.href = "new-styles.css";  // 替换CSS文件
    ```
    

#### 6. 动画与过渡效果

JavaScript通过修改元素的CSS样式来控制动画和过渡效果。例如，可以通过修改元素的 `transition` 或 `animation` 属性，结合JavaScript事件或定时器来实现动态效果。

- **简单的过渡效果**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.style.transition = "all 0.5s ease-in-out";
    element.style.transform = "scale(1.5)";  // 通过样式改变触发过渡动画
    ```
    
- **添加CSS动画**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.style.animation = "bounce 2s infinite";  // 设置动画名称和时间
    ```
    

#### 7. 通过 JS 控制伪类样式

虽然无法直接修改伪类（如`:hover`、`:focus`）的样式，但可以通过添加/移除类来间接控制这些样式。例如，针对鼠标悬停或点击事件，可以通过JavaScript添加对应的类，然后通过CSS定义伪类。

```javascript
let element = document.getElementById("myElement");
element.addEventListener("mouseenter", function() {
  element.classList.add("hovered");
});
element.addEventListener("mouseleave", function() {
  element.classList.remove("hovered");
});
```

在CSS中：

```css
.myElement.hovered:hover {
  background-color: yellow;  // 鼠标悬停时的样式
}
```

### 总结

JavaScript通过DOM操作能够非常灵活地改变HTML页面的CSS样式。可以通过以下几种方式进行操作：

1. **直接修改内联样式**：使用`style`属性。
2. **修改CSS类**：通过`classList`添加、删除或切换类。
3. **修改外部或内部CSS样式表**：通过操作`styleSheets`。
4. **动态加载或修改外部样式表**：通过`link`元素。
5. **控制动画和过渡效果**：通过CSS过渡和动画。
6. **通过JS控制伪类样式**：间接通过类的添加/删除来控制伪类。

这些方法可以帮助开发者在浏览器端实现动态样式调整，从而提高页面的交互性和用户体验。