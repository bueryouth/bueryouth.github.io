
@[TOC](MyBatis-plus整合SQLite3，实现增删查改)

## 关于SQLite
 - SQLite 是一个软件库，实现了自给自足的、无服务器的、零配置的、事务性的 SQL 数据库引擎。
 - SQLite主页：[https://www.sqlite.org/index.html](https://www.sqlite.org/index.html)

## 项目目录
```python
src
└─main
    ├─java
    │  └─com
    │      └─example
    │          └─demo
    │              ├─controller # 控制层目录
    │              ├─dao
    │              │  └─Mapper # 映射器目录
    │              └─entity    # 实体类目录
    └─resources
        ├─static
        │  └─sqlite            # 数据库目录
        └─templates
```

## 创建数据库
- 建一个user表，包含主键id，用户名name。

```sql
create table user
(
  id   INTEGER not null primary key autoincrement,
  name varchar(20)
);
```
## pom.xml依赖添加

```xml
<!-- web启动插件 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--lombok插件-->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>
<!-- sqlite3驱动包 -->
<dependency>
  <groupId>org.xerial</groupId>
  <artifactId>sqlite-jdbc</artifactId>
  <version>3.21.0.1</version>
</dependency>
<!--mybatis-plus插件-->
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.4.3.1</version>
</dependency>
```

## application.yml配置
```yml
# Tomcat
server:
  port: 8899

#spring
spring:
  datasource:
    #引用项目中的数据库文件
    driver-class-name: org.sqlite.JDBC
    url: jdbc:sqlite::resource:static/sqlite/cloud.db
    username:
    password:
  # 指定静态资源的路径
  web:
    resources:
      static-locations: classpath:/static/
```

## 创建User实体类
```java
/**
 * @author BuerYouth
 * @other 使用lombok，可以省略getter和setter方法
 */
@Data
public class User {
    @TableId(type = IdType.AUTO)
    private Integer id;
    private String name;
}
```
## 创建UserMapper映射类
```java
/**
 * @author BuerYouth
 */
@Mapper
public interface UserMapper extends BaseMapper<User> {}
```
## 创建TestController控制类
```java
/**
 * @author BuerYouth
 */
@RestController
public class TestController {
    @Autowired
    UserMapper userMapper;
    /** 增添数据 */
    @PostMapping("/insert")
    public Object insert(String name) {
        User user = new User();
        user.setName(name);
        return userMapper.insert(user);
    }
    /** 查询数据 */
    @GetMapping("/show")
    public Object show() {
        return userMapper.selectList(null);
    }
    /** 删除数据 */
    @DeleteMapping("/delete")
    public Object delete(Integer id) {
        return userMapper.deleteById(id);
    }
    /** 修改数据 */
    @PutMapping("update")
    public Object update(Integer id, String name) {
        User user = new User();
        user.setId(id);
        user.setName(name);
        return userMapper.updateById(user);
    }
}
```

## 测试接口
- 由于接口类型不同，建议使用API测试工具使用
- 或者全部改用GET请求，用浏览器测试

![在这里插入图片描述](https://img-blog.csdnimg.cn/c0e6ff8820bb4d83a5d0cf6768f37715.png)

##  资源下载
 - sqlite_demo
 - CSDN：[https://download.csdn.net/download/qq_43656397/22335670](https://download.csdn.net/download/qq_43656397/22335670)
 - Gitee：[https://gitee.com/bueryouth/sqlite-demo](https://gitee.com/bueryouth/sqlite-demo)
