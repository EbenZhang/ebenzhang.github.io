---
layout: post
title:  MyBatis--环境搭建
date:   2014/11/02 00:00:00 
category: mybatis
tag: mybatis
thumbnail: images/mybatis1.png
---

##概述
之前就使用过MyBatis，但是却没有系统学习过，只是知道是怎么一回事就可以了，之前一直是使用jpa，但是说实话，对jpa用得不是很熟练，特别是映射关系的配置总是记忆不住。而新公司用的是Mybatis，所以打算系统的学习了解一下MyBatis。

Mybatis是什么，干嘛的，这些就不说了，参考资料主要是MyBatis官方文档，能写多少不知道，我对我自己的文笔以及陈述能力总是力不从心，还好主要是我自己看，所以写得再烂也没关系，总好过网上千遍一律的抄袭和转载，可怜的，往往原创还要在转载后面，鄙视百度！

##软件环境
之前一直是使用Spring-MyBatis，不过这次是重新学习，所以先把Spring这个万能粘合剂抛到一边，数据库：MySQL，IDE：Idea，就这两个就可以了，至于客户端什么的不需要



###目录设置

![Alt text]({{ BASE_PATH }}/images/mybatis1.png "mybatis目录")

##步骤

###配置文件
```mybatis-config.xml```


{% highlight xml %}


 <?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
 <configuration>

    <properties resource="jdbc.properties">
    </properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
 </configuration>


{% endhighlight %}


```UserMapper.xml```


{% highlight xml %}


 <?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 <mapper namespace="cn.liuyiyou.mybatis.mapper.UserMapper">
    <select id="findById" resultType="cn.liuyiyou.mybatis.domain.User">
      select * from USER where id = #{id}
    </select>
 </mapper>


{% endhighlight %}


```UserMapper.java```


{% highlight java %}


package cn.liuyiyou.mybatis.mapper;

import cn.liuyiyou.mybatis.domain.User;

/**
 * User: liuyiyou
 * Date: 14-11-2
 * Time: 下午3:28
 */
public interface UserMapper {

    public User findById(Integer id);
}

{% endhighlight %}



##特别注意：
在idea中，如果xml映射文件放在和Mapper同级下可能会出现错误，报找不到资源文件错误，这是idea编辑决定的，通过看编译后的文件会发现，编译的时候只编译resources中的xml文件，所以最好放到resources下


