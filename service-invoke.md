# 服务调用

现阶段使用`openFeign`调用，后面还会新增`dubbo`服务调用。

* 调用方式一：

```java
public class HelloWorld {
    @Autowired
    private RemoteService remoteService;

    public String say(String name){
        Bean param = new Bean();
        param.setName(name);
        remoteService.invoke(PATH, param, new TypeReference<CommonResp<Bean>>() {});
        return name + "Hello World !";
    }
}
```

```java
/**
 * 调用服务
 * @param url 服务路径 serviceName/xxxxx/xxxxx
 * @param param 参数
 * @param type 返回类型
 */
 public <T>T invoke(String url, Object param, TypeReference<T> type){}
```

* 调用方式二：

```java
public class HelloWorld {
    @Autowired
    private RemoteService remoteService;

    public String say(String name){
        JSONObject param = new JSONObject();
        param.put("name",name);
        remoteService.invoke(PATH, param);
        return name + "Hello World !";
    }
}
```

```java
/**
 * 调用服务
 * @param url 服务路径 serviceName/xxxxx/xxxxx
 * @param param 参数
 * @return com.alibaba.fastjson.JSONObject
 */
 public JSONObject invoke(String url, JSONObject param) {}
```
