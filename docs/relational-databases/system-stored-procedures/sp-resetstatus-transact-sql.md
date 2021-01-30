---
description: sp_resetstatus (Transact-SQL)
title: sp_resetstatus (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_resetstatus
- sp_resetstatus_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_resetstatus
ms.assetid: b892727f-ea3b-4b94-88d9-f2386ad4962c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ca7fa3d697fe5714066d677ba04b666817c597d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194384"
---
# <a name="sp_resetstatus-transact-sql"></a>sp_resetstatus (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  重置可疑数据库的状态。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_resetstatus [ @dbname = ] 'database'  
```  
  
## <a name="arguments"></a>参数  
 [ @dbname =] "*数据库*"  
 要重置的数据库的名称。 *数据库* 为 **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 sp_resetstatus 关闭数据库的可疑标志。 此过程更新 sys.databases 中的命名数据库的模式和状态列。 在运行此过程之前，应查阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志并解决所有问题。 执行 sp_resetstatus 后，停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，然后重新启动。  
  
 数据库可能由于某些原因而变得可疑。 可能的原因包括操作系统拒绝对数据库资源的访问，以及一个或多个数据库文件不可用或已损坏。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例重置 `AdventureWorks2012` 数据库的状态。  
  
```  
EXEC sp_resetstatus 'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
