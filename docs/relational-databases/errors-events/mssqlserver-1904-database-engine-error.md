---
description: MSSQLSERVER_1904
title: MSSQLSERVER_1904 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1904 (Database Engine error)
ms.assetid: 2a35d57d-74e2-45a2-8f67-3f2e51d69712
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 198fe8af27532157397b32875882a072f26f6f4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196473"
---
# <a name="mssqlserver_1904"></a>MSSQLSERVER_1904
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|1904|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|KEYCOUNT|  
|消息正文|表 '%.\*ls' 的 %S_MSG '%.*ls' 在 %S_MSG 键列表中具有 %d 个列名。 索引或统计信息键列列表的最大限制为 %d。|  
  
## <a name="explanation"></a>说明  
指定索引或统计信息的键列列表超出了允许的最大列数。  
  
## <a name="user-action"></a>用户操作  
修改键列的列表，使其中包含的列数小于指定的最大列数。  
  
对于非聚集索引，请考虑使用 CREATE INDEX 语句中的 INCLUDE 子句，将列作为非键列添加到索引中。 使用此方法可避免超过当前的索引大小限值（即最多 16 个键列）。 有关详细信息，请参阅 [Create Indexes with Included Columns](~/relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
## <a name="see-also"></a>另请参阅  
[CREATE INDEX (Transact-SQL)](~/t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS (Transact-SQL)](~/t-sql/statements/create-statistics-transact-sql.md)  
  
