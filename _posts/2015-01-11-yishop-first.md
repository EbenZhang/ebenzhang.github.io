---
layout: post
category : yishop
date: 2015/01/11 00:00:00 
title: 广告Ad映射结构
tags : mybatis,java
---

##概述：
这个主要可以学到枚举类如何进行映射，以及关联查询

##映射枚举类

映射枚举类型主要有两种方式，一种是默认映射到数据库中的varchar类型，一种是映射到数据库中int类型

第一种：映射到数据库中的varchar类型。这种是默认的方式。如下所示：


```org.apache.ibatis.type.EnumOrdinalTypeHandler```。如何使用，可以参考下面的使用方式

第二种：映射到数据库中的int类型：

如下所示的Type字段。以数据库中的0代表第一个枚举类型，以此类推

##关联查询

关联查询主要的在两方面，一方面是resultMap如何定义。另外一方面是sql语句如何拼接。任何一点都不可以少：

具体参考代码：

{% highlight xml %}

  <resultMap id="BaseResultMap" type="cn.liuyiyou.yishop.domain.Ad">
        <id column="id" property="id" jdbcType="BIGINT" />
        <result column="create_date" property="createDate" jdbcType="TIMESTAMP" />
        <result column="modify_date" property="modifyDate" jdbcType="TIMESTAMP" />
        <result column="orders" property="orders" jdbcType="INTEGER" />
        <result column="begin_date" property="beginDate" jdbcType="TIMESTAMP" />
        <result column="end_date" property="endDate" jdbcType="TIMESTAMP" />
        <result column="path" property="path" jdbcType="VARCHAR" />
        <result column="title" property="title" jdbcType="VARCHAR" />
        <!-- 这是枚举类型，并将字符串映射成数据库中的整形字段的时候使用这个 -->
        <result column="type" property="type"
            typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler" />
        <result column="url" property="url" jdbcType="VARCHAR" />
        <!-- 这里改成直接使用cn.liuyiyou.yishop.mapper.AdPositionMapper.BaseResultMap可以吗？可以 -->
        <association property="adPosition"
            resultMap="cn.liuyiyou.yishop.mapper.AdPositionMapper.BaseResultMap">
        </association>
        <!-- <association property="adPosition"
            javaType="cn.liuyiyou.yishop.domain.AdPosition">
            <id column="id" jdbcType="BIGINT" property="id" />
            <result column="create_date" jdbcType="TIMESTAMP" property="createDate" />
            <result column="modify_date" jdbcType="TIMESTAMP" property="modifyDate" />
            <result column="description" jdbcType="VARCHAR" property="description" />
            <result column="height" jdbcType="INTEGER" property="height" />
            <result column="name" jdbcType="VARCHAR" property="name" />
            <result column="width" jdbcType="INTEGER" property="width" />
        </association> -->
    </resultMap>


{% endhighlight %}



测试代码：

{% highlight java %}

/**
     *虽然不为空，但是实际上里面是没有数据的，因为该sql语句并没有包含adPosition字段,
     *即使包含了，也无法得到。因为主要是看sql语句
     */
    @Test
    public void selectByPrimaryKeyTest() {
        Ad ad = adMapper.selectByPrimaryKey(1l);
        Assert.assertNotNull(ad.getAdPosition());
        Assert.assertEquals(Ad.Type.image, ad.getType());
    }

    /**
     * 这个能得到，因为sql语句是连接查询。但是这里有一个问题：如果两个关联的表有同名字段，如何得到该这些相同字段的内容？
     */
    @Test
    public void selectAdPositionByAdIdTest(){
        Ad ad = adMapper.selectAdPositionByAdId(1l);
        Assert.assertNotNull(ad.getAdPosition());
        Assert.assertEquals("头部广告", ad.getAdPosition().getName());
    }

{% endhighlight %}


##遗留问题：

如果相关联的两个表有相同的字段名，如何使这两个不冲突？

第一种：重新修改字段名，比如一个name的字段，可以加上类名前缀叫做projctName。这样的好处是不需要重新定义resultMap，但是只适合前期。

第二种：重新定义一个新的ResultMap，这样的话不需要改动持久化类的字段名，但是需要重新定义一个非实体类。


