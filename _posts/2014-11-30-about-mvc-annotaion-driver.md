---
layout: post
category : skill
date: 2014/11/30 00:00:00 
title: 关于mvc-annotation-driver注解--未整理
tags : spring
---

##概述

Spring是支持直接返回json数据的，只需要在控制器方法中加上@ResponseBody注解，在后台加上```<mvc:annotation-driven />```即可

使用Ajax返回数据，请求头和响应头如下所示：


{% highlight xml %}


Request Headers
Accept:*/*
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6
Connection:keep-alive
Content-Length:23
Content-Type:application/x-www-form-urlencoded


Response Headers
Content-Length:2
Content-Type:text/plain;charset=ISO-8859-1
Date:Sat, 29 Nov 2014 11:15:22 GMT
Server:Apache-Coyote/1.1


{% endhighlight %}


##前提
后台只配置了```<mvc-annotation-driver>```其他的没有配置

注意：响应头中，返回的格式是：```Content-Type:text/plain;charset=ISO-8859-1```。如果这个时候，返回的是中文，则会出现乱码



网上的资料千遍一律的解决方案是：

事实上，可以后台xml文件中，不加任何配置就可以解决该问题，只需要在控制器中加上``` @RequestMapping(value="/jsonString",produces="text/plain;charset=UTF-8")```后面即可。

但是，这个解决方案有不好的地方：如果返回的是String还好，如果返回的是map，则可能会报406错误。


{% highlight xml %}


 <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping">

    </bean>

    <bean
            class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean
                        class="org.springframework.http.converter.StringHttpMessageConverter">
                    <property name="supportedMediaTypes">
                        <list>
                            <value>text/html;charset=UTF-8</value>
                        </list>
                    </property>
                </bean>
                <ref bean="mappingJacksonHttpMessageConverter"/>
                <!-- json转换器 -->
            </list>
        </property>

    </bean>

    <bean id="mappingJacksonHttpMessageConverter"
          class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"/>


{% endhighlight %}


{% highlight xml %}


Accept:application/json, text/javascript, */*; q=0.01
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6
Connection:keep-alive
Content-Length:8
Content-Type:application/x-www-form-urlencoded

Content-Type:application/json;charset=UTF-8
Date:Sat, 29 Nov 2014 12:17:24 GMT
Server:Apache-Coyote/1.1
Transfer-Encoding:chunked


{% endhighlight %}


出现406的原因：缺少jakson包

在Spring3.2中，MappingJacksonHttpMessageConverter默认的编码不再是iso-8859-1而是UTF-8。也就是说，如果是使用Spring3.2的版本，在后台是不需要配置MappingJacksonHttpMessageConverter的。

是：StringHttpMessageConverter默认是iso-8859-1

最后一个问题：mvc-annotaiton-driver和 那两个类，可以交换顺序吗？
