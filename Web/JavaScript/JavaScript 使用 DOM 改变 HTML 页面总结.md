### JavaScript 使用 DOM 改变 HTML 页面总结

在JavaScript中，通过DOM（文档对象模型）可以动态操作HTML页面的内容、结构和样式。DOM是一个编程接口，使得JavaScript能够访问和修改HTML或XML文档。

#### 1. 获取 DOM 元素

获取页面中元素的方法：

- `document.getElementById(id)`：根据ID获取元素。
- `document.getElementsByClassName(className)`：根据类名获取元素，返回HTMLCollection。
- `document.getElementsByTagName(tagName)`：根据标签名获取元素，返回HTMLCollection。
- `document.querySelector(selector)`：根据CSS选择器获取第一个匹配的元素。
- `document.querySelectorAll(selector)`：根据CSS选择器获取所有匹配的元素，返回NodeList。

#### 2. 修改元素内容

可以修改元素的HTML或文本内容：

- `innerHTML`：获取或设置元素内部的HTML内容。
    
    ```javascript
    element.innerHTML = "<p>新的内容</p>";
    ```
    
- `textContent`：获取或设置元素内部的纯文本内容。
    
    ```javascript
    element.textContent = "纯文本内容";
    ```
    

#### 3. 修改元素属性

操作元素的属性（如ID、class、src等）：

- `setAttribute(attributeName, value)`：设置属性值。
    
    ```javascript
    element.setAttribute("class", "newClass");
    ```
    
- `getAttribute(attributeName)`：获取属性值。
    
    ```javascript
    let className = element.getAttribute("class");
    ```
    
- 直接修改常见属性（如`src`、`href`、`value`）：
    
    ```javascript
    image.src = "new-image.jpg";
    ```
    

#### 4. 修改元素样式

通过`style`属性修改元素的CSS样式：

```javascript
element.style.backgroundColor = "red";  // 修改背景颜色
element.style.fontSize = "20px";        // 修改字体大小
```

#### 5. 添加、删除和修改元素

JavaScript允许动态地对DOM结构进行操作：

- **创建新元素**：
    
    ```javascript
    let newElement = document.createElement("div");
    newElement.textContent = "新创建的元素";
    document.body.appendChild(newElement);  // 添加到页面
    ```
    
- **删除元素**：
    
    ```javascript
    let element = document.getElementById("myElement");
    element.parentNode.removeChild(element);  // 删除元素
    ```
    
- **插入新元素**：
    
    ```javascript
    let newElement = document.createElement("p");
    newElement.textContent = "新插入的段落";
    let parentElement = document.getElementById("parentElement");
    parentElement.appendChild(newElement);  // 插入到父元素的最后
    ```
    
    插入到特定位置：
    
    ```javascript
    let newElement = document.createElement("p");
    newElement.textContent = "插入到某个元素之前";
    let referenceElement = document.getElementById("referenceElement");
    referenceElement.parentNode.insertBefore(newElement, referenceElement);
    ```
    

#### 6. 事件处理

绑定事件使得页面与用户交互：

- `addEventListener(event, function)`：为元素添加事件监听器。
    
    ```javascript
    button.addEventListener("click", function() {
      alert("按钮被点击了");
    });
    ```
    

#### 7. 遍历 DOM 元素

遍历获取的元素集合：

- 使用`getElementsByClassName`或`getElementsByTagName`（返回HTMLCollection）：
    
    ```javascript
    let elements = document.getElementsByClassName("myClass");
    for (let i = 0; i < elements.length; i++) {
      elements[i].style.color = "blue";
    }
    ```
    
- 使用`querySelectorAll`（返回NodeList）：
    
    ```javascript
    let elements = document.querySelectorAll(".myClass");
    elements.forEach(function(element) {
      element.style.color = "blue";
    });
    ```
    

#### 8. 操作表单元素

操作表单元素，如输入框、复选框等：

- 获取和设置输入框的值：
    
    ```javascript
    let input = document.getElementById("myInput");
    input.value = "新的输入值";  // 设置值
    let value = input.value;    // 获取值
    ```
    
- 获取复选框状态：
    
    ```javascript
    let checkbox = document.getElementById("myCheckbox");
    let isChecked = checkbox.checked;  // 获取复选框是否选中
    ```
    
- 获取选中的单选框：
    
    ```javascript
    let selectedRadio = document.querySelector('input[name="gender"]:checked');
    let value = selectedRadio.value;  // 获取选中的单选框的值
    ```
    

### 总结

JavaScript通过DOM操作可以全面控制HTML页面的内容、结构、样式和行为。常用的DOM操作包括获取元素、修改内容和属性、操作样式、添加删除元素、处理事件等。在实际开发中，DOM操作是构建动态、交互性强的网页的基础，通过合理使用DOM，可以极大地提高页面的响应性和用户体验。