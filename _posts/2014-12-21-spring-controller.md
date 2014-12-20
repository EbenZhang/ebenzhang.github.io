---
layout: post
category : Spring
date: 2014/12/21 00:00:00 
title: Spring控制器
tags : spring
---


##说明

Spring的Controller接口仅仅定义了一个方法用于负责处理客户请求，并返回适当的模型和视图，这也是所有控制器都需要完成的职责。ModelAndView与Controller，这便是Spring MVC框架实现的基础。尽管Controller相当抽象，但Spring提供了多种Controller接口实现类。



<img src="{{ BASE_PATH }}/images/spring-controller.png">

上面这张图是基于Spring3.2版本的，那些已经废弃的控制器，就不考虑了。同时，还少了顶级接口Controller。

第一个：顶级的Controller接口

{% highlight java %}

public interface Controller {

    /**
     * Process the request and return a ModelAndView object which the DispatcherServlet
     * will render. A <code>null</code> return value is not an error: It indicates that
     * this object completed request processing itself, thus there is no ModelAndView
     * to render.
     */
    ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception;

}

{% endhighlight %}

翻译一下：处理请求并返回一个ModelAndView对象，该对象将被DispatchServlet进行渲染（也就是把模型数据渲染到视图（如jsp）上，如果返回null并不是错误，而是说明该对象已经处理完成了请求，只是没有视图进行渲染而已）


第二个：AbstractController

用到了模板方法设计模式

第三个：AbstractUrlViewController

第四个：MultiActionController

第五个：ParamterzableViewController

第六个：ServletForwardingController

第七个：ServletWrappingController

第七个：UrlFilenameViewController

被废弃的：

AbstractFormController 在3.0中过时，用注解控制器取代

AbstractWizardFormController

BaseCommondContrller

SimpleFormController


