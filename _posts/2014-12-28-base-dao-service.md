---
layout: post
category : Spring
date: 2014/12/28 00:00:00 
title: 使用Spring-mybatis实现通用dao的两种实现方式
tags : spring
---


##说明

第一种：需要三个泛型，当前实体对象，主键，映射对象，

好处：具体Service实现类简单，如果自己没有特殊的方法，继承BaseServiceImpl之后不需要写任何东西

不好之处：

每个通用方法都需要传递映射参数：比如findById，如果是UserService调用，则需要这样写：userService.findById(1,UserMapper.class)，这样在BaseMapperImpl中才可以通过class.getName得到映射的名称

第三种：

这种是目前觉得最好的方法：

第一：不需要在方法上写各种泛型参数，同时也不需要在各种方法中传递映射参数

第二：通用dao也不需要写实现：mybatis-spring所倡导的的一致

不好之处：

需要在每个Service实现类中写上：

    @Autowired
    public void setBaseMapper(UserMapper userMapper){

        super.setBaseMapper(userMapper);
    }



