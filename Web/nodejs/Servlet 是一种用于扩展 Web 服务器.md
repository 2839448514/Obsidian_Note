在 Java Web 开发中，**Servlet** 是一种用于扩展 Web 服务器功能的 Java 类，负责处理客户端请求并生成响应。Servlet 由一个或多个方法组成，每个方法都有特定的作用，用于处理请求、初始化、销毁等。以下是 **Servlet** 常用的所有方法及其作用：

### 1. **`init()` 方法**

- **作用**：初始化方法，用于 Servlet 被实例化后，进行一次性的初始化工作。
- **执行时机**：在 Servlet 被加载到内存后且实例化时调用，通常只会调用一次。
- **用途**：
    - 初始化 Servlet 的配置（如数据库连接、配置文件加载等）。
    - 进行需要在整个生命周期内只初始化一次的操作。
- **定义**：

    ```java
    public void init() throws ServletException
    ```

### 2. **`service()` 方法**

- **作用**：负责接收并处理每个请求，它会根据 HTTP 请求的方法（如 GET、POST 等）调用相应的处理方法。
- **执行时机**：每次客户端请求到达时，都会调用 `service()` 方法。
- **用途**：
    - 该方法根据请求类型（`GET`、`POST` 等）调用不同的处理方法。
    - 通过 `HttpServletRequest` 和 `HttpServletResponse` 对象来获取请求和响应数据。
- **定义**：

    ```java
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException
    ```

### 3. **`destroy()` 方法**

- **作用**：用于在 Servlet 被销毁时进行资源的清理。
- **执行时机**：当 Servlet 容器决定卸载 Servlet 时，容器会调用该方法。
- **用途**：
    - 释放 Servlet 占用的资源，如关闭数据库连接、文件流等。
    - 可以在此方法中执行一些资源的清理工作。
- **定义**：

    ```java
    public void destroy()
    ```

### 4. **`doGet()` 方法**

- **作用**：用于处理 HTTP `GET` 请求。
- **执行时机**：当客户端发送 `GET` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - 通常用于处理读取数据请求，客户端通过 URL 请求某些资源时，Servlet 使用 `doGet()` 返回数据。
    - 用于获取数据并在客户端显示，常见于表单数据的显示、页面跳转等操作。
- **定义**：

    ```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 5. **`doPost()` 方法**

- **作用**：用于处理 HTTP `POST` 请求。
- **执行时机**：当客户端发送 `POST` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - 通常用于处理客户端提交的数据，如表单提交、文件上传等。
    - 用于在客户端发送表单数据时，处理并返回相应的结果。
- **定义**：

    ```java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 6. **`doPut()` 方法**

- **作用**：用于处理 HTTP `PUT` 请求。
- **执行时机**：当客户端发送 `PUT` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - `PUT` 请求通常用于上传文件或数据的替换操作。
    - 将客户端发送的内容替换到服务器上的某个资源中，或更新现有的资源。
- **定义**：

    ```java
    protected void doPut(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 7. **`doDelete()` 方法**

- **作用**：用于处理 HTTP `DELETE` 请求。
- **执行时机**：当客户端发送 `DELETE` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - `DELETE` 请求通常用于删除服务器上的某些资源。
    - 例如，删除数据库中的记录或删除文件系统中的文件等。
- **定义**：

    ```java
    protected void doDelete(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 8. **`getServletInfo()` 方法**

- **作用**：返回 Servlet 的信息，如版本、作者等。
- **执行时机**：通常用于获取 Servlet 的元数据。
- **用途**：
    - 返回 Servlet 的信息，供容器或管理者查看。
    - 通常返回关于 Servlet 的描述、版本、作者等信息。
- **定义**：

    ```java
    public String getServletInfo()
    ```

### 9. **`getServletConfig()` 方法**

- **作用**：返回 Servlet 的配置对象，该对象包含 Servlet 的初始化参数。
- **执行时机**：每次访问 Servlet 配置时调用。
- **用途**：
    - 获取 Servlet 的初始化参数。
    - 配置文件（如 web.xml）中指定的初始化参数可以通过该方法获取。
- **定义**：

    ```java
    public ServletConfig getServletConfig()
    ```

### 10. **`log()` 方法**

- **作用**：用于记录 Servlet 中的日志信息。
- **执行时机**：在 Servlet 中执行任何需要记录的日志时调用。
- **用途**：
    - 记录调试信息、异常信息等。
    - 通常与日志系统集成。
- **定义**：

    ```java
    public void log(String msg)
    public void log(String message, Throwable throwable)
    ```

### 11. **`doHead()` 方法**

- **作用**：用于处理 HTTP `HEAD` 请求。
- **执行时机**：当客户端发送 `HEAD` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - `HEAD` 请求与 `GET` 请求相似，唯一的区别是服务器不会返回响应体，仅返回响应头。
    - 用于客户端只关心响应头信息的场景。
- **定义**：

    ```java
    protected void doHead(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 12. **`doOptions()` 方法**

- **作用**：用于处理 HTTP `OPTIONS` 请求。
- **执行时机**：当客户端发送 `OPTIONS` 请求时，Servlet 容器会调用该方法。
- **用途**：
    - `OPTIONS` 请求用于查询服务器支持的 HTTP 方法。
    - 常用于跨域请求中，客户端通过 `OPTIONS` 请求来检查服务器是否支持跨域请求。
- **定义**：

    ```java
    protected void doOptions(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    ```

### 13. **`forward()` 和 `sendRedirect()`**

这两个方法并不是直接由 `HttpServlet` 提供的，但它们常常在 Servlet 中使用，进行请求转发和重定向。

- **`forward()`**：将请求转发到另一个资源（如 JSP 或其他 Servlet）。请求信息不会丢失。

    ```java
    RequestDispatcher dispatcher = request.getRequestDispatcher("/anotherServlet");
    dispatcher.forward(request, response);
    ```

- **`sendRedirect()`**：发送 HTTP 重定向响应，客户端会发起新的请求到指定的 URL。

    ```java
    response.sendRedirect("/anotherServlet");
    ```

### 总结

Servlet 中的方法提供了处理 HTTP 请求的基础功能，每个方法都有不同的用途和生命周期。`init()` 和 `destroy()` 主要用于初始化和销毁 Servlet，`service()` 是 Servlet 处理请求的核心方法，而 `doXXX()` 方法则针对不同类型的 HTTP 请求执行特定的操作。理解这些方法的作用，有助于开发者在 Java Web 开发中灵活使用 Servlet 来处理客户端请求和响应。
