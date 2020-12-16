---
description: MSSQL_ENG003165
title: MSSQL_ENG003165 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG003165 error
ms.assetid: 707d33dd-644e-4cc9-ac51-dddd49031530
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 778661a1ff29f8813a000606d4d49be3c7105643
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475828"
---
# <a name="mssql_eng003165"></a>MSSQL_ENG003165
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|3165|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|数据库 '%ls' 已还原，但在还原/删除复制时出错。 该数据库仍保留为脱机状态。 请参阅 SQL Server 联机丛书中的主题 MSSQL_ENG003165。|  
  
## <a name="explanation"></a>说明  
 如果在还原已复制数据库的备份时出现问题，将引发此错误：  
  
-   如果将备份还原到对其创建备份的同一数据库和服务器，此错误将指示无法正确还原复制设置。  
  
-   如果将备份还原到其他数据库或服务器，此错误将指示无法正确删除复制设置（默认情况下，如果是在其他数据库或服务器上，则删除复制设置）。  
  
 该错误可能是由已还原数据库的状态与一个或多个包含复制元数据的系统数据库 **msdb**、 **master** 或分发数据库的状态不匹配造成的。  
  
## <a name="user-action"></a>用户操作  
 若要解决此问题，请执行下列操作：  
  
1.  执行 ALTER DATABASE 以使数据库联机；例如： `ALTER DATABASE AdventureWorks SET ONLINE`。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。 如果要保留复制设置，请转到步骤 2。 否则，转到步骤 3。  
  
2.  执行 [sp_restoredbreplication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-restoredbreplication-transact-sql.md)。 如果此存储过程成功执行，则还原完成。 如果此存储过程未成功执行，请转到步骤 3。  
  
3.  执行[sp_removedbreplication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) 以删除所有复制设置。  
  
     如果需要，请重新配置复制。 如果您根据建议将复制拓扑编写了脚本，请使用脚本来重新配置该拓扑。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
