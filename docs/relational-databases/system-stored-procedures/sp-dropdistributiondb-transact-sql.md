---
title: sp_dropdistributiondb (Transact-sql) |Microsoft Docs
description: 如果一个分发数据库和其他数据库未使用该数据库，则删除该数据库所使用的文件。 此存储过程在分发服务器上的任何数据库上运行。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistributiondb_TSQL
- sp_dropdistributiondb
helpviewer_keywords:
- sp_dropdistributiondb
ms.assetid: b6dd1846-2259-4d29-93af-a70a5d25a0c5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2de544361c4bcd9befb80e5172e0b4887a984a9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191197"
---
# <a name="sp_dropdistributiondb-transact-sql"></a>sp_dropdistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  删除分发数据库。 如果数据库使用的物理文件未由另一个数据库使用，则删除这些文件。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropdistributiondb [ @database= ] 'database'  
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database'` 要删除的数据库。 *数据库* 为 **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropdistributiondb** 在所有类型的复制中使用。  
  
 在删除分发服务器之前，必须通过执行 **sp_dropdistributor** 来执行此存储过程。  
  
 **sp_dropdistributiondb** 还会删除分发数据库的队列读取器代理作业（如果存在）。  
  
 要禁用分发，分发数据库必须联机。 如果存在分发数据库的数据库快照，必须在禁用分发之前删除该快照。 数据库快照是数据库的只读脱机副本，与复制快照无关。 有关详细信息，请参阅[数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributiondb-tr_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_dropdistributiondb** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributiondb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_helpdistributiondb (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
