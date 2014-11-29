---
layout: post
title:  MyBatis--配置文件
date:   2014/11/02 01:00:00 
category: mybatis
tag: mybatis
thumbnail: /images/mybatis2.png
---

##概述
之前，只是一个简单的MyBatis可运行程序，我们只是在配置文件中配置了一下数据源，和一个映射文件，但是如果仅仅是这样，那么针对每一个实体，都需要在配置文件中加一条mapper，在每一个mapper.xml中的返回值前面都需要加一条```cn.liuyiyou.mybatis.domain```这样就比较麻烦，所以，接下来记录一下mybatis配置文件

MyBatis 的配置文件包含了影响 MyBatis 行为甚深的设置（settings）和属性（properties）信息且
mybatis-config.xml中的配置文件需要一定的顺序，具体如下：如果顺序打乱可能会出现问题，


![Alt text]({{ BASE_PATH }}/images/mybatis2.png "mybatis目录")

##步骤

###properties
```mybatis-config.xml```，当然，像如下配置是错的，有的需要增加相对应的包，有的需要进行一定的处理，先记录在这里，到时候再整理一下，因为目前我对这些配置也不是十分了解

{% highlight xml %}


 <?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
 <configuration>

    <properties resource="jdbc.properties">
    </properties>
    <settings>
        <!--该配置影响的所有映射器中配置的缓存的全局开关。default:true-->
        <setting name="cacheEnabled" value="true"/>
        <!-- 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。
        特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态。 default:false-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 当启用时，带有延迟加载属性的对象的加载与否完全取决于对任意延迟属性的调用；反之，每种属性将会按需加载。 default:true-->
        <setting name="aggressiveLazyLoading" value="true"/>
        <!-- 是否允许单一语句返回多结果集（需要兼容驱动）。 default:true-->
        <setting name="multipleResultSetsEnabled" value="true"/>
        <!-- 使用列标签代替列名。不同的驱动在这方面会有不同的表现，
        具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 default:true-->
        <setting name="useColumnLabel" value="true"/>
        <!-- 允许 JDBC 支持自动生成主键，需要驱动兼容。如果设置为 true
        则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作（比如 Derby）。 default:false-->
        <setting name="useGeneratedKeys" value="false"/>
        <!--指定 MyBatis 是否以及如何自动映射指定的列到字段或属性。
        NONE 表示取消自动映射；
        PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。
        FULL 会自动映射任意复杂的结果集（包括嵌套和其他情况）。  default:PARTIAL-->
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        <!-- 配置默认的执行器。SIMPLE 就是普通的执行器；
        REUSE 执行器会重用预处理语句（prepared statements）；
        BATCH 执行器将重用语句并执行批量更新。 default:SIMPLE-->
        <setting name="defaultExecutorType" value="SIMPLE"/>
        <!--设置超时时间，它决定驱动等待数据库响应的秒数。  default:Not Set (null)-->
        <setting name="defaultStatementTimeout" value="25"/>
        <!-- 允许在嵌套语句中使用行分界（RowBounds）。 default:False-->
        <setting name="safeRowBoundsEnabled" value="false"/>
        <!-- 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 default:False-->
        <setting name="mapUnderscoreToCamelCase" value="false"/>
        <!--MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。默认值为 SESSION，
        这种情况下会缓存一个会话中执行的所有查询。
        若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据。  default:SESSION-->
        <setting name="localCacheScope" value="SESSION"/>
        <!--当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。
        某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。  default:OTHER-->
        <setting name="jdbcTypeForNull" value="OTHER"/>
        <!-- 指定哪个对象的方法触发一次延迟加载。 default:equals,clone,hashCode,toString-->
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
        <!-- 指定动态 SQL 生成的默认语言。 default:org.apache.ibatis.scripting.xmltags.XMLDynamicLanguageDriver-->
        <setting name="defaultScriptingLanguage" value="org.apache.ibatis.scripting.xmltags.XMLDynamicLanguageDriver"/>
        <!-- 指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法，
        这对于有 Map.keySet() 依赖或 null 值初始化的时候是有用的。注意原始类型（int、boolean等）
        是不能设置成 null 的。 default:equals,clone,hashCode,toString-->
        <setting name="callSettersOnNulls" value="equals,clone,hashCode,toString"/>
        <!-- 指定 MyBatis 增加到日志名称的前缀。。 default:false-->
        <setting name="logPrefix" value="equals,clone,hashCode,toString"/>
        <!-- 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。SLF4J | LOG4J | LOG4J2 | JDK_LOGGING |
         COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING。 default:Not set-->
        <setting name="logImpl" value="equals,clone,hashCode,toString"/>
        <!-- 为 Mybatis 用来创建具有延迟加载能力的对象设置代理工具。CGLIB | JAVASSIST。 default:CGLIB-->
        <setting name="proxyFactory" value="equals,clone,hashCode,toString"/>
    </settings>
    <typeAliases>
        <package name="cn.liuyiyou.mybatis.domain"/>
    </typeAliases>

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
        <!--   使用package之后，不需要针对每个实体类都写一个mapper了,不过包名需要与mapper在同一级目录下，
        如果在idea中，则要在resources中建立一个同名的包目录           -->
       <package name="cn.liuyiyou.mybatis.mapper"></package>
       <!--   使用package之后，不需要针对每个实体类都写一个mapper了，而且映射文件可以随便放   right
       <mapper resource="mapper/UserMapper.xml"/>                                    -->
        <!--   而使用resource,url,classes 则不行
        <mapper resource="mapper/*.xml"/>
         -->
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
    <!--因为配置文件中使用了typeAliases属性，所以不需要写cn.liuyiyou.mybatis.domain.前缀了-->
    <select id="findById" resultType="User">
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
在MyBatis配置文件中，其中的属性需要有一定的顺序，比如typeAliases，具体参考上图，如果将typeAliases放在mapper下面配置，则会报错```元素类型为 "configuration" 的内容必须匹配 "(properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,plugins?,environments?,databaseIdProvider?,mappers?)"。```所以，按照这个顺序去配置即可

