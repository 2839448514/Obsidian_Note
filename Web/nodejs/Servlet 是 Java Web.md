Servlet 是 Java Web 开发中的关键组件之一，它的主要职责是处理 HTTP 请求和生成响应。Servlet 提供了许多方法来处理请求、响应以及管理生命周期。以下是对 Servlet 类（特别是 `HttpServlet` 类）的所有常见方法的详细总结。

### 1. **`init()` 方法**

- **定义**：`init()` 方法在 Servlet 被加载时由容器调用，用于初始化 Servlet。该方法只会在 Servlet 生命周期中调用一次。
- **作用**：通常用于执行一次性的初始化操作，如数据库连接、配置读取等。
- **签名**：

    ```java
    public void init(ServletConfig config) throws ServletException
    ```

- **说明**：`ServletConfig` 对象提供了 Servlet 配置信息。

#### 示例：

```java
public void init(ServletConfig config) throws ServletException {
    // 一些初始化操作
    System.out.println("Servlet initialized");
}
```

---

### 2. **`service()` 方法**

- **定义**：`service()` 方法是每次请求都要调用的方法。它负责处理请求并生成响应。`service()` 方法根据 HTTP 请求的方法（如 GET、POST）进一步调用对应的处理方法。
- **作用**：`service()` 方法是 Servlet 的核心方法，根据不同的 HTTP 方法调用相应的 `doXxx()` 方法。
- **签名**：

    ```java
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException
    ```

- **说明**：`ServletRequest` 和 `ServletResponse` 提供了请求和响应的数据和行为。

#### 示例：

```java
public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
    HttpServletRequest req = (HttpServletRequest) request;
    HttpServletResponse resp = (HttpServletResponse) response;
    
    if ("GET".equals(req.getMethod())) {
        doGet(req, resp);
    } else if ("POST".equals(req.getMethod())) {
        doPost(req, resp);
    }
}
```

---

### 3. **`doGet()` 方法**

- **定义**：`doGet()` 方法用于处理 HTTP GET 请求。GET 请求通常用于获取数据。
- **作用**：当客户端发送 GET 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

- **说明**：`HttpServletRequest` 包含请求数据，`HttpServletResponse` 用于设置响应数据。

#### 示例：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    out.println("<html><body><h1>Hello, World!</h1></body></html>");
}
```

---

### 4. **`doPost()` 方法**

- **定义**：`doPost()` 方法用于处理 HTTP POST 请求。POST 请求通常用于提交数据。
- **作用**：当客户端发送 POST 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

- **说明**：POST 请求用于提交表单或进行其他类型的数据传输。

#### 示例：

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String username = request.getParameter("username");
    response.getWriter().println("Hello, " + username);
}
```

---

### 5. **`doPut()` 方法**

- **定义**：`doPut()` 方法用于处理 HTTP PUT 请求。PUT 请求通常用于更新资源。
- **作用**：当客户端发送 PUT 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doPut(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

- **说明**：PUT 请求用于提交数据以更新指定资源。

---

### 6. **`doDelete()` 方法**

- **定义**：`doDelete()` 方法用于处理 HTTP DELETE 请求。DELETE 请求通常用于删除资源。
- **作用**：当客户端发送 DELETE 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doDelete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

- **说明**：DELETE 请求通常用于删除服务器上的资源。

---

### 7. **`doHead()` 方法**

- **定义**：`doHead()` 方法用于处理 HTTP HEAD 请求。HEAD 请求与 GET 请求类似，但是不会返回响应体，只返回响应头。
- **作用**：当客户端发送 HEAD 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doHead(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

---

### 8. **`doOptions()` 方法**

- **定义**：`doOptions()` 方法用于处理 HTTP OPTIONS 请求。OPTIONS 请求允许客户端获取服务器支持的 HTTP 方法。
- **作用**：当客户端发送 OPTIONS 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doOptions(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

---

### 9. **`doTrace()` 方法**

- **定义**：`doTrace()` 方法用于处理 HTTP TRACE 请求。TRACE 请求用于回显服务器收到的请求，通常用于诊断工具。
- **作用**：当客户端发送 TRACE 请求时，容器调用此方法处理请求。
- **签名**：

    ```java
    protected void doTrace(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

---

### 10. **`destroy()` 方法**

- **定义**：`destroy()` 方法在 Servlet 被销毁时由容器调用。该方法只会调用一次，用于释放资源。
- **作用**：通常用于执行清理操作，如关闭数据库连接、释放文件句柄等。
- **签名**：

    ```java
    public void destroy()
    ```

#### 示例：

```java
public void destroy() {
    // 清理操作
    System.out.println("Servlet destroyed");
}
```

---

### 11. **`getServletConfig()` 方法**

- **定义**：`getServletConfig()` 方法用于获取当前 Servlet 的配置对象。`ServletConfig` 包含 Servlet 的初始化参数。
- **作用**：通常用于访问初始化参数或 Servlet 的配置信息。
- **签名**：

    ```java
    public ServletConfig getServletConfig()
    ```

---

### 12. **`getServletContext()` 方法**

- **定义**：`getServletContext()` 方法用于获取 Servlet 容器的上下文对象。`ServletContext` 提供了 Web 应用级别的配置信息。
- **作用**：可以通过 `ServletContext` 访问 Web 应用的共享资源、获取 Web 应用的信息等。
- **签名**：

    ```java
    public ServletContext getServletContext()
    ```

---

### 13. **`log()` 方法**

- **定义**：`log()` 方法用于在 Servlet 中记录日志信息。
- **作用**：用于向容器的日志文件中输出调试信息或错误信息。
- **签名**：

    ```java
    public void log(String msg)
    ```

---

### 14. **`getInitParameter()` 方法**

- **定义**：`getInitParameter()` 方法用于获取 Servlet 配置中的初始化参数。
- **作用**：从 `web.xml` 配置文件中获取初始化参数。
- **签名**：

    ```java
    public String getInitParameter(String name)
    ```

---

### 15. **`getServletInfo()` 方法**

- **定义**：`getServletInfo()` 方法用于获取 Servlet 的描述信息。
- **作用**：通常返回 Servlet 的一些元数据，如版本信息等。
- **签名**：

    ```java
    public String getServletInfo()
    ```

---

### 总结

上述是 Servlet 类中最常见的方法，它们分别处理 HTTP 请求的不同类型（如 GET、POST、PUT、DELETE 等）以及 Servlet 生命周期中的初始化、销毁等操作。在实际开发中，开发者可以根据需求重写相应的方法，以实现业务逻辑。同时，Servlet 提供了多种获取请求、响应数据的方法，帮助开发者高效处理 Web 请求。
