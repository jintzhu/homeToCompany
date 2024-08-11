Servlet：运行在服务器端的小程序

Servlet是一个接口，定义了Java类被浏览器访问到（或者说被tomcat识别）的规则，











开发注意事项：

1. 当你将 `ServletRequest` 对象转换为 `HttpServletRequest` 时，通常是因为你需要使用 `HttpServletRequest` 提供的额外功能，比如HTTP请求相关的信息和操作。在进行类型转换之前，最好先使用 `instanceof` 检查对象是否确实是 `HttpServletRequest` 的实例。这可以避免 `ClassCastException` 的发生。



## cookie 

## token



## Web.xml

```
1.web.xml加载过程（步骤）
首先简单讲一下，web.xml的加载过程。当启动一个WEB项目时，容器包括（JBoss、Tomcat等）首先会读取项目web.xml配置文件里的配置，当这一步骤没有出错并且完成之后，项目才能正常地被启动起来。

1. 启动WEB项目的时候，容器首先会去它的配置文件web.xml读取两个节点:  <listener></listener>和<context-param></context-param>。

2. 紧接着，容器创建一个ServletContext（application），这个WEB项目所有部分都将共享这个上下文。

3. 容器以<context-param></context-param>的name作为键，value作为值，将其转化为键值对，存入ServletContext。

4. 容器创建<listener></listener>中的类实例，根据配置的class类路径<listener-class>来创建监听，在监听中会有contextInitialized(ServletContextEvent args)初始化方法，启动Web应用时，系统调用Listener的该方法，在这个方法中获得：

<span style="font-family:Times New Roman;">ServletContextapplication=ServletContextEvent.getServletContext();</span>  
context-param的值就是application.getInitParameter("context-param的键");得到这个context-param的值之后，你就可以做一些操作了。

 

5. 举例：你可能想在项目启动之前就打开数据库，那么这里就可以在<context-param>中设置数据库的连接方式（驱动、url、user、password），在监听类中初始化数据库的连接。这个监听是自己写的一个类，除了初始化方法，它还有销毁方法，用于关闭应用前释放资源。比如:说数据库连接的关闭，此时，调用contextDestroyed(ServletContextEvent args)，关闭Web应用时，系统调用Listener的该方法。

6. 接着，容器会读取<filter></filter>，根据指定的类路径来实例化过滤器。

7. 以上都是在WEB项目还没有完全启动起来的时候就已经完成了的工作。如果系统中有Servlet，则Servlet是在第一次发起请求的时候被实例化的，而且一般不会被容器销毁，它可以服务于多个用户的请求。所以，Servlet的初始化都要比上面提到的那几个要迟。

8. 总的来说，web.xml的加载顺序是:<context-param>-><listener>-><filter>-><servlet>。其中，如果web.xml中出现了相同的元素，则按照在配置文件中出现的先后顺序来加载。

9. 对于某类元素而言，与它们出现的顺序是有关的。以<filter>为例，web.xml中当然可以定义多个<filter>，与<filter>相关的一个元素是<filter-mapping>，注意，对于拥有相同<filter-name>的<filter>和<filter-mapping>元素而言，<filter-mapping>必须出现在<filter>之后，否则当解析到<filter-mapping>时，它所对应的<filter-name>还未定义。web容器启动初始化每个<filter>时，按照<filter>出现的顺序来初始化的，当请求资源匹配多个<filter-mapping>时，<filter>拦截资源是按照<filter-mapping>元素出现的顺序来依次调用doFilter()方法的。<servlet>同<filter>类似，此处不再赘述。
```

