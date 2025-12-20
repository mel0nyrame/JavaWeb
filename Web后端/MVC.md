# MVC架构模式

MVC(Model View Controller):软件工程的一种架构模式

- MVC(高内聚低耦合,符合开(增加)闭(修改)原则)
  - 模型(数据增删改查,处理业务逻辑):存放数据库对象等,处理数据逻辑
    - 实体类包(pojo/entity/bean):专门存放数据库对应实体类
    - 数据库访问包(dao/mapping):不同表格封装的类
    - 服务包(service):对数据进行预处理的类
  - 视图(界面):已变成独立的前端项目
    - 不存在
  - 控制器(响应):接收数据响应数据
    - 控制层包(controller)

## MVC开发过程

### POJO

>实现数据库对象

- 过程
  - 构建数据库对象
  - 定义数据库的字段变量
  - 实现序列化方法
  - 定义get() set()方法
  - 无参 有参构造器
  - 重写hashCode() equals()方法

>使用Lombok的注解可一键实现上列操作

- Lombok
  - @AllArgsConstructor:添加全参构造
  - @NoArgsConstructor:添加无参构造
  - @Data:实现get() set() hashCode() equals() toString()方法

### DAO

>对表的操作进行封装

- 过程
  - 将连接数据库的过程封装成一个类
  - 编写执行语句的BaseDAO
  - 定义每个表的规范(接口)
  - 实现每个表的接口并继承BaseDAO

### Service

>处理业务,调用DAO封装好的方法

### Controller

>接收用户请求,获得请求中的参数,调用Service中的方法处理并将结果响应给客户端

- 过程:针对表设计接口
  - table1
    - add
    - find
    - update
    - delete
  - table2
  - ...

在Servlet中可以使用子路径获取对应的请求以及反射调用对应方法,例:

```java
package service;

import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import model.DAO.impl.MyTableDAOImpl;
import model.POJO.MyTable;

import java.io.IOException;
import java.io.PrintWriter;
import java.lang.reflect.Method;
import java.util.List;


@WebServlet("/test/*")
public class TestServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp){
        resp.setContentType("application/json;charset=UTF-8");
        String requestURI = req.getRequestURI();
        String[] split = requestURI.split("/");
        String methodName = split[split.length - 1];

        try {
            Method declaredMethod = this.getClass().getDeclaredMethod(methodName,
                    HttpServletRequest.class,HttpServletResponse.class);
            declaredMethod.setAccessible(true);

            declaredMethod.invoke(this, req, resp);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    protected void selectAllData(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        MyTableDAOImpl myTableDAO = new MyTableDAOImpl();
        PrintWriter respWriter = resp.getWriter();

        List<MyTable> myTables = myTableDAO.selectAllData();

        for (MyTable myTable : myTables) {
            respWriter.println(myTable);
        }
    }
}
```
