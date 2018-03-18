---
title: Tomcat学习笔记 
tags: tomcat，容器
grammar_cjkRuby: true
---

## Tomcat是什么？
### 简介
Tomcat 是由 Apache 开发的一个 ==Servlet 容器==，实现了对 Servlet 和 JSP 的支持，并提供了作为Web服务器的一些特有功能，如Tomcat管理和控制平台、安全域管理和Tomcat阀等。

Tomcat不仅仅是一个Servlet容器，它也具有传统的Web服务器的功能：处理Html页面。但是与Apache相比，它的处理静态Html的能力就不如Apache.可以将Tomcat和Apache集成到一块，让Apache处理静态Html，而Tomcat处理Jsp和Servlet.这种集成只需要修改一下Apache和Tomcat的配置文件即可。

Tomcat 包含了一个配置管理工具，也可以通过编辑==XML格式的配置文件==来进行配置。

默认端口：8080

### Tomcat重要目录
* /bin - Tomcat 脚本存放目录（如启动、关闭脚本）。 `*.sh` 文件用于 Unix 系统； `*.bat` 文件用于 Windows 系统。
* /conf - Tomcat 配置文件目录。
* /logs - Tomcat 默认日志目录。
* /webapps - webapp 运行的目录。

### Web工程目录结构

``` stylus
|-- webapp                      # 站点根目录
    |-- META-INF                   # META-INF 目录
    |   `-- MANIFEST.MF              # 配置清单文件
    |-- WEB-INF                    # WEB-INF 目录
    |   |-- classes                # class文件目录
    |   |   |-- *.class              # 程序需要的 class 文件
    |   |   `-- *.xml              # 程序需要的 xml 文件
    |   |-- lib                    # 库文件夹
    |   |   `-- *.jar              # 程序需要的 jar 包
    |   `-- web.xml                # Web应用程序的部署描述文件
    |-- <userdir>                  # 自定义的目录
    |-- <userfiles>                 # 自定义的资源文件
```
==webapp==：工程发布文件夹。其实每个 war 包都可以视为 webapp 的压缩包。

==META-INF==：META-INF 目录用于存放工程自身相关的一些信息，元文件信息，通常由开发工具，环境自动生成。

==WEB-INF==：Java web应用的安全目录。所谓安全就是客户端无法访问，只有服务端可以访问的目录。

==/WEB-INF/classes==：存放程序所需要的所有 Java class 文件。

==/WEB-INF/lib==：存放程序所需要的所有 jar 文件。

/==WEB-INF/web.xml==：web 应用的部署配置文件。它是工程中最重要的配置文件，它描述了 servlet 和组成应用的其它组件，以及应用初始化参数、安全管理约束等。


## Tomcat四大容器
Tomcat主要有 ==连接器 Connector #006780== 和 ==容器 #006780==  两大部分，连接器主要负责建立和解析tcp 连接，具体的业务逻辑有容器处理。

Tomcat提供了==engine，host，context及wrapper==四种容器。这四种容器继承了一个容器基类，因此可以定制化。当然，tomcat也提供了标准实现。

![enter description here][1]


  [1]: ./images/QQ%E6%88%AA%E5%9B%BE20180318154659.png "框架机制"