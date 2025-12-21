# 过滤器

- Filter:过滤器,JavaEE的技术规范之一,作用目标资源请求进行过滤的一套技术规范

![alt text](/Web后端/img/image5.png)

- 应用场景
  - 日志的记录
  - 性能的分析
  - 乱码的处理

开发过滤器流程

- 实现Filter接口
- 重写过滤方法(doFilter)
- 配置过滤器
  - web.xml
  - 注解(@WebFilter)

过滤请求和响应的方法

- 请求到达目标资源之前,先经过过滤器
- 判断请求是否能够到达资源,然后再服务端做出响应处理
  - 判断是否登录
  - 有没有权限
  - 放行代码(`filterChain.doFilter(servletRequest,servletResponse)`)
- 在响应之前再次经过过滤器检查是否满足条件

过滤器生命周期

- 构造器(public Filter):启动项目
- 初始化(init):构造完成之后
- 过滤(doFilter):初始化之后
- 销毁(destroy):关闭服务器

过滤器链:多个过滤器形成对同一个资源进行过滤,不同过滤器工作范围不同

![alt text](img/image6.png)

>过滤器的顺序通常在xml中配置,若无配置则以类名顺序执行过滤器,在Spring等框架可以使用@Order注解注明顺序
