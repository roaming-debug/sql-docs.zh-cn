---
description: MSSQLSERVER_2530
title: MSSQLSERVER_2530 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
ms.assetid: 5d4be07a-38a5-4b25-819c-4dcb4636cc15
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 49e0a196a0ce17a5c95c495ca5feaab9d9889d6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188964"
---
# <a name="mssqlserver_2530"></a>MSSQLSERVER_2530
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|2530|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC_INDEX_IS_OFFLINE|  
|消息正文|表 "%.\*ls" 的索引 "%.*ls" 已禁用。|  
  
## <a name="explanation"></a>说明  
DBCC 语句无法继续，因为指定的索引已禁用。 索引被禁用后一直保持禁用状态，直到它重新生成或删除并重新创建。  
  
## <a name="user-action"></a>用户操作  
  
1.  可以通过使用下列方法之一来启用已禁用的索引：  
  
    -   带 REBUILD 子句的 ALTER INDEX 语句  
  
    -   带 DROP_EXISTING 子句的 CREATE INDEX  
  
    -   DBCC DBREINDEX  
  
2.  请重新运行 DBCC 语句。  
  
## <a name="see-also"></a>另请参阅  
[启用索引和约束](~/relational-databases/indexes/enable-indexes-and-constraints.md)  
[ALTER INDEX (Transact-SQL)](~/t-sql/statements/alter-index-transact-sql.md)  
[CREATE INDEX (Transact-SQL)](~/t-sql/statements/create-index-transact-sql.md)  
[DBCC DBREINDEX (Transact-SQL)](~/t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)  
  
