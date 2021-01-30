---
description: sp_restoredbreplication (Transact-SQL)
title: sp_restoredbreplication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_restoredbreplication
- sp_restoredbreplication_TSQL
helpviewer_keywords:
- sp_restoredbreplication
ms.assetid: a2c5ee32-e6d9-46e9-8031-8ff13c20acf7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 841a162c9619344fa297951277642f306d8539ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194376"
---
# <a name="sp_restoredbreplication-transact-sql"></a>sp_restoredbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  将数据库还原到非发起服务器、数据库或因其他原因而无法运行复制过程的系统时，删除复制设置。 如果将已复制数据库还原到执行备份的服务器或数据库以外的其他服务器或数据库，则将无法保留复制设置。 在还原时，服务器直接调用 **sp_restoredbreplication** ，以自动从还原的数据库删除复制元数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_restoredbreplication [ @srv_orig = ] 'original_server_name'  
        , [ @db_orig = ] 'original_database_name'  
    [ , [ @keep_replication = ] keep_replication ]  
    [ , [ @perform_upgrade = ] perform_upgrade ]  
```  
  
## <a name="arguments"></a>参数  
`[ @srv_orig = ] 'original_server_name'`  
 创建备份的服务器的名称。 *original_server_name* **sysname**，无默认值。  
  
`[ @db_orig = ] 'original_database_name'`  
 已备份数据库的名称。 *original_database_name* **sysname**，无默认值。  
  
`[ @keep_replication = ] keep_replication`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @perform_upgrade = ] perform_upgrade`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_restoredbreplication** 在所有类型的复制中使用。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 或 **dbcreator** 固定服务器角色的成员或 **dbo** 数据库架构才能 **sp_restoredbreplication** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
