### DOM（Document Object Model）操作详解

DOM（文档对象模型）是浏览器提供的一个API，允许程序访问和操作HTML文档的内容、结构和样式。JavaScript通过DOM来动态地修改网页内容。常见的DOM操作包括获取元素、修改元素、创建和删除元素、处理事件、操作样式、查询和过滤等。

#### 一、获取元素

1. **`getElementById()`**  
    根据元素的 `id` 获取单个元素。
    
    ```javascript
    const element = document.getElementById("myElement");
    ```
    
2. **`getElementsByClassName()`**  
    根据类名获取一组元素，返回的是一个“类数组”对象。
    
    ```javascript
    const elements = document.getElementsByClassName("myClass");
    ```
    
3. **`getElementsByTagName()`**  
    根据标签名获取一组元素，返回的是一个“类数组”对象。
    
    ```javascript
    const elements = document.getElementsByTagName("div");
    ```
    
4. **`querySelector()`**  
    根据CSS选择器获取第一个匹配的元素，返回单个元素。
    
    ```javascript
    const element = document.querySelector(".myClass");
    ```
    
5. **`querySelectorAll()`**  
    根据CSS选择器获取所有匹配的元素，返回一个 `NodeList` 对象（可迭代）。
    
    ```javascript
    const elements = document.querySelectorAll(".myClass");
    ```
    

---

#### 二、修改元素内容

1. **`innerHTML`**  
    获取或设置元素的HTML内容。
    
    ```javascript
    // 获取内容
    const content = element.innerHTML;
    // 设置内容
    element.innerHTML = "<p>新内容</p>";
    ```
    
2. **`innerText`**  
    获取或设置元素的文本内容，忽略HTML标签。
    
    ```javascript
    // 获取文本
    const text = element.innerText;
    // 设置文本
    element.innerText = "新文本";
    ```
    
3. **`textContent`**  
    获取或设置元素的文本内容，类似于 `innerText`，但不受CSS样式影响。
    
    ```javascript
    // 获取文本
    const text = element.textContent;
    // 设置文本
    element.textContent = "新文本内容";
    ```
    
4. **`outerHTML`**  
    获取或设置元素及其子元素的HTML内容。
    
    ```javascript
    const outerContent = element.outerHTML;
    element.outerHTML = "<div>新内容</div>";
    ```
    

---

#### 三、修改元素属性

1. **`setAttribute()`**  
    设置指定元素的属性值。
    
    ```javascript
    element.setAttribute("src", "image.jpg");
    ```
    
2. **`getAttribute()`**  
    获取指定元素的属性值。
    
    ```javascript
    const src = element.getAttribute("src");
    ```
    
3. **`removeAttribute()`**  
    删除指定元素的属性。
    
    ```javascript
    element.removeAttribute("src");
    ```
    
4. **`classList`**  
    操作元素的类名（添加、删除、切换类）。
    
    ```javascript
    element.classList.add("new-class"); // 添加类
    element.classList.remove("old-class"); // 删除类
    element.classList.toggle("active"); // 切换类
    ```
    
5. **`id`**  
    获取或设置元素的 `id`。
    
    ```javascript
    const id = element.id;  // 获取 id
    element.id = "newId";   // 设置 id
    ```
    
6. **`style`**  
    直接修改元素的内联样式。
    
    ```javascript
    element.style.backgroundColor = "red"; // 设置背景颜色
    element.style.fontSize = "20px";       // 设置字体大小
    ```
    

---

#### 四、创建和插入元素

1. **`createElement()`**  
    创建新的HTML元素节点。
    
    ```javascript
    const newDiv = document.createElement("div");
    ```
    
2. **`createTextNode()`**  
    创建文本节点。
    
    ```javascript
    const textNode = document.createTextNode("新文本");
    ```
    
3. **`appendChild()`**  
    将一个元素节点添加到父节点的子节点列表末尾。
    
    ```javascript
    parentElement.appendChild(newDiv);
    ```
    
4. **`insertBefore()`**  
    在指定元素之前插入一个新的子元素。
    
    ```javascript
    parentElement.insertBefore(newDiv, referenceNode);
    ```
    
5. **`replaceChild()`**  
    用新的子元素替换旧的子元素。
    
    ```javascript
    parentElement.replaceChild(newDiv, oldDiv);
    ```
    

---

#### 五、删除元素

1. **`removeChild()`**  
    从父元素中删除一个子元素。
    
    ```javascript
    parentElement.removeChild(childElement);
    ```
    
2. **`remove()`**  
    删除当前元素本身。
    
    ```javascript
    element.remove();
    ```
    

---

#### 六、操作样式

1. **`style`**  
    修改元素的行内样式。
    
    ```javascript
    element.style.color = "blue";  // 设置字体颜色
    element.style.fontSize = "18px";  // 设置字体大小
    ```
    
2. **`getComputedStyle()`**  
    获取元素的计算样式，返回一个 `CSSStyleDeclaration` 对象。
    
    ```javascript
    const style = window.getComputedStyle(element);
    const color = style.color;  // 获取元素的颜色
    ```
    
3. **`classList`**  
    操作元素的类名，如添加、删除、切换类。
    
    ```javascript
    element.classList.add("new-class");  // 添加类
    element.classList.remove("old-class");  // 删除类
    element.classList.toggle("active");  // 切换类
    ```
    

---

#### 七、事件处理

1. **`addEventListener()`**  
    为指定元素添加事件监听器，支持多次添加相同类型的事件。
    
    ```javascript
    element.addEventListener("click", function() {
      alert("Clicked!");
    });
    ```
    
2. **`removeEventListener()`**  
    移除指定元素的事件监听器。
    
    ```javascript
    element.removeEventListener("click", handler);
    ```
    
3. **`onclick`**  
    为元素设置点击事件（不推荐，使用 `addEventListener()` 更灵活）。
    
    ```javascript
    element.onclick = function() {
      alert("Clicked!");
    };
    ```
    
4. **`event.preventDefault()`**  
    阻止事件的默认行为。
    
    ```javascript
    element.addEventListener("submit", function(event) {
      event.preventDefault();  // 阻止表单提交
    });
    ```
    
5. **`event.stopPropagation()`**  
    阻止事件的传播。
    
    ```javascript
    element.addEventListener("click", function(event) {
      event.stopPropagation();  // 阻止事件冒泡
    });
    ```
    

---

#### 八、获取元素的位置信息

1. **`getBoundingClientRect()`**  
    获取元素的位置信息（相对于视口的矩形区域）。
    
    ```javascript
    const rect = element.getBoundingClientRect();
    console.log(rect.top, rect.left, rect.width, rect.height);
    ```
    
2. **`offsetTop`, `offsetLeft`, `offsetWidth`, `offsetHeight`**  
    获取元素的偏移量（相对于父元素的位置）。
    
    ```javascript
    const top = element.offsetTop;
    const left = element.offsetLeft;
    const width = element.offsetWidth;
    const height = element.offsetHeight;
    ```
    

---

#### 九、遍历子元素

1. **`children`**  
    获取元素的所有子元素节点（不包括文本节点等非元素节点）。
    
    ```javascript
    const childElements = element.children;
    ```
    
2. **`childNodes`**  
    获取元素的所有子节点（包括文本节点、注释节点等）。
    
    ```javascript
    const allChildNodes = element.childNodes;
    ```
    
3. **`firstChild`, `lastChild`, `nextSibling`, `previousSibling`**  
    获取元素的第一个子节点、最后一个子节点、下一个兄弟节点、上一个兄弟节点。
    
    ```javascript
    const firstChild = element.firstChild;
    const lastChild = element.lastChild;
    const nextSibling = element.nextSibling;
    const prevSibling = element.previousSibling;
    ```
    

---

#### 十、查询与过滤

1. **`matches()`**  
    判断元素是否匹配指定的CSS选择器。
    
    ```javascript
    if (element.matches(".myClass")) {
      console.log("Matched!");
    }
    ```
    
2. **`closest()`**  
    获取元素及其父元素中，匹配指定选择器的第一个元素。
    
    ```javascript
    const closestElement = element.closest(".myClass");
    ```
    
3. **`querySelectorAll()`**
    

根据CSS选择器获取所有匹配的元素，返回一个 `NodeList` 对象。

```javascript
const elements = document.querySelectorAll(".myClass");
```

4. **`find()`**  
    在当前元素中查找符合条件的元素。
    
    ```javascript
    const childElement = element.querySelector(".child");
    ```
    

---

### 总结

|操作类型|方法/属性|描述|
|---|---|---|
|获取元素|`getElementById()`, `getElementsByClassName()`, `querySelector()`|获取页面上的元素|
|修改内容|`innerHTML`, `innerText`, `textContent`, `outerHTML`|修改元素的内容|
|修改属性|`setAttribute()`, `getAttribute()`, `removeAttribute()`, `classList`|修改元素的属性和类名|
|创建/插入元素|`createElement()`, `createTextNode()`, `appendChild()`, `insertBefore()`, `replaceChild()`|创建和插入新元素|
|删除元素|`removeChild()`, `remove()`|删除元素|
|样式修改|`style`, `getComputedStyle()`, `classList`|修改元素的样式|
|事件处理|`addEventListener()`, `removeEventListener()`, `onclick`, `event.preventDefault()`, `event.stopPropagation()`|添加和移除事件监听器|
|位置信息|`getBoundingClientRect()`, `offsetTop`, `offsetLeft`, `offsetWidth`, `offsetHeight`|获取元素的位置和尺寸|
|子元素遍历|`children`, `childNodes`, `firstChild`, `lastChild`, `nextSibling`, `previousSibling`|遍历元素的子元素|
|查询与过滤|`matches()`, `closest()`, `querySelectorAll()`, `find()`|查找和过滤元素|

通过这些常见的DOM操作，你可以高效地访问、修改、创建、删除网页元素，从而实现动态网页交互。