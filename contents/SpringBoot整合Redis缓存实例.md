@[TOC](Spring Boot整合Redis缓存实例)
## Redis简介

**Redis**是一个开源的使用ANSI(美国国家标准协会) C语言编写、遵守BSD协议(一种开源协议)、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，它可以作为数据库、缓存和消息中间件，并提供多种语言的API。
[关于Redis](https://redis.io/)



## 场景介绍

利用Redis实现一个基本的缓存操作，写一个**接口A**接收浏览器传递的get参数，并装入缓存，再写一个**接口B**通过浏览器发送请求后，在浏览器展示**接口A传递的参数**。



## 实例环境及工具

IDEA开发工具，Redis数据库， Spring Boot框架，Maven项目管理工具



## 引入依赖启动器

在建好Spring Boot项目的前提下，向**pom.xml**添加依赖启动器。
```xml
<dependency>
    <groupId>com.github.binarylei</groupId>
    <artifactId>spring-boot-stater-test-data-redis</artifactId>
    <version>0.1.1-alpha</version>
</dependency>
```


## 全局配置Redis

以.properties为例，若是.yml文件，则应需更改
```js
# Redis服务器地址
spring.redis.host=127.0.0.1
# Redis服务器连接端口
spring.redis.port=6379
# Redis服务器连接密码（默认为空）
spring.redis.password=
```


## 编写测试接口

1. 在**controller**层添加**RedisController.java类**

![项目实例](https://img-blog.csdnimg.cn/20210627133216656.png)

2. 实例化StringRedisTemplate

```java
@Autowired
private StringRedisTemplate stringRedisTemplate;
```
3. 编写**测试接口A**

```java
@GetMapping("/setToken")
public String setToken(@RequestParam("token") String token) {
    stringRedisTemplate.opsForValue().set("token", token, 60, TimeUnit.SECONDS);
    String result = "token->" + token + "写入缓存，60秒过期";
    return result;
}
```
4. 编写**测试接口B**

```java
@GetMapping("/getToken")
public String getToken() {
    String token = stringRedisTemplate.opsForValue().get("token");
    System.out.println("token->" + token);
    return "Redis缓存token->" + token;
}
```
5. 完整RedisController.java

```java
/**
 * @author BuerYouth
 * @update 2021/6/27
 */
@RestController
public class RedisController {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @GetMapping("/setToken")
    public String setToken(@RequestParam("token") String token) {
        stringRedisTemplate.opsForValue().set("token", token, 60, TimeUnit.SECONDS);
        String result = "token->" + token + "写入缓存，60秒过期";
        return result;
    }

    @GetMapping("/getToken")
    public String getToken() {
        String token = stringRedisTemplate.opsForValue().get("token");
        System.out.println("token->" + token);
        return "Redis缓存token->" + token;
    }
}
```



## 运行实例

1. redis-server

![在这里插入图片描述](https://img-blog.csdnimg.cn/202106271335178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

2. 运行IDEA中的项目

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210627133706580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

3. 打开API测试工具测试接口

- 写入数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210627134010468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

浏览器测试
```js
http://127.0.0.1:8080/setToken?token=BuerYouth
```
- 读取数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210627134213125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

浏览器测试
```js
http://127.0.0.1:8080/getToken
```
- Redis管理工具查看

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210627134535659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)



## 其他

- 关于StringRedisTemlate
1.**StringRedisTemplate**继承了**RedisTemplate**；
2.**RedisTemplate**可以操作<Object,Object>对象类型数据，而其子类**StringRedisTemlate**则是专门针对<Sting,String>字符串类型的数据进行操作。
- StringRedisTemplate的简单用法
```java
template.opsForValue.set(key,value);//设置Redis键值对
template.opsForValue.get(key);//获取数据库key对应的值
```
