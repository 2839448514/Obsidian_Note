### 1. **核心注解**

#### 1.1 `@SpringBootApplication`

- **作用**：这是 Spring Boot 应用的核心注解，通常位于主类上，结合了 `@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan` 注解。
- **功能**：
    - `@Configuration`：标记该类为 Spring 配置类，相当于 `applicationContext.xml`。
    - `@EnableAutoConfiguration`：启用 Spring Boot 的自动配置机制。
    - `@ComponentScan`：启用组件扫描，Spring 会扫描当前包及其子包下的组件（如 `@Component`、`@Service`、`@Repository` 等注解标记的类）。
- **示例**：
    
    ```java
    @SpringBootApplication
    public class MyApplication {
        public static void main(String[] args) {
            SpringApplication.run(MyApplication.class, args);
        }
    }
    ```
    

#### 1.2 `@Configuration`

- **作用**：用于标记该类为配置类，Spring 会从中读取配置并注册 Bean。可以替代传统的 `applicationContext.xml` 配置文件。
- **示例**：
    
    ```java
    @Configuration
    public class AppConfig {
        @Bean
        public MyService myService() {
            return new MyService();
        }
    }
    ```
    

#### 1.3 `@EnableAutoConfiguration`

- **作用**：启用 Spring Boot 的自动配置功能。它会根据项目的依赖自动配置 Spring 应用所需的相关组件（如数据库连接池、消息中间件、Web 配置等）。
- **通常使用场景**：通常不需要显式使用此注解，因为它已经包含在 `@SpringBootApplication` 中。

### 2. **控制器与请求映射注解**

#### 2.1 `@RestController`

- **作用**：标记一个类为 RESTful 控制器，类中的所有方法默认返回 JSON 或其他数据格式（而不是视图）。
- **功能**：该注解是 `@Controller` 和 `@ResponseBody` 的组合。`@ResponseBody` 会将方法的返回值作为响应体返回，`@Controller` 用于定义该类为 Spring 控制器。
- **示例**：
    
    ```java
    @RestController
    @RequestMapping("/api")
    public class MyController {
        @GetMapping("/hello")
        public String hello() {
            return "Hello, World!";
        }
    }
    ```
    

#### 2.2 `@Controller`

- **作用**：用于标记一个类为控制器类，通常与 `@RequestMapping` 等映射注解一起使用，处理客户端请求并返回视图。
- **示例**：
    
    ```java
    @Controller
    public class MyController {
        @RequestMapping("/home")
        public String home(Model model) {
            model.addAttribute("message", "Hello, Spring!");
            return "home";
        }
    }
    ```
    

#### 2.3 `@RequestMapping`

- **作用**：用于映射 HTTP 请求到相应的处理方法。
- **功能**：可以指定 URL 路径、HTTP 方法、请求参数等。
- **示例**：
    
    ```java
    @RequestMapping("/hello")
    public String sayHello() {
        return "Hello, Spring!";
    }
    ```
    

#### 2.4 `@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`

- **作用**：这四个注解是 `@RequestMapping` 的快捷方式，分别用于处理 GET、POST、PUT 和 DELETE 请求。
- **示例**：
    
    ```java
    @GetMapping("/hello")
    public String getHello() {
        return "Hello, GET request!";
    }
    
    @PostMapping("/hello")
    public String postHello(@RequestBody String body) {
        return "Hello, POST request!";
    }
    ```
    

#### 2.5 `@RequestParam`

- **作用**：用来接收请求中的查询参数。
- **示例**：
    
    ```java
    @GetMapping("/greet")
    public String greet(@RequestParam String name) {
        return "Hello, " + name;
    }
    ```
    

#### 2.6 `@PathVariable`

- **作用**：用于接收 URL 中的路径参数。
- **示例**：
    
    ```java
    @GetMapping("/user/{id}")
    public String getUser(@PathVariable Long id) {
        return "User ID: " + id;
    }
    ```
    

#### 2.7 `@RequestBody`

- **作用**：用于将 HTTP 请求的请求体（如 JSON）自动绑定到方法的参数上。
- **示例**：
    
    ```java
    @PostMapping("/users")
    public String createUser(@RequestBody User user) {
        return "User created: " + user.getName();
    }
    ```
    

### 3. **服务与依赖注入注解**

#### 3.1 `@Service`

- **作用**：标记一个类为服务层组件。与 `@Component` 一样，它也是一种组件扫描注解，Spring 会自动将标记为 `@Service` 的类注册为 Bean。
- **示例**：
    
    ```java
    @Service
    public class UserService {
        public String getUserName() {
            return "John Doe";
        }
    }
    ```
    

#### 3.2 `@Autowired`

- **作用**：用于自动注入 Bean。可以标注在构造器、字段或方法上，Spring 会自动查找并注入匹配的 Bean。
- **示例**：
    
    ```java
    @Autowired
    private UserService userService;
    ```
    

#### 3.3 `@Bean`

- **作用**：标记方法为 Spring Bean 的定义。当你需要在 Java 配置类中手动创建一个 Bean 时，可以使用 `@Bean` 注解。
- **示例**：
    
    ```java
    @Configuration
    public class AppConfig {
        @Bean
        public UserService userService() {
            return new UserService();
        }
    }
    ```
    

#### 3.4 `@Component`

- **作用**：通用的组件注解，Spring 会自动扫描并注册为 Spring 容器的 Bean。
- **示例**：
    
    ```java
    @Component
    public class MyComponent {
        public void doSomething() {
            System.out.println("Doing something!");
        }
    }
    ```
    

### 4. **配置与属性注解**

#### 4.1 `@Value`

- **作用**：用于注入外部配置文件的属性值。
- **示例**：
    
    ```java
    @Value("${app.name}")
    private String appName;
    ```
    

#### 4.2 `@ConfigurationProperties`

- **作用**：将配置文件中的属性值绑定到一个 POJO 类中。
- **示例**：
    
    ```java
    @ConfigurationProperties(prefix = "app")
    public class AppConfig {
        private String name;
        private String version;
    
        // getters and setters
    }
    ```
    

#### 4.3 `@Profile`

- **作用**：用于定义不同的配置文件配置。可以指定某个 Bean 只在特定的环境（如开发、测试、生产）下生效。
- **示例**：
    
    ```java
    @Profile("dev")
    @Service
    public class DevService {
        public String getServiceInfo() {
            return "Development Service";
        }
    }
    ```
    

### 5. **其他常用注解**

#### 5.1 `@PostConstruct` 和 `@PreDestroy`

- **作用**：`@PostConstruct` 在 Bean 初始化后执行，`@PreDestroy` 在 Bean 销毁之前执行。
- **示例**：
    
    ```java
    @PostConstruct
    public void init() {
        System.out.println("Bean Initialized!");
    }
    
    @PreDestroy
    public void destroy() {
        System.out.println("Bean Destroyed!");
    }
    ```
    

#### 5.2 `@RequestMapping`

- **作用**：用于处理 HTTP 请求的请求映射，常见的有 `@GetMapping`、`@PostMapping` 等。
- **示例**：
    
    ```java
    @RequestMapping("/home")
    public String home() {
        return "home";
    }
    ```
    

#### 5.3 `@ExceptionHandler`

- **作用**：用于捕获和处理控制器中的异常。
- **示例**：
    
    ```java
    @ExceptionHandler(Exception.class)
    public String handleException(Exception ex) {
        return "Error occurred: " + ex.getMessage();
    }
    ```
    

#### 5.4 `@EnableAutoConfiguration`

- **作用**：启用 Spring Boot 的自动配置，通常与 `@SpringBootApplication` 一起使用。
- **示例**：
    
    ```java
    @EnableAutoConfiguration
    public class MyApplication {
        public static void main(String[] args) {
            SpringApplication.run(MyApplication.class, args); } }
```
