### Node.js `http` 模块总结

Node.js 的 `http` 模块是构建 HTTP 服务和客户端通信的核心模块之一。它提供了创建 HTTP 服务器、发送 HTTP 请求和处理 HTTP 响应的功能。以下是 `http` 模块的详细总结，包括其常用功能和方法。

### 1. **创建 HTTP 服务器**

`http` 模块提供了 `http.createServer()` 方法，用于创建一个 HTTP 服务器。该服务器可以处理来自客户端的请求，并返回相应的响应。

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.statusCode = 200; // 设置响应状态码
    res.setHeader('Content-Type', 'text/plain'); // 设置响应头
    res.end('Hello, World!'); // 结束响应并发送内容
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

#### 关键方法：

- `http.createServer(requestListener)`：创建 HTTP 服务器。
- `server.listen(port, host, callback)`：启动服务器，监听指定端口和地址。

### 2. **处理 HTTP 请求**

每当接收到请求时，服务器会触发回调函数。在回调函数中，可以根据请求的 URL、请求方法（GET、POST 等）、请求头等进行处理。

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.method === 'GET') {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('GET request received');
    } else if (req.method === 'POST') {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('POST request received');
    } else {
        res.statusCode = 405; // Method Not Allowed
        res.end('Method Not Allowed');
    }
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

#### 常用属性：

- `req.method`：获取请求方法（GET、POST、PUT、DELETE 等）。
- `req.url`：获取请求的 URL。
- `req.headers`：获取请求头。

#### 常用响应方法：

- `res.statusCode`：设置响应状态码。
- `res.setHeader(name, value)`：设置响应头。
- `res.end(data)`：结束响应并发送数据。

### 3. **处理请求体数据**

在发送 POST 或 PUT 请求时，数据通常会附加在请求体中。可以通过监听 `req.on('data', callback)` 事件来逐步接收请求体中的数据，并在数据接收完毕后处理。

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.method === 'POST') {
        let body = '';
        req.on('data', chunk => {
            body += chunk; // 拼接数据
        });

        req.on('end', () => {
            res.statusCode = 200;
            res.setHeader('Content-Type', 'text/plain');
            res.end(`Received data: ${body}`);
        });
    } else {
        res.statusCode = 404;
        res.end('Not a POST request');
    }
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

#### 常用事件：

- `req.on('data', callback)`：接收请求体数据。
- `req.on('end', callback)`：请求体数据接收完毕时触发。
- `req.on('error', callback)`：处理请求中的错误。

### 4. **路由处理**

虽然 `http` 模块本身不提供内置的路由功能，但可以通过检查请求的 URL 和方法来实现简单的路由功能。

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.url === '/') {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('Home Page');
    } else if (req.url === '/about') {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('About Page');
    } else {
        res.statusCode = 404;
        res.end('Page Not Found');
    }
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

### 5. **发送 HTTP 请求（客户端）**

`http` 模块还可以用作客户端发送 HTTP 请求。通过 `http.request()` 或 `http.get()` 方法，可以向服务器发送请求并处理响应。

#### 示例：发送 GET 请求

```javascript
const http = require('http');

http.get('http://www.example.com', (res) => {
    let data = '';
    
    res.on('data', chunk => {
        data += chunk; // 拼接数据
    });

    res.on('end', () => {
        console.log(data); // 输出响应内容
    });
}).on('error', (err) => {
    console.error('Request Error:', err.message); // 错误处理
});
```

#### 示例：发送 POST 请求

```javascript
const http = require('http');

const options = {
    hostname: 'www.example.com',
    port: 80,
    path: '/submit',
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Content-Length': Buffer.byteLength(data)
    }
};

const data = JSON.stringify({ name: 'John', age: 30 });

const req = http.request(options, (res) => {
    let responseData = '';

    res.on('data', (chunk) => {
        responseData += chunk;
    });

    res.on('end', () => {
        console.log('Response:', responseData);
    });
});

req.on('error', (e) => {
    console.error(`Problem with request: ${e.message}`);
});

req.write(data);
req.end();
```

#### 关键方法：

- `http.request(options, callback)`：创建 HTTP 请求。
- `http.get(url, callback)`：简化的 GET 请求方法。

### 6. **HTTPS 支持**

Node.js 提供了 `https` 模块，用于处理加密的 HTTPS 请求和响应。它是 `http` 模块的扩展，提供了 SSL/TLS 支持。

#### 示例：创建 HTTPS 服务器

```javascript
const https = require('https');
const fs = require('fs');

const options = {
    key: fs.readFileSync('private-key.pem'),
    cert: fs.readFileSync('certificate.pem')
};

const server = https.createServer(options, (req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello, HTTPS World!');
});

server.listen(3000, () => {
    console.log('Server running at https://localhost:3000/');
});
```

### 7. **常用事件**

- **`req.on('data', callback)`**：接收请求体数据。
- **`req.on('end', callback)`**：请求体数据接收完毕时触发。
- **`req.on('error', callback)`**：请求中发生错误时触发。
- **`res.on('finish', callback)`**：响应结束时触发。
- **`res.on('close', callback)`**：响应连接关闭时触发。

### 8. **其他有用的方法和属性**

- **`req.url`**：获取请求的 URL 地址。
- **`req.headers`**：获取请求头部信息。
- **`res.statusCode`**：设置响应的状态码。
- **`res.setHeader(name, value)`**：设置响应头。
- **`res.write(data)`**：分块发送响应数据。
- **`res.end(data)`**：结束响应并发送数据。

### 9. **错误处理**

`http` 模块提供了事件和回调来处理错误，确保服务器在出现错误时能够正确处理。

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World');
});

server.on('error', (err) => {
    console.error('Server Error:', err.message); // 监听错误事件
});

server.listen(3000, () => {
    console.log('Server running at http://localhost:3000/');
});
```

### 总结

- **HTTP 服务器**：通过 `http.createServer()` 创建，能够监听请求并返回响应。
- **请求和响应**：通过 `req` 对象获取请求信息，使用 `res` 对象设置响应信息。
- **请求体**：支持通过 `req.on('data')` 和 `req.on('end')` 事件处理 POST 请求的请求体。
- **客户端请求**：使用 `http.request()` 或 `http.get()` 发送 HTTP 请求并处理响应。
- **HTTPS**：提供了加密的 HTTPS 支持，保障通信安全。
- **错误处理**：通过事件机制捕获和处理请求与服务器错误。

`http` 模块是构建 HTTP 服务和处理网络请求的基础

模块，适合用来构建简单的服务端应用程序。对于更复杂的应用，可以结合第三方框架（如 Express）进一步简化开发过程。
