# Introduction (简介)
Bootapp is a framework for fast deployment of modern apps. It contains basic modules including user account & authentication(configurable register/login method, third-party login, ...), information service(articles, carousel, announcements, ...), etc. 

Bootapp is initially built upon istio, however non-istio deployment can still work but with loss of management functions.

Bootapp 是一个为快速创建现代应用架构而生的服务框架，包含用户授权认证（可配置的注册、登录方法，第三方登录，...），信息服务（文章，轮播图，公告，...），等基础服务模块。

Bootapp 基于istio创建，但亦可脱离istio单独部署，此时很多服务治理功能将会缺失。

# Architecture (架构)
 ![image](https://github.com/bootapp/documentation/raw/master/images/architecture.jpg)