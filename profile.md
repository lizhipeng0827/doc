# 配置文件

## 项目配置文件

`项目配置文件`需要在`Nacos配置中心`配置，如：`application.properties`

* 服务名称命名规范

服务名称必须使用小写，分隔符使用`-`。如：`qrcode-service`

在`application.properties`中，服务名称`hmmy.server.name`与`bootstrap.yml`项目启动配置文件服务名称`bootstrap.yml`相同，如：`hmmy.server.name=qrcode-service`。

{% hint style="danger" %}
注意事项

1. hprose服务如果需要注册到注册中心，需要添加 `hmmy.server.old=true`
2. 同时需要注册到以前的注册中心，需要添加 `hmmy.server.hasHprose=true`
{% endhint %}

{% hint style="info" %}
配置用户拦截器

* com.hmmy.login.interceptors=/gardenService/\*\*
{% endhint %}

### Nacos配置步骤 <a href="#nacos" id="nacos"></a>

* 登录Nacos

{% hint style="info" %}
Nacos配置中心

* 地址：[http://192.168.2.233:8848/nacos](http://192.168.2.233:8848/nacos) （测试环境）
* 账号：nacos
* 密码：nacos
{% endhint %}

登录后打开配置列表，选择命名空间`test`(配置管理上方)，点击右上方“+”添加按钮，如下图所示

![配置页面](http://hmmy-mall-nursery-stock.oss-cn-beijing.aliyuncs.com/20210607093108.png)

* 添加配置文件

{% hint style="info" %}
`字段说明`

1. `Data Id`：配置文件名称，如：测试环境：`xxx-service-dev.properties`，正式环境：`xxx-service-prod.properties`。命名规范：`服务名称-环境标识.配置文件类型`
2. `Group`：默认不需要修改
3. `配置格式`：配置文件类型，与配置文件名称后缀对应，如果是yml类型则选择YAML
4. `配置内容`：配置文件内容
{% endhint %}

![新建配置](http://hmmy-mall-nursery-stock.oss-cn-beijing.aliyuncs.com/20210607092345.png)

## 项目启动配置文件

配置文件路径 `src/main/resources/bootstrap.yml`

此配置文件只需更改`server.port`端口号、`spring.application`项目名称

环境切换：`spring.profiles.active`，正式环境：`prod`，测试环境：`dev`

```yaml
server:
  port: 23501
spring:
  profiles:
    active: dev
  application:
    name: member-info

#开发环境
---
spring:
  profiles: dev
  cloud:
    nacos:
      config:
        file-extension: properties
        namespace: c367eff6-4ecf-4d52-bdd1-5b519e58d9ed
        server-addr: 192.168.2.233:8848
        shared-configs:
          - ${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
        extension-configs:
          - data-id: ${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
            group: DEFAULT_GROUP
            refresh: true
      discovery:
        namespace: c367eff6-4ecf-4d52-bdd1-5b519e58d9ed
        server-addr: 192.168.2.233:8848


#正式环境
---
spring:
  profiles: prod
  cloud:
    nacos:
      config:
        file-extension: properties
        namespace: 7d3fec7d-3f04-46fa-b4d8-589ceef5645d
        server-addr: 172.16.0.98:8848,172.16.0.99:8848,172.16.0.114:8848
        shared-configs:
          - ${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
        extension-configs:
          - data-id: ${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}
            group: DEFAULT_GROUP
            refresh: true
      discovery:
        namespace: 7d3fec7d-3f04-46fa-b4d8-589ceef5645d
        server-addr: 172.16.0.98:8848,172.16.0.99:8848,172.16.0.114:8848
```

## 路由配置

默认情况下，路由是动态路由，自动生成。如：

`http://xxx-service/xxx/xxx/xxx`

### 前端API地址兼容 <a href="#hprose" id="hprose"></a>

{% hint style="danger" %}
注意事项

新网关服务名称因规范问题，不允许驼峰命名，只能是全小写与全大写两种，所以兼容以前地址需要做一次路由转发。hmmy-routes配置文件用于配置路由，为JSON格式。
{% endhint %}

nacos 配置文件 ： `hmmy-routes`

![hmmy-routes配置文件](http://hmmy-mall-nursery-stock.oss-cn-beijing.aliyuncs.com/20210617090730.png)

![配置详情](https://hmmy-mall-nursery-stock.oss-cn-beijing.aliyuncs.com/20210617090859.png)

{% hint style="danger" %}
此配置应用于兼容hprose网关地址，如：/xxxService/xxxxRPC/xxxx，新网关地址：/xxx-service/xxxService/xxxxRPC/xxxx，此配置会根据xxxService转发到xxx-service处理。

predicates匹配路由，uri为目标服务

```javascript
{
        "id": "hmmy-photo-service",
        "predicates": [
            {
                "name": "Path",
                "args": {
                    "pattern": "/photoService/**"
                }
            }
        ],
        "uri": "lb://photo-service",
        "filters": []
    }
```
{% endhint %}

### easyui 页面转发 <a href="#easyui" id="easyui"></a>

{% hint style="danger" %}
uri为原始地址，配置后完整地址为：`http://网关地址/authpage/config/xxx.html`

```javascript
    {
        "id": "hmmy-auth-page",
        "predicates": [
            {
                "name": "Path",
                "args": {
                    "pattern": "/authpage/**"
                }
            }
        ],
        "uri": "http://192.168.2.233:23504",
        "filters": [
            {
                "name":"RewritePath",
                "args":{
                    "_genkey_0": "/authpage/(?<remaining>.*)",
                    "_genkey_1": "/$\\{remaining}"
                }
            }
        ]
    }
```
{% endhint %}

