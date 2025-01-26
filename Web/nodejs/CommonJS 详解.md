### CommonJS 详解

**CommonJS** 是一种模块化规范，它的目的是为 JavaScript 提供一种标准的方式来组织和共享代码。它被广泛应用于 Node.js 中，并且在许多服务器端 JavaScript 环境中使用。CommonJS 规范定义了模块的加载、导出和引用机制。

### CommonJS 的关键概念

#### 1. **模块导出 (module.exports 和 exports)**

在 CommonJS 中，每个文件被认为是一个独立的模块。模块可以通过 `module.exports` 或 `exports` 对象来导出功能，以便其他模块使用。

- **`module.exports`** 是一个对象，用来暴露模块的功能，供外部模块引用。可以将一个函数、对象、类等赋给 `module.exports`，这样外部模块就可以使用它。
    
- **`exports`** 是 `module.exports` 的快捷方式。一般情况下，使用 `exports` 来导出一个模块的多个成员。
    

**示例：**

```javascript
// 导出模块
module.exports = function greet(name) {
  console.log('Hello, ' + name);
};

// 或者使用 exports
exports.greet = function(name) {
  console.log('Hello, ' + name);
};
```

#### 2. **模块导入 (require)**

CommonJS 使用 `require` 语法来引入模块。`require` 是一个同步方法，加载模块并返回 `module.exports` 的内容。

**示例：**

```javascript
// 引入模块
const greet = require('./greet');

// 使用导出的功能
greet('Alice');
```

#### 3. **同步加载**

CommonJS 模块的加载是同步的，这意味着 `require` 会阻塞代码执行，直到模块完全加载完毕。这对于服务器端应用来说是合适的，因为服务器端通常只需加载一次模块。

#### 4. **缓存机制**

CommonJS 模块在第一次 `require` 时会被加载并执行，并且会缓存模块的导出结果。之后再 `require` 相同的模块时，Node.js 会直接返回缓存的结果，而不会重新加载和执行模块。

**示例：**

```javascript
// greet.js
console.log('Module is being loaded');
module.exports = function greet(name) {
  console.log('Hello, ' + name);
};

// app.js
const greet1 = require('./greet');  // 输出: 'Module is being loaded'
const greet2 = require('./greet');  // 不会再次输出
greet1('Alice');
greet2('Bob');
```

在上面的例子中，`greet` 模块只会加载一次，第二次引入时，直接使用缓存的结果，不会再次执行模块内部的代码。

#### 5. **模块路径解析**

当你使用 `require` 引入模块时，Node.js 会根据以下规则来解析模块路径：

- **相对路径**：如果模块路径以 `./` 或 `../` 开头，Node.js 会按照相对路径进行查找。
    
    ```javascript
    const greet = require('./greet'); // 引入当前目录的 greet.js 模块
    ```
    
- **绝对路径**：如果模块路径是一个完整的文件路径，Node.js 会直接使用该路径。
    
    ```javascript
    const greet = require('/path/to/greet'); // 引入绝对路径的模块
    ```
    
- **模块名**：如果没有 `./` 或 `../`，Node.js 会在 `node_modules` 目录中查找该模块。
    
    ```javascript
    const express = require('express'); // 引入 node_modules 中的 express 模块
    ```
    

#### 6. **模块的作用域**

每个模块都有自己的作用域。在模块内部定义的变量和函数不会泄漏到全局作用域。它们只能在模块内部访问，除非通过 `module.exports` 或 `exports` 显式导出。

```javascript
// greet.js
let message = 'Hello, world!';
module.exports.greet = function() {
  console.log(message);
};

// app.js
const greet = require('./greet');
greet.greet();  // 输出: Hello, world!
console.log(message);  // 报错: message is not defined
```

#### 7. **使用 `__dirname` 和 `__filename`**

- **`__dirname`**：表示当前模块文件所在的目录的绝对路径。
- **`__filename`**：表示当前模块文件的绝对路径。

这两个特殊的全局变量可以用来获取文件系统中的路径信息。

**示例：**

```javascript
console.log(__dirname);  // 输出当前模块的目录路径
console.log(__filename); // 输出当前模块的完整路径
```

#### 8. **CommonJS 与 ES6 模块化的比较**

ES6（ECMAScript 6）引入了新的模块语法 (`import` 和 `export`)，与 CommonJS 的 `require` 和 `module.exports` 有一定的不同。

- **CommonJS** 是同步加载的，通常用于服务器端（如 Node.js）。
- **ES6 模块** 是静态分析的，支持更强的优化，并且支持 `import` 和 `export` 语法，通常用于浏览器端。

在现代的 JavaScript 环境中，Node.js 支持 ES6 模块语法（通过 `.mjs` 文件或启用 `"type": "module"` 配置）。不过，`require` 和 `module.exports` 仍然是 Node.js 中的默认模块化方式。

### 总结

- **CommonJS** 是 JavaScript 模块化的标准，主要用于 Node.js 中。它通过 `module.exports` 和 `require` 实现模块的导出和导入。
- **模块的加载** 是同步的，且首次加载后会缓存模块。
- **作用域**：每个模块都有自己的作用域，避免了变量污染全局作用域。
- **路径解析**：支持相对路径、绝对路径以及 `node_modules` 中的模块查找。
- **与 ES6 模块的比较**：ES6 模块支持更强的优化和静态分析，但 CommonJS 在 Node.js 环境中仍然非常常用。

通过理解这些基本概念，你可以掌握 CommonJS 模块化规范，并能够在 Node.js 中高效地组织和共享代码。