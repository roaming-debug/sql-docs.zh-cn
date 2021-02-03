---
description: MSSQL_ENG003724
title: MSSQL_ENG003724 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG003724 error
ms.assetid: 10cb119d-92df-4124-b85d-cd2f2666c99c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ba60ce4e15ce700a08b417e513638dcb47785282
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198943"
---
# <a name="mssql_eng003724"></a>MSSQL_ENG003724
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|3724|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|无法对 %S_MSG '%.*ls' 执行 %S_MSG，因为它正用于复制。|  
  
## <a name="explanation"></a>说明  
 数据库中的对象经复制后，通常会在系统表 **sysarticles** （用于快照和事务发布）或 **sysmergearticles** （用于合并发布）中标记为已复制。 尝试删除复制的对象时，会引发此错误。  
  
## <a name="user-action"></a>用户操作  
 尝试删除对象之前，请确保数据库对象未经复制。 例如：  
  
-   如果此错误发生在发布数据库中，则先从发布中删除项目，然后再删除对象。 有关详细信息，请参阅[向现有发布添加项目和从中删除项目](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
-   如果此错误发生在订阅数据库中，则先删除订阅，然后再删除对象。 有关详细信息，请参阅[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)。 对于事务发布的订阅，可以删除单个项目的订阅，而不用删除整个发布。 有关详细信息，请参阅 [sp_dropsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)。  
  
 如果此错误发生在未复制的数据库中，请执行 [sp_removedbreplication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)，以确保此数据库中的对象不会标记为已复制。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
