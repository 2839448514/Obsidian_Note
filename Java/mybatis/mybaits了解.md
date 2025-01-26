MyBatis 是一个用于数据持久化的框架，它通过映射 SQL 语句到 Java 对象，为开发人员提供了较为自由且灵活的数据操作方式。Spring Boot 提供了对 MyBatis 的原生支持，可以通过简单的配置将 MyBatis 集成到 Spring Boot 项目中。接下来，我们将详细介绍 MyBatis 在 Spring Boot 中的集成和使用。

### 1. **MyBatis 简介**

MyBatis 是一个持久层框架，它支持自定义 SQL、存储过程和高级映射。与 JPA（Java Persistence API）不同，MyBatis 不使用对象关系映射（ORM），而是通过映射 SQL 语句来进行数据库操作。MyBatis 的优点在于它更灵活，可以完全控制 SQL 执行过程。

### 2. **MyBatis 在 Spring Boot 中的集成**

Spring Boot 集成 MyBatis 的过程是非常简化的，因为 Spring Boot 提供了开箱即用的 `mybatis-spring-boot-starter`。它能自动为你配置 MyBatis 的各种参数、数据源以及事务管理。

#### 2.1 **添加依赖**

要在 Spring Boot 项目中使用 MyBatis，首先需要在 `pom.xml` 中添加相关依赖。

```xml
<dependencies>
    <!-- Spring Boot Starter Web (如果你使用 Web 功能的话) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Starter MyBatis -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version> <!-- 可以根据需要修改为最新版 -->
    </dependency>

    <!-- MySQL 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!-- Spring Boot Starter JDBC (如果你没有使用 JPA) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>

    <!-- 用于测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

- `mybatis-spring-boot-starter` 是用于简化 MyBatis 配置的核心依赖。
- `mysql-connector-java` 是连接 MySQL 数据库的驱动。

#### 2.2 **配置数据源**

在 `application.properties` 或 `application.yml` 中配置数据库连接信息。Spring Boot 会自动根据这些配置来创建数据源，并将它们注入到 MyBatis。

```properties
# 数据源配置
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# MyBatis 配置
mybatis.mapper-locations=classpath:/mappers/*.xml
mybatis.type-aliases-package=com.example.domain
mybatis.configuration.map-underscore-to-camel-case=true
```

- `spring.datasource.url`：配置数据库的连接 URL。
- `spring.datasource.username` 和 `spring.datasource.password`：配置数据库的用户名和密码。
- `mybatis.mapper-locations`：配置 MyBatis 映射文件（XML）的位置。
- `mybatis.type-aliases-package`：配置实体类的包路径，MyBatis 会自动扫描并注册这些类作为别名。
- `mybatis.configuration.map-underscore-to-camel-case`：配置 MyBatis 将数据库字段名（如 `first_name`）自动映射为 Java 类的驼峰命名（如 `firstName`）。

#### 2.3 **创建 Mapper 接口**

MyBatis 使用 `Mapper` 接口来定义 SQL 操作。接口方法的命名和映射 SQL 语句的 ID 一一对应。

```java
package com.example.mapper;

import com.example.domain.User;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface UserMapper {

    @Select("SELECT * FROM users WHERE id = #{id}")
    User findById(int id);

    List<User> findAll();
}
```

- `@Select` 注解可以直接在方法上写 SQL 查询，MyBatis 会自动执行此 SQL 并返回查询结果。
- `findById` 查询 ID 为 `id` 的用户。
- `findAll` 查询所有用户。

#### 2.4 **创建 Mapper XML 文件**

除了注解外，MyBatis 还支持使用 XML 配置 SQL 查询。通常在大型项目中，SQL 语句和业务逻辑分离，使用 XML 会更加清晰。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<mapper namespace="com.example.mapper.UserMapper">

    <!-- 查询用户 -->
    <select id="findById" resultType="com.example.domain.User">
        SELECT * FROM users WHERE id = #{id}
    </select>

    <!-- 查询所有用户 -->
    <select id="findAll" resultType="com.example.domain.User">
        SELECT * FROM users
    </select>

    <!-- 插入用户 -->
    <insert id="insertUser" parameterType="com.example.domain.User">
        INSERT INTO users (name, age, email)
        VALUES (#{name}, #{age}, #{email})
    </insert>
</mapper>
```

- `namespace`：指定当前 `XML` 文件的映射接口路径。
- `select`：定义查询操作。
- `insert`：定义插入操作。

#### 2.5 **创建实体类（POJO）**

为 MyBatis 提供的数据模型类（POJO）通常与数据库表结构一一对应。每个属性与数据库表的列对应。

```java
package com.example.domain;

public class User {
    private int id;
    private String name;
    private int age;
    private String email;

    // Getters and Setters
}
```

#### 2.6 **配置 Mapper 扫描**

通过 `@MapperScan` 注解扫描 Mapper 接口。`@MapperScan` 会自动扫描指定的包，查找所有的 Mapper 接口。

```java
package com.example;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.example.mapper") // 扫描 Mapper 接口
public class MyBatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyBatisApplication.class, args);
    }
}
```

#### 2.7 **使用 Mapper**

在服务类中通过 `@Autowired` 注入 `Mapper`，并调用相关方法。

```java
package com.example.service;

import com.example.domain.User;
import com.example.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User getUserById(int id) {
        return userMapper.findById(id);
    }

    public List<User> getAllUsers() {
        return userMapper.findAll();
    }
}
```

### 3. **MyBatis 高级特性**

#### 3.1 **动态 SQL**

MyBatis 支持动态 SQL，可以根据条件动态生成查询语句。使用 `if`、`choose`、`foreach` 等标签来构造 SQL。

```xml
<select id="findByConditions" resultType="com.example.domain.User">
    SELECT * FROM users
    <where>
        <if test="name != null">AND name = #{name}</if>
        <if test="age != null">AND age = #{age}</if>
    </where>
</select>
```

#### 3.2 **缓存机制**

MyBatis 支持两级缓存：一级缓存和二级缓存。

- **一级缓存**：默认启用，作用范围是 `SqlSession`，在同一个 `SqlSession` 内，重复查询会返回缓存的结果。
- **二级缓存**：跨 `SqlSession` 的缓存，通常配置在 Mapper 文件中。

启用二级缓存：

```xml
<cache/>
```

#### 3.3 **分页插件**

MyBatis 支持通过分页插件来实现分页查询。常见的分页插件有 `PageHelper`。

#### 3.4 **事务管理**

MyBatis 可以与 Spring 的事务管理器集成。在 Spring Boot 中，事务由 Spring 提供的 `@Transactional` 注解管理。

```java
@Service
@Transactional
public class UserService {
    // 事务管理
}
```

### 4. **总结**

MyBatis 是一个灵活且强大的持久层框架，它通过 SQL 映射提供了高度可定制的查询方式。在 Spring Boot 中，MyBatis 通过 `mybatis-spring-boot-starter` 提供了非常简便的集成方式。通过配置数据源、Mapper 接口、Mapper XML 文件以及事务管理，我们可以快速将 MyBatis 集成到 Spring Boot 项目中，实现高效的数据库操作。

MyBatis 的灵活性和高可定制性使得它非常适合需要复杂 SQL 查询和高度自定义的场景。
