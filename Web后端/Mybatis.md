# Mybatis

持久层框架,用于简化JDBC开发

## 配置Mybatis

- application.properties

```yaml
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.username=root
spring.datasource.password=1234
```

## 编写简单SQL语句并实现功能

- 创建Mapper接口(即DAO对象)

```java
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user")
    public List<User> allUser();
}
```

- 实现方法

```java
@Autowired
private UserMapper userMapper;

@RequestMapping("/allUser")
public Result allUser() {
    return Result.success(userMapper.allUser());
}
```

## 编写SQL语句并实现功能(注解)

根据id实现CRUD功能

### 删除

根据id删除用户

- UserMapper.java

```java
@Delete("DELETE FROM user WHERE id = #{id}")
public int deleteUserById(Integer id);
```

- Controller.java

```java
@RequestMapping("/delete/{id}")
public Result deleteUserById(@PathVariable Integer id) {
    int row = userMapper.deleteUserById(id);
    return Result.success(row);
}
```

- 参数占位符
  - `#{}`:预编译SQL语句,自动设置参数值,一般用于参数设置
  - `${}`:直接拼接SQL语句,存在SQL注入的风险,一般用于动态拼接数据库和表名

### 增加

根据id增加用户

- UserMapper.java

```java
@Insert("INSERT INTO user(name, age) VALUES (#{name},#{age})") //#{}内填上pojo对应的属性名
public int addUser(User user);
```

- Controller.java

```java
@RequestMapping("/add")
public Result addUser(String name,Integer age) {
    int row = userMapper.addUser(new User(name,age));
    return Result.success(row);
}
```

#### 增加后获取主键值

- @Optional注解
  - keyProperty:将主键值赋值给pojo的某个属性
  - useGeneratedKeys:获取主键值

```java
@Optional(keyProperty = "",useGeneratedKeys = true)
```

### 更新

根据id更新用户

- UserMapper.java

```java
@Update("UPDATE user SET name = #{name},age=#{age} WHERE id = #{id}")
public int updateUser(User user);
```

- Controller.java

```java
@RequestMapping("/update/{id}")
public Result updateUser(@PathVariable Integer id,String name,Integer age) {
    int row = userMapper.updateUser(new User(id, name, age));
    return Result.success(row);
}
```

### 查询

当封装对象中的属性名和表中的字段名不相同时会封装失败

- 方案一:给表起别名(不常用)

```java
@Select("SELECT id,name,department_id departmentId FROM user WHERE id = #{departmentId}")
public List<User> getById();
```

- 方案二:使用Result注解(不常用)

```java
@Results({
    @Result(column = "字段名",property = "属性名")
})
@Select("Select * FROM user")
public List<User> getById();
```

- 方案三:开启Mybatis的自动驼峰命名

```properties
mybatis.configuration.map-underscore-to-camel-case=true
```

#### 查询条件在字符串内

若查询条件在字符串内例如`LIKE '%王%'`,则不能使用`#{}`,可使用字符串拼接函数concat('%',#{name},'%')

```java
@Select("SELECT * FROM user WHERE name LIKE CONCAT('%',#{name},'%')")
public List<User> getByIdAndName(String name);
```

## 编写复杂SQL语句并实现功能(XML)

### XML映射文件规范

- XML映射文件必须和Mapper接口名称相同,必须放在同包同名下面
  - UserMapper.java -> UserMapper.xml
  - java.com.mel0ny.springboot.mapper.UserMapper -> resources.com.mel0ny.springboot.mapper.UserMapper
- XML映射文件的namespace属性必须和Mapper接口名称一致
- XML映射文件中sql语句的id与Mapper接口中的方法名一致,并保持返回类型一致

---

- UserMapper.java

```java
@Mapper
public interface UserMapper(){
    public List<User> list(String name,Integer id);
}
```

- UserMapper.xml

```xml
<!--- 约束文件 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mel0ny.springboot.mapper.UserMapper">
    <select id="selectByNameAndName" resultType="com.mel0ny.springboot.pojo.User">
        SELECT * FROM user WHERE name = #{name} AND age = #{age}
    </select>
</mapper>
```

### 动态SQL语句

有些SQL很容易写死,需要根据需求使用SQL语句

#### \<if>标签

用于判断条件是否成立,使用test属性来进行判断,如果条件为true,则拼接SQL语句

- \<if>属性:
  - test:""(条件为true的表达式)
- \<where>可以自动删除`AND`
- \<set>可以自动删去条件前面的逗号

```java
public List<User> selectByNameOrAge(User user);

public int updateUserById(User user);
```

```xml
<mapper namespace="com.mel0ny.springboot.mapper.UserMapper">
    <select id="selectByNameOrAge" resultType="com.mel0ny.springboot.pojo.User">
        SELECT * FROM user
        <where>
            <if test="name != null">
                AND name = #{name}
            </if>
            <if test="age != null">
                AND age = #{age}
            </if>
        </where>
    </select>
    <update id="updateUserById">
        UPDATE user
        <set>
            <if test="name != null">name = #{name},</if>
            <if test="age != null">age = #{age}</if>
        </set>
        <where>
            id = #{id}
        </where>
    </update>
</mapper>
```

#### \<foreach>标签

循环拼接SQL语句(批量删除数据)

- \<foreach>属性:
  - collection=""(遍历的集合)
  - item=""(遍历的元素)
  - separator=""(分隔符)
  - open=""(遍历开始前的SQL片段)
  - close=""(遍历结束后的SQL片段)

>传入的参数为List<元素的属性>

```java
public int deleteUserByIdList(List<Integer> ids);
```

```xml
<!--- 相当于DELETE FROM user WHERE id IN (...)-->>
<delete id="deleteUserByIdList">
    DELETE FROM user WHERE id IN
    <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
```

#### \<sql>和\<include>标签

封装固定的重复性高的SQL语句

```xml
<sql id="selectAll">
    SELECT id,name,age
</sql>
<select id="..." resultType="...">
    <include refid="selectAll"/> //将语句拼在此处
</select>
```
