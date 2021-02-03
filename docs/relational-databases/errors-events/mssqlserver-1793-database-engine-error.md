---
description: MSSQLSERVER_1793
title: MSSQLSERVER_1793 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
ms.assetid: 808db1d0-acc1-41d0-9287-8a5455001a02
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 44c44f6c32073bd8b93c35020dc3481867229ca8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196563"
---
# <a name="mssqlserver_1793"></a>MSSQLSERVER_1793
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|1793|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|FILESTREAM_BASEDATA_NEED_SAME_PARTITION|  
|消息正文|由于没有为 FILESTREAM 数据指定分区方案，因此无法删除索引“%.*ls”。|  
  
## <a name="explanation"></a>说明  
当你尝试在包含 FILESTREAM 数据的表上删除聚集索引，并且为基础数据指定了 **MOVE TO** 子句，但没有为 FILESTREAM 数据指定 **FILESTREAM_ON** 子句时，将出现此消息。  
  
## <a name="user-action"></a>用户操作  
在删除包含 FILESTREAM 数据的表上的聚集索引时，使用下列选项之一：  
  
-   为基础数据指定 **MOVE TO** 子句并且为 FILESTREAM 数据指定 **FILESTREAM_ON** 子句。  
  
-   不为基础数据指定 **MOVE TO** 子句，也不为 FILESTREAM 数据指定 **FILESTREAM_ON** 子句。  
  
下面的示例失败，因为为基础数据指定了分区方案，但没有为 FILESTREAM 数据指定。  
  
```Transact-SQL  
DROP INDEX [<clustered_index_name>] ON [<table_name>]   
WITH ( ONLINE = OFF, MOVE TO [PRIMARY] )  
GO  
```  
  
下面的示例成功，因为既为基础数据指定了 **MOVE TO** 子句，又为 FILESTREAM 数据指定了 **FILESTREAM_ON** 子句。  
  
```Transact-SQL  
DROP INDEX [<clustered_index_name>] ON [<table_name>]   
WITH ( ONLINE = OFF, MOVE TO [PRIMARY], filestream_on 'default' )  
GO  
```  
  
下面的示例也成功，因为既没有为基础数据指定 **MOVE TO** 子句，也没有为 FILESTREAM 数据指定 **FILESTREAM_ON** 子句。  
  
```Transact-SQL  
DROP INDEX [<clustered_index_name>] ON [<table_name>]   
WITH ( ONLINE = OFF )  
GO  
```  
  
