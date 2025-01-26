Lombok 是一个 Java 库，它通过注解的方式自动生成 Java 类中的常用方法（如 getter、setter、toString、hashCode 和 equals 等），从而简化 Java 代码，提高开发效率，避免大量重复的模板代码。Lombok 在编译时通过注解处理器生成代码，所以编译后的字节码中才有这些方法，但源码中不会显示。

### 1. 添加 Lombok 依赖

在 Maven 项目中，首先需要在 `pom.xml` 中添加 Lombok 依赖：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version> <!-- 使用最新的版本 -->
    <scope>provided</scope>
</dependency>
```

对于 Gradle 项目，添加如下依赖：

```gradle
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.24'  // 使用最新版本
    annotationProcessor 'org.projectlombok:lombok:1.18.24'  // 编译时使用 Lombok
}
```

### 2. 常用注解

#### 2.1 `@Getter` / `@Setter`

- `@Getter`：自动为类的所有字段生成 getter 方法。
- `@Setter`：自动为类的所有字段生成 setter 方法。

```java
@Getter
@Setter
public class Person {
    private String name;
    private int age;
}
```

上面的代码会自动生成如下方法：

```java
public String getName() {
    return name;
}

public void setName(String name) {
    this.name = name;
}

public int getAge() {
    return age;
}

public void setAge(int age) {
    this.age = age;
}
```

#### 2.2 `@ToString`

`@ToString` 注解会自动生成 `toString()` 方法，默认会打印所有字段。如果需要控制输出字段，可以使用 `@ToString.Exclude` 来排除某些字段。

```java
@ToString
public class Person {
    private String name;
    private int age;
}
```

自动生成的 `toString()` 方法如下：

```java
@Override
public String toString() {
    return "Person(name=" + name + ", age=" + age + ")";
}
```

如果排除某些字段：

```java
@ToString(exclude = "age")
public class Person {
    private String name;
    private int age;
}
```

输出时将不会包括 `age` 字段。

#### 2.3 `@EqualsAndHashCode`

`@EqualsAndHashCode` 注解会自动为类生成 `equals()` 和 `hashCode()` 方法，默认比较所有字段。如果只需要比较某些字段，可以通过 `@EqualsAndHashCode.Include` 和 `@EqualsAndHashCode.Exclude` 来控制。

```java
@EqualsAndHashCode
public class Person {
    private String name;
    private int age;
}
```

生成的 `equals()` 和 `hashCode()` 方法会根据类的所有字段来比较对象。

如果需要自定义比较字段：

```java
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class Person {
    @EqualsAndHashCode.Include
    private String name;

    @EqualsAndHashCode.Include
    private int age;
}
```

#### 2.4 `@NoArgsConstructor` / `@AllArgsConstructor` / `@RequiredArgsConstructor`

- `@NoArgsConstructor`：生成无参构造函数。
- `@AllArgsConstructor`：生成包含所有字段的构造函数。
- `@RequiredArgsConstructor`：生成包含 `final` 字段或 `@NonNull` 注解字段的构造函数。

```java
@NoArgsConstructor
public class Person {
    private String name;
    private int age;
}
```

```java
@AllArgsConstructor
public class Person {
    private String name;
    private int age;
}
```

```java
@RequiredArgsConstructor
public class Person {
    private final String name;
    private int age;
}
```

生成的构造函数会自动根据注解要求生成。

#### 2.5 `@Data`

`@Data` 注解是一个组合注解，它包括了 `@Getter`、`@Setter`、`@ToString`、`@EqualsAndHashCode` 和 `@RequiredArgsConstructor` 等注解。使用这个注解时，所有这些常用方法都会自动生成。

```java
@Data
public class Person {
    private String name;
    private int age;
}
```

这将自动生成如下方法：

- `getName()`, `setName()`
- `getAge()`, `setAge()`
- `toString()`
- `equals()` 和 `hashCode()` 方法
- 无参构造方法（如果字段是 `final`，则会生成包含所有 `final` 字段的构造方法）

#### 2.6 `@Value`

`@Value` 注解是一个不可变类的组合注解，相当于同时使用了 `@Getter`、`@ToString`、`@EqualsAndHashCode` 和 `@AllArgsConstructor`，并且所有字段都是 `final` 类型的。

```java
@Value
public class Person {
    private String name;
    private int age;
}
```

生成的类：

- 所有字段默认是 `final`。
- 自动生成构造方法、getter 方法、`toString()`、`equals()` 和 `hashCode()`。

#### 2.7 `@NonNull`

`@NonNull` 注解表示一个字段在构造函数或方法调用时不能为 `null`。如果该字段为 `null`，会抛出 `NullPointerException`。

```java
public class Person {
    private String name;

    public Person(@NonNull String name) {
        this.name = name;
    }
}
```

### 3. 其他常用功能

#### 3.1 `@Slf4j`

Lombok 提供了 `@Slf4j` 注解，用于自动生成日志对象。你无需手动声明日志对象。

```java
@Slf4j
public class Person {
    public void printName(String name) {
        log.info("The name is {}", name);
    }
}
```

#### 3.2 `@Cleanup`

`@Cleanup` 注解可以帮助你自动关闭资源。它是对 `try-with-resources` 的一种简化，可以自动调用 `close()` 方法。

```java
@Cleanup
InputStream inputStream = new FileInputStream("file.txt");
```

`@Cleanup` 会确保 `inputStream` 在方法执行结束时自动关闭，避免资源泄漏。

#### 3.3 `@SuperBuilder`

`@SuperBuilder` 注解可以帮助你实现构建者模式，支持继承和多态。

```java
@Getter
@SuperBuilder
public class Person {
    private String name;
    private int age;
}

@Getter
@SuperBuilder
public class Employee extends Person {
    private String jobTitle;
}
```

使用 `@SuperBuilder` 注解后，可以使用构建者模式来创建 `Person` 和 `Employee` 实例：

```java
Person person = Person.builder()
    .name("John")
    .age(30)
    .build();

Employee employee = Employee.builder()
    .name("Jane")
    .age(25)
    .jobTitle("Developer")
    .build();
```

### 4. 总结

Lombok 是一个非常实用的工具，通过注解的方式自动生成 Java 类中常见的代码，从而减少了冗长的模板代码，提升了开发效率。常用的注解包括 `@Getter`、`@Setter`、`@ToString`、`@EqualsAndHashCode`、`@Data`、`@Value`、`@NoArgsConstructor`、`@AllArgsConstructor` 等，Lombok 还提供了如 `@Slf4j`、`@Cleanup` 和 `@SuperBuilder` 等增强功能，极大地方便了日常开发中的常见需求。

在使用 Lombok 时，注意：

1. **编译时生效**：Lombok 生成的代码只在编译后存在，源码中并不会显示，因此需要在 IDE 中安装 Lombok 插件以支持正确的代码提示。
2. **代码可读性**：过度使用 Lombok 可能会导致代码不可读或不容易追踪错误，适当使用可以提高开发效率。
