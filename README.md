# 前言

此文档目的是为了能快速了解与快速上手操作。

微服务解决方案使用的`Spring Cloud Alibaba`，Spring Cloud Alibaba 是阿里巴巴提供的微服务开发一站式解决方案，是阿里巴巴开源中间件与 Spring Cloud 体系的融合。

Spring Cloud Alibaba里面包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

## Spring Cloud Alibaba 包含组件

![](https://pic4.zhimg.com/80/v2-46c0b9e0d41c441d222390c79a4cd53b\_720w.jpg)

## 当前已使用组件

* Nacos：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
* Sentinel：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。
* OpenFeign：是一个声明式的Web Service客户端。
* Dubbo：国内应用非常广泛的一款高性能 Java RPC 框架。
* Gateway：API网关，请求统一入口。

## 版本

| 名称                   | 版本              | 备注               |
| -------------------- | --------------- | ---------------- |
| Spring Boot          | `2.3.7.RELEASE` | -                |
| Spring Cloud         | `Hoxton.SR9`    | -                |
| Spring Cloud Alibaba | `2.2.5.RELEASE` | -                |
| Nacos                | `2.0.1`         | 注册中心、配置中心        |
| Gateway              | `2.2.6.RELEASE` | API网关            |
| Sentinel             | `2.2.5.RELEASE` | 流量控制、熔断降级、系统负载保护 |
| OpenFeign            | `2.2.6.RELEASE` | 服务调用             |
