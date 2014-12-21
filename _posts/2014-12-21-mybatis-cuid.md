---
layout: post
category : mybatis
date: 2014/12/21 16:004:00 
title: Mybatis增删改查
tags : mybatis
---

##说明

这是一个最简单的mybatis增删改查，只包含两个部分（mybaitis-config除外），配置文件userMapper.xml和测试类，前面会单独的将增删改查拿出来，后面再会将总的程序放上。在最后，会说明，在增加，删除，更新的时候mybatis其实内部都是使用的update，所以这个时候具体怎么调用主要是看sql语句如何写的，与具体的调用方法和配置方法没有关系，叫这些名词只是为了方便理解，甚至我们可以使用session.delete()和 ```<delete id="insert" >```的组合来进入插入操作


##环境搭建：

参考： 

[Mybatis--环境搭建]({{ BASE_PATH }}/posts/start-mybatis/ "mybatis环境搭建")


##实体类：

```User.java```


{% highlight java %}


package cn.liuyiyou.mybatis.domain;

/**
 * User: liuyiyou
 * Date: 14-11-2
 * Time: 下午3:26
 */
public class User {

    private Integer id;
    private String name;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


{% endhighlight %}


##增加：

mybaitis配置文件：


{% highlight xml %}

 
 
    <!--增加-->
    <insert id="insertUser">
        insert into user (name) values (#{name})
    </insert>


{% endhighlight %}


在单元测试中：

{% highlight java %}


    /**
     * 增加测试
     */
    @Test
    public void testInsertUser() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            int result = session.insert("cn.liuyiyou.mybatis.mapper.UserMapper.insertUser", "lyy");
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }


{% endhighlight %}



##修改：
在配置文件中：


{% highlight xml %}

    <!--更新-->
    <update id="updateUser" >
        update user set name = #{name}  where id = #{id}
    </update>



{% endhighlight %}


在单元测试类中：


{% highlight java %}


    /**
     * 更新测试
     */
    @Test
    public void testUpdatetUser() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            User user = new User();
            user.setId(1);
            user.setName("liuyiyou");
            int result = session.update("cn.liuyiyou.mybatis.mapper.UserMapper.updateUser", user);
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }

    

{% endhighlight %}


##删除：
在配置文件中


{% highlight xml %}

    <!--删除-->
    <delete id="deleteUserById">
        delete from user  where id = #{id}
    </delete>


{% endhighlight %}


在单元测试类中：


{% highlight java %}

  /**
     * 删除测试
     */
    @Test
    public void testDeleteUserById() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            int result = session.delete("cn.liuyiyou.mybatis.mapper.UserMapper.deleteUserById", 9);
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }


{% endhighlight %}



##查询
在配置文件中

{% highlight xml %}

    <!--查询-->
    <select id="findById" resultType="User">
        select * from USER where id = #{id}
    </select>



{% endhighlight %}

在单元测试中：


{% highlight java %}

 

    /**
     * 查询测试
     */
    @Test
    public void testFindById() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            User user = (User) session.selectOne("cn.liuyiyou.mybatis.mapper.UserMapper.findById", 1);
            System.out.println(user.getName());
        } finally {
            session.close();
        }
    }



{% endhighlight %}


##总的程序：

```userMapper.xml```


{% highlight xml %}

 <?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 <mapper namespace="cn.liuyiyou.mybatis.mapper.UserMapper">

    <!--增加-->
    <insert id="insertUser">
        insert into user (name) values (#{name})
    </insert>


    <!--修改-->
    <update id="updateUser" >
        update user set name = #{name}  where id = #{id}
    </update>

    <!--查询-->
    <select id="findById" resultType="User">
        select * from USER where id = #{id}
    </select>


    <!--删除-->
    <delete id="deleteUserById">
        delete from user  where id = #{id}
    </delete>


 </mapper>


{% endhighlight %}



单元测试：


{% highlight java %}


 
package cn.liuyiyou.mybatis.mapper;

import cn.liuyiyou.mybatis.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

/**
 * User: liuyiyou
 * Date: 14-11-2
 * Time: 下午3:31
 */
public class UserMapperTest {


    private static SqlSessionFactory sqlSessionFactory;


    @Before
    public void setUp() throws Exception {
        String resource = "mybatis-config.xml";
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

    }


    /**
     * 增加测试
     */
    @Test
    public void testInsertUser() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            int result = session.insert("cn.liuyiyou.mybatis.mapper.UserMapper.insertUser", "lyy");
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }

 
   /**
     * 更新测试
     */
    @Test
    public void testUpdatetUser() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            User user = new User();
            user.setId(1);
            user.setName("liuyiyou");
            int result = session.update("cn.liuyiyou.mybatis.mapper.UserMapper.updateUser", user);
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }

   


    /**
     * 查询测试
     */
    @Test
    public void testFindById() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            User user = (User) session.selectOne("cn.liuyiyou.mybatis.mapper.UserMapper.findById", 1);
            System.out.println(user.getName());
        } finally {
            session.close();
        }
    }



    /**
     * 删除测试
     */
    @Test
    public void testDeleteUserById() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            int result = session.delete("cn.liuyiyou.mybatis.mapper.UserMapper.deleteUserById", 9);
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }

    

 
}

 

{% endhighlight %}


除了查询之外，增加，修改，删除都可以使用可以混用，关键在于sql语句

第一种：在代码中混用
可以使用```session.update() session.insert() session.delete()```。进行插入
此时在xml中使用```<insert>```

可以使用```session.update() session.insert() session.delete()```。进行修改
此时在xml中使用```<update>```

可以使用```session.update() session.insert() session.delete()```。进行删除
此时在xml中使用```<delete>```

第二种：在xml文件中使用
可以使用```<update >  <insert>  <delete>```，进行插入
此时在java中使用```session.insert()```

可以使用```<update >  <insert>  <delete>```，进行修改
此时在java中使用```session.update();```

可以使用```<update >  <insert>  <delete>```，进行删除
此时在java中使用```session.delete()```

第三种：混合使用
上面两种随意组合来完成插入，修改，删除操作


使用delte完成插入操作


{% highlight java %}

  @Test
    public void testInsertUser6() {

        SqlSession session = sqlSessionFactory.openSession();
        try {
            int result = session.delete("cn.liuyiyou.mybatis.mapper.UserMapper.insertUser3", "lyy");
            System.out.println(result);
            session.commit();
        } finally {
            session.close();
        }
    }

{% endhighlight %}


{% highlight java %}


    <delete id="insertUser3">
        insert into user (name) values (#{name})
    </delete>

{% endhighlight %}


##注意事项
1. 更新的时候注意参数，一般是实体类或者Map类型
2. session需要commit否则虽然成功，但是在数据库中并没有显示