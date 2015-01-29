
---
layout: post
title: Use BCP to Backup/Restore Table
category : Database
tags : [Database]
original: true
---


##导出

    bcp YourDBName.dbo.YourTableName out YourOutputFilePath -U YourUserName -P YourPassword -S YourServer'sHostName -c

##导入

    bcp YourDBName.dbo.YourTableName in YourDatabaseTableFilePath -U YourUserName -P YourPassword -S YourServer'sHostName -c

详细参数参考 [这里](https://msdn.microsoft.com/en-au/library/ms162802.aspx)
