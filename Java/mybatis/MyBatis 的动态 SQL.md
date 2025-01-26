MyBatis 的动态 SQL 允许在运行时根据不同的条件动态生成 SQL 语句。它使得 SQL 查询更加灵活，可以避免硬编码 SQL 语句，简化了复杂条件下的 SQL 生成。MyBatis 提供了多种标签来实现动态 SQL，下面是对所有相关内容的总结。

### 1. **`<if>` 标签**

- **作用**：根据条件判断是否生成 SQL 片段。
    
- **用法**：
    
    ```xml
    <if test="condition">
      SQL片段
    </if>
    ```
    
    - `test` 属性：是一个 SpEL 表达式，返回 `true` 时生成 SQL。
    
    **示例**：
    
    ```xml
    <select id="selectUser" resultType="User">
      SELECT * FROM users
      <if test="username != null">
        AND username = #{username}
      </if>
    </select>
    ```
    

### 2. **`<choose>`、`<when>` 和 `<otherwise>` 标签**

- **作用**：相当于 `switch` 语句，根据不同条件选择生成 SQL。
- **用法**：
    
    ```xml
    <choose>
      <when test="condition1">
        SQL片段1
      </when>
      <when test="condition2">
        SQL片段2
      </when>
      <otherwise>
        默认SQL片段
      </otherwise>
    </choose>
    ```
    
    **示例**：
    
    ```xml
    <select id="selectUser" resultType="User">
      SELECT * FROM users
      <choose>
        <when test="username != null">
          AND username = #{username}
        </when>
        <when test="age != null">
          AND age = #{age}
        </when>
        <otherwise>
          AND status = 'active'
        </otherwise>
      </choose>
    </select>
    ```
    

### 3. **`<foreach>` 标签**

- **作用**：用于遍历集合或数组，生成 `IN` 语句或其他重复部分的 SQL。
- **用法**：
    
    ```xml
    <foreach item="item" collection="collection" open="(" separator="," close=")">
      #{item}
    </foreach>
    ```
    
    **示例**：
    
    ```xml
    <select id="selectUsers" parameterType="List" resultType="User">
      SELECT * FROM users WHERE id IN
      <foreach item="id" collection="list" open="(" separator="," close=")">
        #{id}
      </foreach>
    </select>
    ```
    

### 4. **`<trim>` 标签**

- **作用**：用于去除不必要的前缀或后缀（如 `AND` 或 `OR`）。
- **用法**：
    
    ```xml
    <trim prefix="AND" prefixOverrides="AND">
      <if test="condition">
        SQL片段
      </if>
    </trim>
    ```
    
    **示例**：
    
    ```xml
    <select id="selectUser" resultType="User">
      SELECT * FROM users
      <where>
        <trim prefix="AND" prefixOverrides="AND">
          <if test="username != null">
            username = #{username}
          </if>
          <if test="age != null">
            age = #{age}
          </if>
        </trim>
      </where>
    </select>
    ```
    

### 5. **`<set>` 标签**

- **作用**：用于生成 `UPDATE` 语句时，自动添加逗号并避免最后一列后多余的逗号。
- **用法**：
    
    ```xml
    <set>
      <if test="condition">
        SQL片段
      </if>
    </set>
    ```
    
    **示例**：
    
    ```xml
    <update id="updateUser" parameterType="User">
      UPDATE users
      <set>
        <if test="username != null">username = #{username},</if>
        <if test="age != null">age = #{age},</if>
      </set>
      WHERE id = #{id}
    </update>
    ```
    

### 6. **`<bind>` 标签**

- **作用**：在 SQL 中定义一个新的变量，并为其赋值，通常用于动态计算。
- **用法**：
    
    ```xml
    <bind name="varName" value="expression"/>
    ```
    
    **示例**：
    
    ```xml
    <select id="selectUser" resultType="User">
      <bind name="tableName" value="'user_' + ${type}"/>
      SELECT * FROM ${tableName}
    </select>
    ```
    

### 7. **`<sql>` 标签**

- **作用**：用于定义可重用的 SQL 片段，减少重复代码。
- **用法**：
    
    ```xml
    <sql id="sqlFragment">
      SQL片段
    </sql>
    ```
    
    然后通过 `<include>` 引用：
    
    ```xml
    <select id="selectUser" resultType="User">
      SELECT <include refid="sqlFragment"/> FROM users
    </select>
    ```
    

### 8. **`<where>` 标签**

- **作用**：自动去掉 SQL 语句中的多余的 `AND` 或 `OR`，用于 `SELECT` 或 `UPDATE` 语句。
- **用法**：
    
    ```xml
    <where>
      SQL片段
    </where>
    ```
    
    **示例**：
    
    ```xml
    <select id="selectUser" resultType="User">
      SELECT * FROM users
      <where>
        <if test="username != null">
          AND username = #{username}
        </if>
      </where>
    </select>
    ```
    

### 总结：

- MyBatis 提供了强大的动态 SQL 功能，能够根据不同条件生成灵活的 SQL 语句。
- 主要的动态 SQL 标签有 `<if>`, `<choose>`, `<foreach>`, `<trim>`, `<set>`, `<bind>`, `<sql>` 和 `<where>`。
- 动态 SQL 可以极大地减少硬编码 SQL 的重复，提高代码的可维护性与可扩展性，但也要避免滥用，以免导致 SQL 过于复杂。