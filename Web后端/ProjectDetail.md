# 项目开发中的细节

## Restful接口建议规范

### REST风格

```html
https://localhost:8080/api/users/1 GET请求:查询id为1的用户信息
https://localhost:8080/api/users POST请求:新增用户
https://localhost:8080/api/users PUT请求:修改用户
https://localhost:8080/api/users/1 DELETE:删除id为1的用户信息
```

- GET:使用@GetMapping(url)注解
- POST:使用@PostMapping(url)注解
- PUT:使用@PutMapping(url)注解
- DELTE:使用@DeleteMapping(url)注解

### 响应状态

- Result.java

```java
package com.mel0ny.springboot.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Result {
    private Integer id;
    private String msg;
    private Object data;

    public static Result success() {
        return new Result(1,"success",null);
    }

    public static Result success(Object data){
        return new Result(1,"success",data);
    }

    public static Result success(String msg,Object data) {
        return new Result(1,msg,data);
    }

    public static Result error() {
        return new Result(0,"error",null);
    }

    public static Result error(Object data){
        return new Result(0,"error",data);
    }

    public static Result error(String msg,Object data) {
        return new Result(0,msg,data);
    }
}
```

### 接口文档

接口文档

- xxx功能接口
  - 基本信息
    - 请求路径
    - 请求方式
    - 接口描述
  - 请求参数
  - 响应参数
    - 参数格式
    - 参数说明
    - 响应数据样例

## 记录日志

- 在类上使用@Slf4j注解即可使用log对象记录日志

```java
@Slf4j
@RestController
public class TestController {
    //相当于private static Logger logger = LoggerFactory.getLogger(TestController.class);
}
```
