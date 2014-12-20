---
layout: post
category : Spring
date: 2014/12/20 00:00:00 
title: 最简单的Spring-mvc环境搭建
tags : spring
---

###Spring-MVC核心流程（doDispathch）
- 用户发送请求到前度控制器DispatchServlet，前端控制器收到请求后不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局流程控制

- DispathchServlet—》HandlerMapping，HandlerMapping将会把会把请求映射为HandlerExecutionChain对象（包含一个Handler处理器对象，多个HandlerInterceptor拦截器）对象，通过这种策略模式，很容易添加新的映射策略。核心代码： mapperHandler = getHander(processRequest,false);

- DispathchServlet —》HandlerAdapter，HanderAdapter将会把处理器包装为适配器。核心代码：HadlerAdapter ha = geetHandlerAdapter(mapperHandler.getHander()); 从而支持多种类型的处理器

- HandlerAdapter—》处理器（Controller）功能方法的调用，HandlerAdapter将会根据适配的结果调用真正的处理器功能处理方法，完成功能处理，并返回一个ModelAndView对象（包含模型数据和一个逻辑视图）核心代码：mv = ha.handler(processRequest,response,mappingHandler.getHander())

- ModelAndView的逻辑视图名—》ViewResolver，ViewResolver将吧逻辑视图名解析为具体的View，View会根据传过来的的模型数据进行渲染，此处的Model实际是一个map结构。通过这种策略模式很容易支持其他视图技术。核心代码  render(mv, processRequest,response).
返回授权给DispatchServlet, 由DispatchServlet返回给响应用户。


##项目说明

目的：

该项目通过运行一个最简单的Spring-mvc项目，来分析Spring-mvc的流程，尽量做到配置最少，为了更加了解运行过程，不使用注解，而是使用2.5时候的方式


根据上面的Spring-MVC核心流程，我们做如下配置

1. 在web中配置dispatch-servelt.xml
2. 在dispatch-servlet.xml配置HandlerMapping。我们用了默认的
BeanNameUrlHandlerMapping：表示将请求的URL和Bean名字映射，如URL为 “上下文/first”，则Spring配置文件必须有一个名字为“/first”的Bean，上下文默认忽略。（Spring默认提供该HandlerMapping，所以可以省略）
3. 在dispatch-servlet.xml配置HandlerAdapter：我们用了默认的SimpleControllerHandlerAdapter：表示所有实现了org.springframework.web.servlet.mvc.Controller接口的Bean可以作为Spring Web MVC中的处理器。如果需要其他类型的处理器可以通过实现HadlerAdapter来解决。（Spring默认提供，所以可以省略）
4. 新建控制器，并实现HandlerAdapter中的接口
5. 在disptch-servlet.xml中配置师徒解析器，我们使用默认的InternalResouceViewResolver。（Spring默认提供，所以可以省略）

综合起来，HandlerMapping和HandlerAdapter,ViewResolver不需要进行配置，



配置如下：


```pom.xml```中


{% highlight xml %}


  <!--加入该依赖，会自动增加aopalliance:1.0,commons-logging,spring-aop,spring-beans,spring-context,
        spring-core,spring-expression,spring-web,spring-webmvc,-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>


        <!--这个并不是必须的，但是Spring早起的控制器参数中需要-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>${javax.servlet.api}</version>
            <scope>provided</scope>
        </dependency>



{% endhighlight %}


在web.xml中配置DispatchServlet:


{% highlight xml %}


    <!--spring-mvc环境的DispatchServlet,必须配置，且名称必须为dispatcher-servlet.xml-->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>


{% endhighlight %}


处理器：

{% highlight java %}

package cn.liuyiyou.spring.simple.web;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Created with IntelliJ IDEA.
 * User: liuyiyou
 * Date: 14-7-29
 * Time: 下午2:19
 * 不使用注解，所以需要实现Controller接口
 */
public class ControllerTest implements Controller {

    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {

        ModelAndView mv = new ModelAndView();
        mv.setViewName("first.jsp");
        return mv;
    }
}


package cn.liuyiyou.spring.mvc.web;

/**
 * User: liuyiyou
 * Date: 14-12-20
 * Time: 下午8:37
 */
public class SecondController {


}


{% endhighlight %}



{% highlight xml %}


 <!--使用Spring默认提供的BeanNameUrlHandlerMapping，
 默认会将该class的类名去掉Controller并将首字母改成小写-->
 <bean name="/first" class="cn.liuyiyou.spring.mvc.web.FirstController" />

 <!--这个类是空的，-->
 <bean name="/second" class="cn.liuyiyou.spring.mvc.web.SecondController" />


{% endhighlight %}

在webapp目录下，新建first.jsp，second.jsp，注意，该文件是直接放在webapp下


分别访问：

http://localhost:8888/projectName/first   成功

http://localhost:8888/projectName/second  失败

> 信息: Mapped URL path [/first] onto handler '/first'

> 十二月 20, 2014 9:57:32 下午 org.springframework.web.servlet.handler.AbstractUrlHandlerMapping registerHandler

> 信息: Mapped URL path [/second] onto handler '/second'

可以看到两个请求都映射到了，但是第一个成功，而第二个失败。原因是第一个实现了Controller接口，第二个则是空的，没有对应的适配器，所以报错误

##Spring-MVC涉及到的主要组件

- HandlerMapping 处理映射器
- HandlerAdapter 处理器适配器，并调用处理器相关方法
- HandlerExceptionResolver 异常处理器
- ViewResolver 视图处理器
- LocalResolver 本地化解析器
- ThemeResolver 主题解析器
- MutipartResolver 文件上传解析器
- FlashMapManager

###简单说明一下

当发起一个http请求之后，由dispatchServlet接收，接收之后Spring如何知道这是一个解析该请求呢？这就需要handlerMapping呢，如上面的BeanNameUrlHandlerMapping把该请求解析，如first，则就需要一个FirstController类，编写该类之后，Spring又如何知道这是一个控制器呢？这就需要HandlerAdapter进行适配，如上面的SimpleControllerAdapter，原来需要该控制器实现Controller接口啊，那么我们就实现该接口，实现该接口之后，Spring就能在这里面处理业务逻辑，处理完之后，怎么在前台显示呢？这就需要视图解析器呢，如上面的InternalResouceViewResolver。然后我们写一个jsp，就可以显示视图了


之所以用了很少的配置就能运行spring-mvc程序的原因是因为Spring为我们提供了一套默认的组件，当初始化DispatchServlet的时候，Spring会自动扫描上下文的bean，根据名称或者类型查找自定义的组件，找不到是使用```org.springframework.web.servlet.jar```中```DispatchServlet.properties```中提供的默认组件。
