---
description: MSSQL_ENG014152
title: MSSQL_ENG014152 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014152 error
ms.assetid: 4215e2b1-cd30-441f-9671-9afc742adf6e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0087219c660270afc75623cff7024dda1957b0e1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463208"
---
# <a name="mssql_eng014152"></a>MSSQL_ENG014152
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14152|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|复制 - %s：代理 %s 计划重试。 %s|  
  
## <a name="explanation"></a>说明  
 指定的复制代理已计划重试请求的操作。 进程继续重试，直到达到配置的最大作业步骤重试次数。  
  
 通常由于下列原因之一发生重试：  
  
-   超过了 **QueryTimeout** 值。  
  
-   超过了 **LoginTimeout** 值。  
  
-   将复制进程选为了死锁牺牲品。  
  
## <a name="user-action"></a>用户操作  
 如果重试消息很少出现，则不需要用户操作。  
  
 使用 [sp_help_jobstep](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md) 查看指定复制代理将要重试的“运行代理”  步骤的当前最大尝试次数设置。 可使用 [sp_update_jobstep](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md) 存储过程的 `@retry_attempts` 参数来调整作业步骤重试的次数。  
  
 如果重试消息频繁重复出现，则应根据错误消息排除导致重试的问题。 对于指示为何必须计划重试的消息，检查代理的历史记录。 某些情况下，您可能需要对复制代理启用更详细的日志记录。 有关如何配置复制日志记录的详细信息，请参阅 Microsoft 知识库文章 [312292](https://support.microsoft.com/kb/312292)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
