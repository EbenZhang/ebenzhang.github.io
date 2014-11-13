---
layout: post
category : tools
date: 2014/11/14 00:00:00 
title: Idea的优点
tags : idea
---

###概述
虽然工作中用的还是卡得要死，慢得要死，各种坑爹插件的myeclipse，但是学习中还是用idea，所以我打算总结出一个ieda的一些优点，希望有一天工作中也能用上

###牛逼的提示功能
在如今，使用vim或者记事本编程的人不知道还存不存在，但是使用java的人肯定是不敢这么搞的

####jQuery提示
不需要自己去下载插件，idea就能给你友好的提示，这一点甚至比Sublime还要出色。
更厉害的是，idea还能找到jQuery的插件。

比如我现的文件中有一个jQuery.form.js。那么我在自己的js中文件中这样写

```

$(document).ready(function(){
    $("#myForm").a
});

```

会提示ajaxSubmit


####路径提示
比如，在jsp的form的action中，需要写上路径，如果使用Springmvc，idea会自动搜到你所有的@RequestMapping中的映射


####js/css提示
如果要引入某个js文件或者css文件，ieda会自动搜索，此外，在前端写css的时候，ieda会自动提示项目中的css，包括class  ，id 等



###部署：
###tomcat

两种方式，第一种是运行，这种情况下，项目不会部署到tomcat的webapp下，只是依赖tomcat。这点类似于eclipse。  在部署的时候，不选择war

另外一种类似myeclipse，部署后会在tomcat的webapp下

可以实现类热部署，启动完成之后，jsp或者js文件修改之后，可以uploade resources。java文件修改之后可以 uplodad resources and classres






