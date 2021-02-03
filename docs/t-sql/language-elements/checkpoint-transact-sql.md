---
description: CHECKPOINT (Transact-SQL)
title: CHECKPOINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHECKPOINT_TSQL
- CHECKPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], checkpoints
- automatic checkpoints
- writing dirty pages to disk
- pages [SQL Server], dirty
- checkpoints [SQL Server]
- CHECKPOINT statement
- log truncate mode [SQL Server]
- dirty pages
- logs [SQL Server], checkpoints
- manual checkpoints [SQL Server]
- pages [SQL Server], checkpoints
ms.assetid: ccdfc689-ad4e-44c0-83f7-0f2cfcfb6406
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 823bef4f26703b47404f28abb211cc87f70c0a04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208805"
---
# <a name="checkpoint-transact-sql"></a>CHECKPOINT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在您当前连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中生成一个手动检查点。  
  
> [!NOTE]  
>  有关不同类型的数据库检查点和常规检查点操作的信息，请参阅[数据库检查点 &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CHECKPOINT [ checkpoint_duration ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 checkpoint_duration  
 以秒为单位指定手动检查点完成所需的时间。 如果指定 checkpoint_duration，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 会在请求的持续时间内尝试执行检查点。 checkpoint_duration 必须是一个数据类型为 int 的表达式，并且必须大于零。 如果省略该参数，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将调整检查点持续时间，以便最大程度地降低对数据库应用程序性能的影响。 checkpoint_duration 选项是高级选项。  
  
## <a name="factors-affecting-the-duration-of-checkpoint-operations"></a>影响检查点操作持续时间的因素  
 通常，执行检查点操作所需的时间会随着该操作必须写入的脏页数的增加而增加。 默认情况下，为最大程度地降低对其他应用程序性能的影响，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将调整检查点操作执行写入的频率。 降低写频率将增加完成检查点操作所需的时间。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对手动检查点使用此策略，除非在 CHECKPOINT 命令中指定了 checkpoint_duration 值。  
  
 使用 checkpoint_duration 时对性能所造成的影响取决于脏页数、系统中的活动以及指定的实际持续时间。 例如，如果正常情况下完成检查点操作需要 120 秒，则将 checkpoint_duration 指定为 45 秒时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于检查点的资源将比默认情况下分配的资源多。 反之，将 checkpoint_duration 指定为 180 秒时，将导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配的资源将比默认情况下分配的资源少。 总之，checkpoint_duration 较短时，会增加用于检查点的资源，而 checkpoint_duration 较长时，会减少用于检查点的资源。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 总是尽可能地完成检查点操作，并且操作完成后，CHECKPOINT 语句将立即返回。 因此，完成检查点的时间有时比指定的持续时间短，有时则比指定的持续时间长。  
  
##  <a name="security"></a><a name="Security"></a> Security  
  
### <a name="permissions"></a>权限  
 CHECKPOINT 权限仅默认授予 **sysadmin** 固定服务器角色的成员以及 **db_owner** 和 **db_backupoperator** 固定数据库角色的成员，且不可转让。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [配置恢复间隔服务器配置选项](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)   
 [SHUTDOWN (Transact-SQL)](../../t-sql/language-elements/shutdown-transact-sql.md)  
  
  
