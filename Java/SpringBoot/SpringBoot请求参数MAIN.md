在 Spring MVC 中处理请求参数时，可以通过多种方式获取客户端传递的参数。这些方式包括使用 `@RequestParam`、`@PathVariable`、`@ModelAttribute`、`@RequestBody` 等注解。以下是这些方式的总结：

### 1. **通过查询字符串（Query Parameters）传递参数**

使用 `@RequestParam` 注解来获取请求中的查询参数。可以处理简单的参数，也可以处理复杂类型（如日期、数字等）。如果请求中包含多个参数，可以通过命名或类型匹配进行接收。

#### 示例：基本参数（`String`、`Integer`、`Boolean`）

请求：

```
/simpleParam?name=张三&age=25
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("name") String name, @RequestParam("age") Integer age) {
    System.out.println("Received name: " + name);
    System.out.println("Received age: " + age);
    return "OK";
}
```

#### 示例：日期时间参数（`LocalDate`、`LocalDateTime`）

请求：

```
/simpleParam?date=2024-01-13
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("date") LocalDate date) {
    System.out.println("Received date: " + date);
    return "OK";
}
```

### 2. **通过路径变量（Path Variables）传递参数**

使用 `@PathVariable` 注解获取 URL 路径中的变量。这种方式适合用于 RESTful 风格的 URL 中。

#### 示例：获取路径中的参数

请求：

```
/simpleParam/张三/25
```

控制器方法：

```java
@RequestMapping("/simpleParam/{name}/{age}")
public String simpleParam(@PathVariable("name") String name, @PathVariable("age") Integer age) {
    System.out.println("Received name: " + name);
    System.out.println("Received age: " + age);
    return "OK";
}
```

### 3. **通过请求体（Request Body）传递参数**

使用 `@RequestBody` 注解来接收请求体中的 JSON 或 XML 数据。通常用于 POST 请求，这种方式可以直接接收复杂对象的参数。

#### 示例：接收 JSON 格式的请求体

请求体：

```json
{
    "name": "张三",
    "age": 25
}
```

控制器方法：

```java
@RequestMapping(value = "/simpleParam", method = RequestMethod.POST)
public String simpleParam(@RequestBody User user) {
    System.out.println("Received name: " + user.getName());
    System.out.println("Received age: " + user.getAge());
    return "OK";
}
```

### 4. **通过模型绑定（ModelAttribute）传递参数**

使用 `@ModelAttribute` 注解，Spring 会自动将请求参数绑定到 Java Bean 对象的属性上。它可以用于请求参数和表单数据的绑定。

#### 示例：通过 `@ModelAttribute` 绑定参数到对象

请求：

```
/simpleParam?name=张三&age=25
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@ModelAttribute User user) {
    System.out.println("Received name: " + user.getName());
    System.out.println("Received age: " + user.getAge());
    return "OK";
}
```

Java Bean：

```java
public class User {
    private String name;
    private Integer age;

    // Getters and Setters
}
```

### 5. **通过 `@RequestHeader` 传递 HTTP 请求头参数**

使用 `@RequestHeader` 注解从 HTTP 请求头中获取参数。常用于处理用户代理、语言等 HTTP 请求头。

#### 示例：获取请求头中的参数

请求：

```
/simpleParam
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestHeader("User-Agent") String userAgent) {
    System.out.println("Received User-Agent: " + userAgent);
    return "OK";
}
```

### 6. **通过 `@RequestParam` 处理数组和集合**

你可以使用 `@RequestParam` 来接收数组或集合类型的参数。

#### 示例：传递数组参数

请求：

```
/simpleParam?names=张三&names=李四&names=王五
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("names") String[] names) {
    for (String name : names) {
        System.out.println("Received name: " + name);
    }
    return "OK";
}
```

#### 示例：传递集合参数（`List`）

请求：

```
/simpleParam?names=张三&names=李四&names=王五
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("names") List<String> names) {
    for (String name : names) {
        System.out.println("Received name: " + name);
    }
    return "OK";
}
```

### 7. **处理 JSON 请求体**

- **`@RequestBody`** 主要用于接收 JSON 格式的请求体，可以将 JSON 自动映射到 Java 对象。

**示例**：

```java
@RequestMapping("/create")
public String create(@RequestBody Product product) {
    System.out.println("Product Name: " + product.getName());
    return "Created";
}
```

- Spring Boot 会自动处理 JSON 的序列化与反序列化（需要引入 Jackson 或其他库）。

### 7. **通过 `@RequestParam` 和 `@DateTimeFormat` 处理自定义日期格式**

如果日期格式不是标准的 ISO 格式，你可以使用 `@DateTimeFormat` 注解来指定日期格式。

#### 示例：自定义日期格式

请求：

```
/simpleParam?date=2024/01/13
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("date") @DateTimeFormat(pattern = "yyyy/MM/dd") LocalDate date) {
    System.out.println("Received date: " + date);
    return "OK";
}
```

### 8. **通过 `@RequestParam` 和 `@RequestHeader` 获取多个值**

如果查询参数或请求头中包含多个相同名称的参数，你可以将它们接收到一个数组或集合中。

#### 示例：接收多个查询参数

请求：

```
/simpleParam?colors=red&colors=green&colors=blue
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("colors") List<String> colors) {
    for (String color : colors) {
        System.out.println("Received color: " + color);
    }
    return "OK";
}
```

#### 示例：接收多个请求头

请求头：

```
User-Agent: Mozilla/5.0
User-Agent: Chrome/91.0
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestHeader("User-Agent") List<String> userAgents) {
    for (String userAgent : userAgents) {
        System.out.println("Received User-Agent: " + userAgent);
    }
    return "OK";
}
```

除了上面列出的请求参数处理方式之外，Spring MVC 还提供了一些其他方法来处理请求参数和数据。以下是一些补充的方式：

### 9. **通过 `@RequestParam` 处理默认值**

在使用 `@RequestParam` 时，如果请求中没有传递某个参数，Spring 会自动使用默认值。可以通过 `defaultValue` 属性来设置默认值。

#### 示例：提供默认值

请求：

```
/simpleParam?name=张三
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam(value = "name", defaultValue = "未知") String name) {
    System.out.println("Received name: " + name);
    return "OK";
}
```

如果没有传递 `name` 参数，`name` 会使用默认值 `"未知"`。

### 10. **通过 `@RequestParam` 处理必填参数**

如果请求中没有提供某个必填的参数，Spring 会抛出 `MissingServletRequestParameterException` 异常。可以通过 `required` 属性来指定是否为必填参数，默认为 `true`。

#### 示例：必填参数

请求：

```
/simpleParam
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam(value = "name", required = true) String name) {
    System.out.println("Received name: " + name);
    return "OK";
}
```

如果请求没有提供 `name` 参数，Spring 会抛出 `400 Bad Request` 错误。

### 11. **通过 `@RequestParam` 处理参数类型转换**

Spring 会自动进行参数类型转换。如果请求中的参数类型与控制器方法中的类型不匹配，Spring 会根据注册的类型转换器自动进行转换。例如，`String` 转换为 `Integer` 或 `LocalDate` 等。

#### 示例：类型转换

请求：

```
/simpleParam?age=25
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("age") Integer age) {
    System.out.println("Received age: " + age);
    return "OK";
}
```

在此示例中，Spring 会自动将 `age` 字符串 `"25"` 转换为 `Integer` 类型。

### 12. **通过 `@CookieValue` 获取 Cookie 参数**

Spring 提供了 `@CookieValue` 注解来从请求的 Cookies 中获取参数。它类似于 `@RequestHeader`，但是是从 Cookie 中读取数据。

#### 示例：获取 Cookie 参数

请求：

```
/simpleParam
Cookie: sessionId=12345; userName=张三
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@CookieValue("sessionId") String sessionId, @CookieValue("userName") String userName) {
    System.out.println("Received sessionId: " + sessionId);
    System.out.println("Received userName: " + userName);
    return "OK";
}
```

### 13. **通过 `@RequestParam` 和 `@Valid` 校验参数**

如果需要对请求参数进行校验，可以使用 `@Valid` 或 `@Validated` 注解结合 `@RequestParam` 或 `@ModelAttribute`。这通常与 Java Bean Validation（如 `@NotNull`、`@Size`、`@Min` 等注解）一起使用。

#### 示例：使用 Java Bean Validation 进行参数校验

请求：

```
/simpleParam?name=张三&age=25
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@Valid @ModelAttribute User user, BindingResult result) {
    if (result.hasErrors()) {
        return "Error";
    }
    System.out.println("Received name: " + user.getName());
    System.out.println("Received age: " + user.getAge());
    return "OK";
}
```

Java Bean：

```java
public class User {
    @NotNull(message = "Name cannot be null")
    private String name;

    @Min(value = 18, message = "Age must be at least 18")
    private Integer age;

    // Getters and Setters
}
```

如果请求参数不满足验证条件，Spring 会返回相应的错误信息。

### 14. **通过 `@RequestPart` 处理表单上传文件**

在处理文件上传时，可以使用 `@RequestPart` 注解来获取上传的文件。通常，文件通过 `multipart/form-data` 格式上传。

#### 示例：接收文件上传

HTML 表单：

```html
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```

控制器方法：

```java
@RequestMapping(value = "/upload", method = RequestMethod.POST)
public String upload(@RequestPart("file") MultipartFile file) throws IOException {
    System.out.println("Received file: " + file.getOriginalFilename());
    // 处理文件
    return "OK";
}
```

### 15. **通过 `@RequestParam` 接收枚举值**

如果请求参数传递的是枚举类型，Spring 可以自动将查询参数转换为枚举值。

#### 示例：接收枚举值

请求：

```
/simpleParam?status=ACTIVE
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("status") Status status) {
    System.out.println("Received status: " + status);
    return "OK";
}
```

枚举：

```java
public enum Status {
    ACTIVE,
    INACTIVE
}
```

### 16. **通过 `@RequestParam` 和 `@RequestHeader` 获取所有值**

如果请求中的参数或请求头包含多个相同名称的值，Spring 会将这些值封装到数组或集合中。

#### 示例：获取多个查询参数

请求：

```
/simpleParam?items=apple&items=banana&items=orange
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestParam("items") List<String> items) {
    for (String item : items) {
        System.out.println("Received item: " + item);
    }
    return "OK";
}
```

#### 示例：获取多个请求头

请求头：

```
User-Agent: Mozilla/5.0
User-Agent: Chrome/91.0
```

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestHeader("User-Agent") List<String> userAgents) {
    for (String userAgent : userAgents) {
        System.out.println("Received User-Agent: " + userAgent);
    }
    return "OK";
}
```

### 17. **通过 `@RequestAttribute` 获取请求中的属性**

`@RequestAttribute` 用于从请求中获取属性。请求属性是通过 `request.setAttribute()` 设置的，通常用于在过滤器或拦截器中设置。

#### 示例：获取请求属性

控制器方法：

```java
@RequestMapping("/simpleParam")
public String simpleParam(@RequestAttribute("user") User user) {
    System.out.println("Received user: " + user.getName());
    return "OK";
}
```

在过滤器或拦截器中设置请求属性：

```java
request.setAttribute("user", new User("张三", 25));
```

---

### 总结

|**方式**|**注解**|**说明**|**示例**|
|---|---|---|---|
|**查询参数**|`@RequestParam`|从查询字符串获取请求参数，支持基本类型、日期、数组等|`/simpleParam?name=张三&age=25`|
|**路径变量**|`@PathVariable`|从 URL 路径中提取参数|`/simpleParam/{name}/{age}`|
|**请求体**|`@RequestBody`|从请求体中提取数据（通常是 JSON 或 XML）|`{"name": "张三", "age": 25}`|
|**模型绑定**|`@ModelAttribute`|将请求参数绑定到 Java Bean 对象|`/simpleParam?name=张三&age=25`|
|**请求头**|`@RequestHeader`|从 HTTP 请求头中获取参数|`User-Agent: Mozilla/5.0`|
|**数组或集合**|`@RequestParam`|从查询字符串获取数组或集合类型的参数|`/simpleParam?names=张三&names=李四`|
|**自定义日期格式**|`@DateTimeFormat`|自定义日期格式|`/simpleParam?date=2024/01/13`|
|**默认值**|`@RequestParam`|提供默认值，若请求中没有传递参数时使用|`/simpleParam?name=张三`|
|**必填参数**|`@RequestParam`|指定请求参数为必填项|`/simpleParam`|
|**类型转换**|`@RequestParam`|自动进行类型转换，如 `String` 转换为 `Integer`||

或 `LocalDate` | `/simpleParam?age=25` | | **Cookie 参数** | `@CookieValue` | 从请求中的 Cookies 中提取参数 | `Cookie: sessionId=12345;` | | **校验参数** | `@Valid`/`@Validated` | 使用 Java Bean Validation 对请求参数进行校验 | `/simpleParam?name=张三&age=25` | | **表单上传文件** | `@RequestPart` | 处理 multipart 表单数据，获取上传的文件 | `<input type="file" name="file">` | | **枚举类型** | `@RequestParam` | 将查询参数转换为枚举类型 | `/simpleParam?status=ACTIVE` | | **多个相同参数** | `@RequestParam`/`@RequestHeader` | 获取多个相同名称的查询参数或请求头 | `/simpleParam?items=apple&items=banana` | | **请求属性** | `@RequestAttribute` | 获取请求中设置的属性 | `request.setAttribute("user", new User("张三", 25))` |

这些方式涵盖了 Spring MVC 中最常用的请求参数传递方法，适用于多种应用场景。你可以根据实际需求选择合适的方式来处理参数。
