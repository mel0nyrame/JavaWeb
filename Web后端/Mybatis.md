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

## 编写SQL语句并实现功能(注解/XML)

根据id实现CRUD功能

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
