`Express` 是一个广泛使用的 Node.js web 应用框架，它提供了一些简化开发 web 应用和 API 的功能。以下是 `Express` 的常见方法和概念的总结。

### 1. **创建 Express 应用**

```javascript
const express = require('express');
const app = express();
```

- `express()`：创建一个新的 Express 应用实例。应用实例是一个函数，处理请求和响应。

### 2. **路由**

路由用于定义应用响应请求的路径。

#### 2.1 定义路由

```javascript
app.get('/path', (req, res) => {
  res.send('Hello World!');
});
```

- `app.get(path, callback)`：定义一个 GET 请求路由。
- `app.post(path, callback)`：定义一个 POST 请求路由。
- `app.put(path, callback)`：定义一个 PUT 请求路由。
- `app.delete(path, callback)`：定义一个 DELETE 请求路由。

#### 2.2 路由处理器

路由处理器的回调函数接受两个参数：`req` 和 `res`，分别表示请求和响应对象。

```javascript
app.get('/hello', (req, res) => {
  res.send('Hello World');
});
```

- `req`: 请求对象，包含请求的详细信息（如查询参数、请求体等）。
- `res`: 响应对象，用于设置响应数据。

### 3. **请求对象（`req`）**

`req` 对象包含关于 HTTP 请求的信息。

#### 3.1 请求路径

```javascript
app.get('/user/:id', (req, res) => {
  const userId = req.params.id; // 获取动态路径参数
  res.send(`User ID: ${userId}`);
});
```

- `req.params`：获取 URL 路径中的动态参数。

#### 3.2 查询参数

```javascript
app.get('/search', (req, res) => {
  const query = req.query.q; // 获取查询参数
  res.send(`Search query: ${query}`);
});
```

- `req.query`：获取 URL 查询字符串中的参数。

#### 3.3 请求体数据（用于 POST 请求）

```javascript
app.post('/user', express.json(), (req, res) => {
  const user = req.body; // 获取 POST 请求中的 JSON 数据
  res.send(`User created: ${JSON.stringify(user)}`);
});
```

- `req.body`：获取 POST 请求的正文内容。需要通过 `express.json()` 或 `express.urlencoded()` 等中间件来解析请求体。

### 4. **响应对象（`res`）**

`res` 对象用于设置响应的数据、状态码等。

#### 4.1 发送响应数据

```javascript
res.send('Hello World'); // 发送文本响应
res.json({ message: 'Hello World' }); // 发送 JSON 响应
res.sendFile('/path/to/file'); // 发送文件
```

- `res.send()`：发送文本、HTML 或 JSON 数据。
- `res.json()`：发送 JSON 格式的数据。
- `res.sendFile()`：发送一个文件作为响应。

#### 4.2 设置响应状态码

```javascript
res.status(404).send('Not Found'); // 设置状态码为 404 并发送响应
```

- `res.status(code)`：设置响应的 HTTP 状态码。

#### 4.3 重定向

```javascript
res.redirect('/new-path'); // 重定向到另一个路径
```

- `res.redirect(url)`：执行 HTTP 重定向。

### 5. **中间件**

中间件是 Express 应用中处理请求和响应的函数。它可以在请求到达路由处理函数之前或之后执行。

#### 5.1 使用中间件

```javascript
app.use((req, res, next) => {
  console.log('Middleware executed');
  next(); // 继续处理请求
});
```

- `app.use()`：应用一个中间件，处理所有的请求。
- `next()`：调用下一个中间件或路由处理函数。

#### 5.2 内置中间件

- `express.json()`：解析请求体中的 JSON 数据。
- `express.urlencoded()`：解析请求体中的 URL 编码数据。

**示例**：

```javascript
app.use(express.json()); // 解析 JSON 数据
app.use(express.urlencoded({ extended: true })); // 解析 URL 编码的数据
```

#### 5.3 错误处理中间件

错误处理中间件需要接受四个参数：`err`, `req`, `res`, `next`。

```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something went wrong!');
});
```

### 6. **路由器（Router）**

Express 提供了 `Router` 对象，用于将不同的路由组织到不同的模块中。

```javascript
const router = express.Router();

router.get('/', (req, res) => {
  res.send('Router base path');
});

app.use('/router', router); // 将 router 挂载到 /router 路径下
```

### 7. **静态文件**

Express 可以提供静态文件，如 HTML 文件、CSS、图片等。

```javascript
app.use(express.static('public'));
```

- `express.static()`：指定静态文件的目录，应用将自动为这些文件提供服务。

### 8. **启动服务器**

```javascript
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

- `app.listen(port, callback)`：启动 Express 应用，监听指定端口。

### 9. **路由参数和查询字符串**

- **路由参数**：`/user/:id` 用于获取动态路径参数。

    ```javascript
    app.get('/user/:id', (req, res) => {
      res.send(`User ID: ${req.params.id}`);
    });
    ```

- **查询字符串**：`/search?q=keyword` 用于获取查询参数。

    ```javascript
    app.get('/search', (req, res) => {
      res.send(`Search query: ${req.query.q}`);
    });
    ```

### 10. **请求的生命周期**

在 Express 中，请求经历的生命周期包括：

1. **接收请求**：浏览器或客户端发起请求。
2. **路由匹配**：Express 根据请求的 URL 匹配合适的路由。
3. **中间件处理**：每个请求可以通过多个中间件处理。
4. **返回响应**：最终响应通过 `res` 返回给客户端。

### 总结

Express 是一个轻量且灵活的框架，提供了丰富的工具来处理 HTTP 请求和响应。通过其路由、请求对象、响应对象、中间件等功能，开发者可以快速构建 web 应用和 API。常见的操作包括创建路由、使用中间件、处理请求数据、发送响应以及提供静态文件。
