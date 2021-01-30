---
description: sysmail_start_sp (Transact-SQL)
title: sysmail_start_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_start_sp
- sysmail_start_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_start_sp
ms.assetid: 25fd7bb6-cfdd-463f-bea8-c6fcb805d3f5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c3685409d82c2c1591ae129b93c4c3036f5925c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181902"
---
# <a name="sysmail_start_sp-transact-sql"></a>sysmail_start_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  通过启动外部程序使用的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对象来启动数据库邮件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_start_sp  
```  
  
## <a name="arguments"></a>参数  
 无  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 数据库邮件未启用或未随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一起安装。 使用数据库邮件配置向导可以启用并安装数据库邮件对象。  
  
 此存储过程位于 **msdb** 数据库中。 该存储过程将启动保存待发消息请求的数据库邮件队列，并对外部程序启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 激活。  
  
 此队列启动后，数据库邮件外部程序即可处理消息。 使用此过程，可以在使用 **sysmail_stop_sp** 存储过程停止队列后重新启动队列。  
  
> [!NOTE]  
>  此存储过程只启动数据库邮件的队列。 它不会激活数据库中的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息传递功能。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何在 **msdb** 数据库中开始数据库邮件。 该示例假设数据库邮件已启用。  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_start_sp ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件 Xp 服务器配置选项](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)   
 [sysmail_stop_sp &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sysmail-stop-sp-transact-sql.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
