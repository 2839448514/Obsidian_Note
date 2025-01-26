### Servlet 详解

`Servlet` 是 Java Web 编程中的核心组件之一，作为 Java EE（现 Jakarta EE）规范的一部分，用于扩展服务器的功能。它是在 Web 服务器和客户端（通常是浏览器）之间处理请求和响应的组件。Servlet 可以动态地生成响应内容，例如 HTML 页面、JSON 数据等，通常用于实现 Web 应用程序的服务器端逻辑。

#### 1. **什么是 Servlet**

`Servlet` 是一种基于 Java 编写的服务器端程序，能够接收并处理客户端的请求（通常是 HTTP 请求）。它与传统的 CGI（通用网关接口）程序相比，性能更高，并且能更好地与服务器和容器集成。

##### **Servlet 的工作原理**

1. **客户端请求**：浏览器向服务器发送 HTTP 请求。
2. **请求分发**：Web 容器根据请求的 URL 查找并调用对应的 Servlet。
3. **Servlet 处理请求**：Servlet 接收请求并进行处理，执行业务逻辑，可能会访问数据库或其他服务。
4. **响应生成**：Servlet 生成响应（通常是 HTML 页面或 JSON 数据）。
5. **返回响应**：Servlet 将响应返回给客户端。

#### 2. **Servlet 生命周期**

Servlet 生命周期包括从被加载到被销毁的整个过程。生命周期由 Servlet 容器（如 Tomcat）管理，开发者只需实现 Servlet 的核心方法。

##### **生命周期步骤**

1. **加载与实例化**：当一个 Servlet 被首次请求时，容器会根据配置加载并实例化 Servlet 类。
2. **初始化**：容器调用 `init()` 方法进行 Servlet 初始化。该方法只会执行一次，通常用来执行一次性操作，如数据库连接等。
3. **请求处理**：每当客户端发送请求时，容器调用 `service()` 方法处理请求。`service()` 方法通常根据请求的 HTTP 方法（如 `GET`、`POST`）调用相应的 `doGet()`、`doPost()` 等方法。
4. **销毁**：当容器停止或者 Servlet 被卸载时，容器调用 `destroy()` 方法销毁 Servlet 实例，用于清理资源，如关闭数据库连接。

#### 3. **Servlet 核心方法详解**

##### **`init()`**

- **作用**：Servlet 初始化方法，当 Servlet 被加载时，容器会调用此方法进行初始化。通常用于执行 Servlet 初始化任务，例如数据库连接、文件读取等。
- **调用时机**：容器加载 Servlet 时调用。
- **参数**：`ServletConfig`，包含 Servlet 初始化参数。
- **方法签名**：

	```java
    public void init(ServletConfig config) throws ServletException;
    ```

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
        // 初始化操作，如加载配置文件或数据库连接
    }
}
```

##### **`service()`**

- **作用**：`service()` 是每次请求时调用的核心方法。它负责处理客户端的请求，并根据 HTTP 请求类型（如 `GET`、`POST`）将请求转发到相应的 `doGet()`、`doPost()` 等方法。
- **调用时机**：每次请求到达 Servlet 时，容器会调用此方法。
- **方法签名**：

	```java
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException;
    ```

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        // 实际处理请求的逻辑
        doGet(req, res); // 默认调用 doGet()
    }
}
```

##### **`doGet()` 和 `doPost()`**

- **作用**：`doGet()` 和 `doPost()` 是用于处理特定 HTTP 请求的方法。当请求方法为 `GET` 时，容器调用 `doGet()`；当请求方法为 `POST` 时，容器调用 `doPost()`。
- **调用时机**：当客户端发送对应的 HTTP 请求（`GET` 或 `POST`）时，容器会调用相应的 `doGet()` 或 `doPost()`。
- **方法签名**：

	```java
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException;
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException;
    ```

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 处理 GET 请求
        resp.getWriter().println("Hello from GET!");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 处理 POST 请求
        resp.getWriter().println("Hello from POST!");
    }
}
```

##### **`destroy()`**

- **作用**：当 Servlet 被销毁时调用，用于释放资源，如关闭数据库连接、清理缓存等。
- **调用时机**：当容器停止时，或 Servlet 被卸载时，调用此方法。
- **方法签名**：

	```java
    public void destroy();
    ```

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    public void destroy() {
        // 销毁操作，如关闭数据库连接
    }
}
```

#### 4. **ServletConfig 和 ServletContext**

##### **`ServletConfig`**

- **作用**：`ServletConfig` 是 Servlet 的配置对象，提供 Servlet 初始化时的配置信息（如初始化参数）。
- **方法**：
	- `getInitParameter(String name)`：获取 Servlet 初始化参数。
	- `getServletContext()`：获取 `ServletContext`，提供关于整个 Web 应用的信息。

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
        String dbUrl = config.getInitParameter("dbUrl");
    }
}
```

##### **`ServletContext`**

- **作用**：`ServletContext` 是 Web 应用的上下文对象，代表整个 Web 应用的环境。它允许不同的 Servlet 之间共享数据。
- **方法**：
	- `getAttribute(String name)`：获取应用级别的属性。
	- `setAttribute(String name, Object object)`：设置应用级别的属性。
	- `getRealPath(String path)`：获取给定虚拟路径的实际路径。

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = getServletContext();
        String realPath = context.getRealPath("/WEB-INF/config.xml");
    }
}
```

#### 5. **请求与响应对象**

##### **`HttpServletRequest`**

- **作用**：`HttpServletRequest` 提供请求信息，如请求参数、请求头、请求方式等。
- **常用方法**：
	- `getParameter(String name)`：获取请求参数。
	- `getHeader(String name)`：获取请求头。
	- `getSession()`：获取当前会话。

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String user = req.getParameter("username");
    }
}
```

##### **`HttpServletResponse`**

- **作用**：`HttpServletResponse` 用于构建响应信息，如响应数据、响应状态码等。
- **常用方法**：
	- `setStatus(int statusCode)`：设置响应状态码。
	- `setContentType(String type)`：设置响应内容类型（如 `text/html`）。
	- `getWriter()`：获取响应输出流，向客户端写入数据。

**示例**：

```java
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        PrintWriter out = resp.getWriter();
        out.println("<h1>Hello World</h1>");
    }
}
```

#### 6. **常见 Servlet 类型**

- **HttpServlet**：处理 HTTP 请求（`GET`、`POST` 等）。绝大多数 Web 应用都使用此类进行开发。
- **GenericServlet**：处理通用协议请求（不只是 HTTP）。通常用于低级别的网络服务，但不常用。

### 总结

`Servlet` 是 Java Web 编程中的核心组件，提供了一个处理 HTTP 请求和响应的强大工具。通过实现 `init()`、`doGet()`、`doPost()`、`destroy()` 等方法，开发者可以处理各种 Web 请求并返回响应。Servlet 生命周期由容器管理，包括初始化、请求处理和销毁三个主要阶段。理解和掌握 Servlet 的生命周期及其方法是开发 Java Web 应用的基础。
