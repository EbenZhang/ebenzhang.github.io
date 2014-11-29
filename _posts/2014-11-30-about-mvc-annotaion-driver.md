---
layout: post
category : Spring
date: 2014/11/30 00:00:00 
title: 关于mvc-annotation-driven实现原理
tags : spring
---

原文地址：<a href="http://liuyiyou.cn/posts/about-mvc-annotaion-driver/">查看原文</a>

##概述
mvc-annotation-driven的实现类是：org.springframework.web.servlet.config.AnnotationDrivenBeanDefinitionParser。需要注意的是在这个类不是public的，所以无法直接import，如果使用import的快捷键，会导入``` org.springframework.scheduling.config.AnnotationDrivenBeanDefinitionParser``` 这个类，不过可以使用command+鼠标找到该注解的说明，然后点击idea左上角的那个像靶子一样的东西快速定位到该类

通过parse方法主要向工厂注册了以下类：

1. RequestMappingHandlerMapping 

2. BeanNameUrlHandlerMapping

3. RequestMappingHandlerAdapter

4. HttpRequestHandlerAdapter

5. SimpleControllerHandlerAdapter

6. ExceptionHandlerExceptionResolver 

7. ResponseStatusExceptionResolver 

8. DefaultHandlerExceptionResolver 



##前提
1. 后台只配置了```<mvc-annotation-driver>```其他的没有配置

2. 返回的是String类型

使用Ajax通过@ResponseBody返回String数据，请求头和响应头如下所示：


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



注意：响应头中，返回的格式是：```Content-Type:text/plain;charset=ISO-8859-1```。如果这个时候，返回的是中文，则会出现乱码，原因是因为：

```AnnotationMethodHandlerAdapter```中，初始化的时候，直接初始化了```StringHttpMessageConverter```

{% highlight java %}


public AnnotationMethodHandlerAdapter() {
        // no restriction of HTTP methods by default
        super(false);

        // See SPR-7316
        //这三个是直接初始化的，其他几个则要看类路径中是否存在该依赖包
        StringHttpMessageConverter stringHttpMessageConverter = new StringHttpMessageConverter();
        stringHttpMessageConverter.setWriteAcceptCharset(false);
        this.messageConverters = new HttpMessageConverter[]{new ByteArrayHttpMessageConverter(), stringHttpMessageConverter,
                new SourceHttpMessageConverter(), new XmlAwareFormHttpMessageConverter()};
    }



{% endhighlight %}


而在```StringHttpMessageConverter```中，默认的字符集是ISO-8859-1。所以返回String的时候，是以iso-8859-1来返回


{% highlight java %}

//默认字符集
public static final Charset DEFAULT_CHARSET = Charset.forName("ISO-8859-1");

//初始化的时候使用的是默认字符集
public StringHttpMessageConverter() {
        this(DEFAULT_CHARSET);
    }

    public StringHttpMessageConverter(Charset defaultCharset) {
        super(new MediaType("text", "plain", defaultCharset), MediaType.ALL);
        this.defaultCharset = defaultCharset;
        this.availableCharsets = new ArrayList<Charset>(Charset.availableCharsets().values());
    }

{% endhighlight %}


所以，直接返回String的时候，是以iso-8859-1来返回的，如果这个时候返回的是中文，则会出现乱码。

这个时候，如果直接通过@ResponseBody，返回一个Map，则会报406异常，

可以看出，在这个类中，是没有针对json做处理的，那为何只要有jacskon依赖包，就能处理json呢？

这个主要是通过```org.springframework.web.servlet.config.AnnotationDrivenBeanDefinitionParser```来实现的。

再次说明一下：需要注意的是在这个类```不是public的，所以无法直接import```，如果使用import的快捷键，会导入``` org.springframework.scheduling.config.AnnotationDrivenBeanDefinitionParser``` ，


{% highlight java %}

    //已下几个都是在classpath中搜索依赖包是否存在
    private static final boolean jackson2Present =
            ClassUtils.isPresent("com.fasterxml.jackson.databind.ObjectMapper", AnnotationDrivenBeanDefinitionParser.class.getClassLoader()) &&
                    ClassUtils.isPresent("com.fasterxml.jackson.core.JsonGenerator", AnnotationDrivenBeanDefinitionParser.class.getClassLoader());

    private static final boolean jacksonPresent =
            ClassUtils.isPresent("org.codehaus.jackson.map.ObjectMapper", AnnotationDrivenBeanDefinitionParser.class.getClassLoader()) &&
                    ClassUtils.isPresent("org.codehaus.jackson.JsonGenerator", AnnotationDrivenBeanDefinitionParser.class.getClassLoader());


 private ManagedList<?> getMessageConverters(Element element, Object source, ParserContext parserContext) {
        //是否有开发人员自定义的message-convetes，如果有，则加入集合中
        
        Element convertersElement = DomUtils.getChildElementByTagName(element, "message-converters");
        ManagedList<? super Object> messageConverters = new ManagedList<Object>();
        if (convertersElement != null) {
            messageConverters.setSource(source);
            for (Element beanElement : DomUtils.getChildElementsByTagName(convertersElement, new String[] { "bean", "ref" })) {
                Object object = parserContext.getDelegate().parsePropertySubElement(beanElement, null);
                messageConverters.add(object);
            }
        }
        
        //如果没有自定义的message-converters，则：
        if (convertersElement == null || Boolean.valueOf(convertersElement.getAttribute("register-defaults"))) {
            messageConverters.setSource(source);
            messageConverters.add(createConverterBeanDefinition(ByteArrayHttpMessageConverter.class, source));

            //以下这几个是RequestMethodAdapter中已经注册了的。所以不需要判断，就直接加入
            RootBeanDefinition stringConverterDef = createConverterBeanDefinition(StringHttpMessageConverter.class, source);
            stringConverterDef.getPropertyValues().add("writeAcceptCharset", false);
            messageConverters.add(stringConverterDef);

            messageConverters.add(createConverterBeanDefinition(ResourceHttpMessageConverter.class, source));
            messageConverters.add(createConverterBeanDefinition(SourceHttpMessageConverter.class, source));
            messageConverters.add(createConverterBeanDefinition(AllEncompassingFormHttpMessageConverter.class, source));
           
            //如果这些需要的依赖包存在，则将以下的message-converters加入
            if (romePresent) {
                messageConverters.add(createConverterBeanDefinition(AtomFeedHttpMessageConverter.class, source));
                messageConverters.add(createConverterBeanDefinition(RssChannelHttpMessageConverter.class, source));
            }
            if (jaxb2Present) {
                messageConverters
                        .add(createConverterBeanDefinition(Jaxb2RootElementHttpMessageConverter.class, source));
            }
            if (jackson2Present) {
                messageConverters.add(createConverterBeanDefinition(MappingJackson2HttpMessageConverter.class, source));
            }
            else if (jacksonPresent) {
                messageConverters.add(createConverterBeanDefinition(MappingJacksonHttpMessageConverter.class, source));
            }
        }
        return messageConverters;
    }


{% endhighlight %}


所以，为什么只要加入```<mvc-annotation-driver>```和jakson包之后@ResponseBody就能支持直接返回json而不报406错误的原因。

针对中文乱码，主要有以下两种解决方案：

1. 控制器中加上``` @RequestMapping(value="/jsonString",produces="text/plain;charset=UTF-8")```后面即可。

这个需要验证：如果加上这句后，直接通过@ResponseBody返回Map的的时候，响应头是text/plain还是 application/json？

2. 在后台配置：

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
          class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter">

          <!--这个的重点不是中文乱码，而是解决ie下出现的下载问题，在ie8中，如果通过jquery.form.js提交mutipart/form表单，返回的json数据会不会alert出来，而是弹出一个下载框，原因是ie下，文件上传是通过一个iframe来进行的。返回的文件头是text/html，而这个和application/json冲突-->
            <property name="supportedMediaTypes">
                        <list>
                            <value>text/html;charset=UTF-8</value>
                        </list>
            </property>
    </bean>


{% endhighlight %}


第一种方案好地方是直观，而且不影响其他代码，一个例子是：在ie8中，如果是mutipart/form表单中使用文件上传，并使用ajaxsubmit提交，jquery.form.js会以iframe的形式提交表单，返回的是text/html。这个时候，响应请求会会弹出下载框，而其他地方则是以application/json返回。如果按照第二种方案修改，则其他地方会出现错误。所以这个时候第一种方案比较好

第二种的好处是，只需要修改一次，在其他地方不再需要修改。不过，如果只是如上配置，还是会有问题，因为该种配置只能返回application/json。在某些情况下可能会出现问题，不过只要在```mappingJacksonHttpMessageConverter```中配置和StringHttpMessageConverter的supportedMediaTypes即可解决问题

{% highlight xml %}

Request Headers
Accept:application/json, text/javascript, */*; q=0.01
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6
Connection:keep-alive
Content-Length:8
Content-Type:application/x-www-form-urlencoded

Response Headers
Content-Type:application/json;charset=UTF-8
Date:Sat, 29 Nov 2014 12:17:24 GMT
Server:Apache-Coyote/1.1
Transfer-Encoding:chunked


{% endhighlight %}

可以看出，返回的是application/json，而不是text/html，因为只有在返回String的时候，因为配置了messag-converters覆盖了默认的配置，才返回text/html。

至于出现406的原因：缺少jakson包，而返回的却是Map，Spring无法使用已有的messageConvertes无法解析json协议

在Spring3.2中，MappingJacksonHttpMessageConverter默认的编码不再是iso-8859-1而是UTF-8。也就是说，如果是使用Spring3.2的版本，在后台是不需要配置MappingJacksonHttpMessageConverter的。```（更正：不是不要配置，而是mvc-annotation-driven在扫描class的时候发现已经有了该依赖包，所以直接加mappingjacksonHttpMessageConvertes加入到messageConvertes中）```


最后一个问题：mvc-annotaiton-driven的简写方式和详细配置方式能一起使用吗？ 与使用顺序有关吗？


