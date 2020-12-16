---
description: MSSQL_REPL020011
title: MSSQL_REPL020011 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_REPL020011 error
ms.assetid: f72072d7-bbb6-48ad-ac88-afa74aeb4d58
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3bf143f39b38decc7a51a7eb71385b0cf4684360
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432209"
---
# <a name="mssql_repl020011"></a>MSSQL_REPL020011
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|20011|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|进程无法在“%2”上执行“%1”。|  
  
## <a name="explanation"></a>说明  
 事务复制处理过程中的许多情况都可能引发此错误，例如日志读取器代理执行 **sp_replcmds**（此过程无法在 \<ServerName> 上执行“sp_replcmds”）或 **sp_repldone**（此过程无法在 \<ServerName> 上执行“sp_repldone”）时。  
  
## <a name="user-action"></a>用户操作  
 如果在您刚刚从备份中还原的数据库中出现此错误，请确保遵守了备份和还原文档中列出的步骤，包括在适合时执行 **sp_replrestart** 。 有关详细信息，请参阅 [快照复制和事务复制的备份和还原策略](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)。  
  
 此错误是一个内部处理错误，如果在还原之外的环境中出现此错误，则通常表示必须删除或重新配置复制。 如果无法删除复制，请与客户支持部门联系以获取帮助。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [sp_replcmds (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)  
  
  
