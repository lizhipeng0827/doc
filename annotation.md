# 注解

服务注解

* `@HmmyService` 服务标记注解
* `@HmmyFunction` 方法注解

| 参数 | 名称 | 说明 |
| :---: | :--- | :--- |
| `powerType` | 是否需要验权 | `0`需要验证用户权限`1` 只需要登录 `2`不需要登录启动类注解 |
| `interfaceType` | 接口类型 | `0` 内网接口 \(服务调用\) `1` 外网接口 \(走网关\) `2` 内外网通用 \(服务调用或走网关\) |
| `isAuthCenter` | 是否鉴权中心鉴权 | `0` 鉴权中心鉴权 `1` 外网接口 \(走网关\) `2` 服务处理鉴权 |

```java
@HmmyService
@Service
@RequestMapping(value="helloWorld")
public class HelloWorld {

    @RequestMapping(value = "/say" ,method = RequestMethod.GET)
    @HmmyFunction(powerType = 2, interfaceType = 0, isAuthCenter = 0)
    public String say(String name){
        return name + "Hello World !";
    }

}
```

启动类注解

* `@SpringCloudApplication`
* `@EnableFeignClients`开启Feign，此注解主要用于服务之间调用

```javascript
@SpringCloudApplication
@EnableFeignClients(basePackages="com.hmmy")
public class HelloWorldApplication {

    public static void main(String[] args) {
        Class[] classes = {HelloWorldApplication.class, SpringBootConsoleApplication.class};
        SpringApplication.run(classes, args);
    }

}
```

配置动态刷新

`@RefreshScope`

配置的动态刷新，仅需要使用`@RefreshScope`注解。 使用配置类来创建bean时，若要实现注入bean的刷新，需要在配置类和Bean创建方法上均加上`@RefreshScope`注解。在对应配置被修改后，所有开启了刷新的注入bean在下一次调用时会重新进行初始化并替换掉之前注入的Bean。因bean替换时，弃用的bean会执行销毁方法来释放资源，故自定义Bean建议实现`Closeable`接口。

需要注意的是，由于`@RefreshScope`注解底层是使用cglib动态代理来实现，而cglib是创建动态子类继承来完成功能的增强，在使用`@RefreshScope`注解刷新包含final属性/final方法的bean时，会导致返回的代理对象为null的情况。此时需要将需要刷新的Bean封装一层，避免final属性/final方法的问题。

