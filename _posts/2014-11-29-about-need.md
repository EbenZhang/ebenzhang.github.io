---
layout: post
category : skill
date: 2014/11/29 00:00:00 
title: 稳定需求的重要性
tags : skill
---

##概述
在需求分析，项目架构，代码涉及中，需求总是千变万化的，这是一件很正常的事情，但是因为人与人的沟通中，非开发人员和业务人员的看待需求的眼光不一致，所以导致需求的频繁改动还是会带来不好的效果，花费了脑力，财力和资源，达到的效果可能南辕北辙，所以想写点什么

在《代码大全》中，对于需求有如下说明：

> 在大型项目中，如果在架构阶段检测到需求错误，那么修复他的成本通常是“在需求阶段检测并修复该错误”的3倍。如果在编码阶段检测到需求错误，那么修复他的成本是5倍到10倍；在系统测试阶段，成本是10倍，在发布之后，成本陡增为10倍到100倍



当然，要保证需求一成不变是不可能的，同时在《代码大全》中也有如下方式来解决需求变更：

1. 确保每一个人都知道需求变更的代价---不知道业务人员是否需要了解
2. 建立一套变更控制程序--就是增对每次变更（稍微大的）都要有一个更改方案
3. 使用能适用变更的开发方法--就是程序员的事情
4. 放弃这个项目--这个不是开发人员能决定的
5. 注意项目的商业案例
6. 建立需求核对表

##下面是一张需求核对表

###针对功能需求

- 是否详细定义了系统的全部输入，包括其来源，精度，取值范围，出现频率等
- 是否详细定义了系统的全部输出，包括目的地，精度，取值范围，出现频率等
- 是否定义了所有的输出格式（web，报表）
- 是否定义了所有硬件及软件的外部接口
- 是否详细定义了全部外部的接口通信，包括握手协议，纠错协议，通信协议等
- 是否列出了用户想要做的全部事情
- 是否详细定义了每个任务所用的数据，以及每个任务得到的数据

###针对非功能需求

- 是否为全部的必要操作，从用户的角度，详细描述了期望响应时间
- 是否详细描述了其他与计时相关的考虑，例如时间处理，数据传输率，系统吞吐量
- 是否详细定义了安全级别
-是否详细定义了可靠性，包括软件失灵的后果，发送故障时需要保护的信息，错误监测与恢复的策略
- 是否详细定义了机器内存和剩余磁盘空间的最小值
- 是否详细定义了系统的可维护性，包括适用特定功能的变更，操作环境的变更，与其他软件的接口的变更能力
- 是否包括对成功的定义和失败的定义

粗看一下啊，基本只做到了```是否列出了用户想要做的全部事情```不过还是可以参考的

##我的一些看法

##一：前端插件的选择

1.1：jQuery的版本选择需要注意，虽说jQuery是向下兼容的，但是又的时候可能会出现两个插件会依赖不同的版本，所以jQuery版本不是越高越好，对于jQuery牛人来说没什么，但是对大部分编码人员来说会是一个比较严重的问题。而且很多功能其实用不着。

1.2：前端css是否需要使用现在比较流行的？如bootstrap，因为如果在后面再确定是否使用bootstrap会发现根本不可行。但是现在很多的前端插件css是基于bootstrap的。比如jquery.fileupload

##二：jdk版本的选择

JDK版本号要统一且最好使用1.6

原因：现在jdk8都已经出来了，选择1.6是不是已经落伍？对经验丰富的架构师来说，选择高版本没有什么问题，但是对一般公司或者企业来说却没有那么简单。框架，打包工具等对有的并没有支持高版本，比如最eclipse默认的编译器还是1.5，你搞个@override还给你报个错,比如你使用1.7的环境，在编写的时候用switch的str没什么问题，但是在用maven的时候可能会出现编译异常，虽然说可以解决，但是还是浪费时间。

##三：前端的统一提交方式

表单的提交方式是直接使用form表单提交还是使用ajax提交还是使用jQery.form的ajaxSubmit提交 如果直接使用表单提交，那么后端验证无法友好的返回到前端，如果是使用ajax提交，则最好在最开始的时候，在spring中配置json返回方式。不然前期前端返回是application/json。在某些情况下必须要返回text/html的时候，会在ie下发送请求的时候，会弹出下载框

##四：统一的前端验证

现在系统中很多地方都需要验证，因为没有统一的验证方式（其实有，但是没用，因为不具有统一性）所以很多时候需要验证的时候都是自己写的，而且这样的代码会造成很多重复的代码。建议使用jquery.validator

##五：统一的数据返回格式

当然，这个只是建议，有的时候返回实体类，再通过el表达式会比较快，但是很多时候json格式的返回数据会比较好

##六：表之间是否需要外键关联？
     
这个问题之前在网上有过很多讨论（http://www.zhihu.com/question/19600081），两方都有道理。不过如果你们公司有一个牛逼的dba但是编码人员水平不一，那就用外键吧！如果没有牛逼的dba，但是有牛逼的编码人员，那就不用外键，在业务层处理。我们项目没有牛逼dba，所以没有用外键，但是也没有牛逼的编码人员，结果初期不明显，到后面处处是坑。无处不在的空指针异常，百分之九十九是因为数据问题引起的，比如你删了一个产品，结果与产品相关的订单啊，退款啊什么的却没有删除，一旦你查询订单列表或者退款列表，必报空指针异常。改动起来超级麻烦（那不是一个人的事）

##七：框架的选择

现在除了主流的ssmh外，还有很多其他框架，所以在项目过程中需要注意，如果对一个框架不是非常了解，最好还是不用，不然出问题了很难解决。像ssmh网上有很多资料可以查阅，但是其他的不是那么容易了。




