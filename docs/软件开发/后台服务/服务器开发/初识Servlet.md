# 初识 Servlet

> 最近需要学习一下服务器的开发，就从 Servlet 入手了。
>
> 初有体会，感觉其实就是调用了一个函数，传进去了几个参数，对象里处理完了又给你返回个值，只是因为是远程的，所以包装了一层，或是 Http协议，或是容器，我也形容不准，dalao 们勿喷

## Tomcat 的配置

Tomcat 的基础配置我就不说了，很简单，我要说的是在 idea 里的配置。

* 进入 Run/Debug Configuration 里面添加 Tomcat
* 需要注意在 Deployment 里配置 Artifact 时有个 Application context，这个里面如果什么都不填的话，访问路径就是你在 web.xml 里个 servlet 配置的 url，网上很多都会在```localhost:8080/```和你的 url 之间加上项目名称，这应该是 MyEclipse 的配置，在 idea 里并不是这样，这个坑的我好惨的
* html 文件在 web 包根目录下，当然也可以放在包下，不过这样访问路径就要带上包名：```localhost:8080/packagename/html```

---

剩下的就是一些 API，多了解了解写一写就熟练了，跟 JSP 关系挺大的，但其实我并不会 JSP 哈哈