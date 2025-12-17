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
  - 重写hashcode() equals()方法

>使用Lombok的注解可一键实现上列操作

- Lombok
  - @AllArgsConstructor:添加全参构造
  - @NoArgsConstructor:添加无参构造
  - @Data:实现get() set() hashcode() equals() toString()方法

### DAO
