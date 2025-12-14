# XML

扩展标记语言,常用于传输数据

特点:

- 标签自定义(有约束文件可自定义)
- 格式必须类似于树
- 开头格式固定

```xml
<?xml version="1.0" encoding="UTF8"?>//首行必须是这个

<jdbc>
    <user>root</user>
    <password>root</password>
    <port>3306</port>
</jdbc>
```

## DOM4J解析XML

几乎不用了解即可
