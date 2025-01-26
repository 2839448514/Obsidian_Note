在 Spring Boot 项目中使用 MyBatis 时，可以执行以下操作。以下是 MyBatis 在 Spring Boot 中常用的详细操作总结：

### 1. 配置和集成

#### 1.1 添加依赖

在 `pom.xml` 中添加 MyBatis 和数据库驱动依赖。

```xml
<!-- MyBatis Starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>

<!-- 数据库驱动（例如 MySQL） -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
```

#### 1.2 配置数据源

在 `application.properties` 或 `application.yml` 中配置数据源信息。

`application.properties` 示例：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
mybatis.mapper-locations=classpath:/mapper/*.xml
mybatis.type-aliases-package=com.example.demo.model
```

#### 1.3 配置 MyBatis 扫描 Mapper 类

可以通过 `@MapperScan` 注解自动扫描 MyBatis Mapper 接口。

```java
@SpringBootApplication
@MapperScan("com.example.demo.mapper")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### 2. 常用操作

#### 2.1 插入操作

使用 `@Insert` 注解进行插入操作：

```java
@Mapper
public interface UserMapper {
    @Insert("INSERT INTO user (name, age, email) VALUES (#{name}, #{age}, #{email})")
    void insertUser(User user);
}
```

或者使用 XML 文件进行插入操作：

```xml
<insert id="insertUser" parameterType="com.example.demo.model.User">
    INSERT INTO user (name, age, email)
    VALUES (#{name}, #{age}, #{email})
</insert>
```

#### 2.2 查询操作

**查询单条数据：**

```java
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user WHERE id = #{id}")
    User selectUserById(int id);
}
```

**查询所有数据：**

```java
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user")
    List<User> selectAllUsers();
}
```

**查询带条件的数据：**

```java
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user WHERE age > #{age}")
    List<User> selectUsersByAge(int age);
}
```

**使用 XML 文件查询：**

```xml
<select id="selectUserById" parameterType="int" resultType="com.example.demo.model.User">
    SELECT * FROM user WHERE id = #{id}
</select>
```

#### 2.3 更新操作

```java
@Mapper
public interface UserMapper {
    @Update("UPDATE user SET name = #{name}, age = #{age}, email = #{email} WHERE id = #{id}")
    void updateUser(User user);
}
```

使用 XML 文件更新：

```xml
<update id="updateUser" parameterType="com.example.demo.model.User">
    UPDATE user
    SET name = #{name}, age = #{age}, email = #{email}
    WHERE id = #{id}
</update>
```

#### 2.4 删除操作

```java
@Mapper
public interface UserMapper {
    @Delete("DELETE FROM user WHERE id = #{id}")
    void deleteUser(int id);
}
```

使用 XML 文件删除：

```xml
<delete id="deleteUser" parameterType="int">
    DELETE FROM user WHERE id = #{id}
</delete>
```

### 3. 使用 XML 映射文件

MyBatis 支持将 SQL 映射放入 XML 文件中，这样可以避免将 SQL 语句硬编码到 Java 代码中。映射文件通常与 Mapper 接口配合使用。

#### 3.1 XML 配置示例

在 `resources/mapper` 目录下创建 `UserMapper.xml` 文件：

```xml
<mapper namespace="com.example.demo.mapper.UserMapper">
    <select id="selectUserById" parameterType="int" resultType="com.example.demo.model.User">
        SELECT * FROM user WHERE id = #{id}
    </select>

    <insert id="insertUser" parameterType="com.example.demo.model.User">
        INSERT INTO user (name, age, email) VALUES (#{name}, #{age}, #{email})
    </insert>

    <update id="updateUser" parameterType="com.example.demo.model.User">
        UPDATE user SET name = #{name}, age = #{age}, email = #{email} WHERE id = #{id}
    </update>

    <delete id="deleteUser" parameterType="int">
        DELETE FROM user WHERE id = #{id}
    </delete>
</mapper>
```

#### 3.2 配置扫描 Mapper XML

在 `application.properties` 中配置 XML 文件的扫描路径：

```properties
mybatis.mapper-locations=classpath:/mapper/*.xml
```

### 4. 分页查询

使用 MyBatis 分页插件（如 PageHelper）来进行分页查询。

#### 4.1 配置 PageHelper 插件

首先，在 `pom.xml` 中添加 PageHelper 依赖：

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.1.10</version>
</dependency>
```

#### 4.2 分页查询示例

在查询方法中使用 `PageHelper.startPage()` 来启动分页：

```java
public List<User> getUsersWithPagination(int pageNum, int pageSize) {
    PageHelper.startPage(pageNum, pageSize);
    return userMapper.selectAllUsers();
}
```

### 5. 事务管理

在 Spring Boot 中，事务管理可以使用 `@Transactional` 注解进行。

#### 5.1 添加事务注解

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    @Transactional
    public void addUserAndUpdate(User user) {
        userMapper.insertUser(user);
        userMapper.updateUser(user);
    }
}
```

### 6. 批量操作

#### 6.1 批量插入

通过 `foreach` 标签在 XML 中实现批量插入：

```xml
<insert id="insertUsers" parameterType="java.util.List">
    INSERT INTO user (name, age, email)
    VALUES
    <foreach collection="list" item="user" separator=",">
        (#{user.name}, #{user.age}, #{user.email})
    </foreach>
</insert>
```

#### 6.2 批量删除

```xml
<delete id="deleteUsers" parameterType="java.util.List">
    DELETE FROM user WHERE id IN
    <foreach collection="list" item="id" open="(" separator="," close=")">
        #{id}
    </foreach>
</delete>
```

### 7. 自定义类型处理器

MyBatis 允许自定义类型处理器，将 Java 类型转换为 SQL 类型。

#### 7.1 编写自定义类型处理器

```java
@MappedTypes(User.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class UserTypeHandler extends BaseTypeHandler<User> {

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, User parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, parameter.getName());
    }

    @Override
    public User getNullableResult(ResultSet rs, String columnName) throws SQLException {
        return new User(rs.getString(columnName));
    }

    @Override
    public User getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        return new User(rs.getString(columnIndex));
    }

    @Override
    public User getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        return new User(cs.getString(columnIndex));
    }
}
```

#### 7.2 在 XML 中配置类型处理器

```xml
<resultMap id="userResultMap" type="com.example.demo.model.User">
    <result column="name" property="name" typeHandler="com.example.demo.handler.UserTypeHandler"/>
</resultMap>
```

### 总结

MyBatis 在 Spring Boot 中的操作包括但不限于：配置数据源和扫描器，执行基本的 CRUD 操作、分页查询、事务管理、批量操作、自定义类型处理器等。通过使用 XML 映射文件，我们可以更灵活地管理 SQL 语句，并实现更复杂的数据库交互。
