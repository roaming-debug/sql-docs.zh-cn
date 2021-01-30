---
description: sp_helpdatatypemap (Transact-SQL)
title: sp_helpdatatypemap (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdatatypemap
- sp_helpdatatypemap_TSQL
helpviewer_keywords:
- sp_helpdatatypemap
ms.assetid: 800c9c65-723e-4961-a63d-327987f129f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 46750f82e08a9a3dcb936320614390bc6f359c4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176445"
---
# <a name="sp_helpdatatypemap-transact-sql"></a>sp_helpdatatypemap (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回有关在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库管理系统 (DBMS) 之间定义的数据类型映射的信息。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdatatypemap [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @defaults_only = ] defaults_only ]  
```  
  
## <a name="arguments"></a>参数  
`[ @source_dbms = ] 'source_dbms'` 要从中映射数据类型的 DBMS 的名称。 *source_dbms* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|源为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|源为 Oracle 数据库。|  
  
`[ @source_version = ] 'source_version'` 源 DBMS 的产品版本。 *source_version* 的数据类型为 **varchar (10)**，并且如果未指定，则返回源 DBMS 的所有版本的数据类型映射。 启用结果集按 DBMS 源版本进行筛选。  
  
`[ @source_type = ] 'source_type'` 源 DBMS 中列出的数据类型。 *source_type* 为 **sysname**，如果未指定，则返回源 DBMS 中所有数据类型的映射。 启用结果集按源 DBMS 中的数据类型进行筛选。  
  
`[ @destination_dbms = ] 'destination_dbms'` 目标 DBMS 的名称。 *destination_dbms* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|目标为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|目标为 Oracle 数据库。|  
|**DB2**|目标为 IBM DB2 数据库。|  
|**SYBASE**|目标为 Sybase 数据库。|  
  
`[ @destination_version = ] 'destination_version'` 目标 DBMS 的产品版本。 *destination_version* 是 **varchar (10)**，如果未指定，则返回目标 DBMS 的所有版本的映射。 启用结果集按 DBMS 的目标版本进行筛选。  
  
`[ @destination_type = ] 'destination_type'` 目标 DBMS 中列出的数据类型。 *destination_type* 为 **sysname**，如果未指定，则返回目标 DBMS 中所有数据类型的映射。 启用结果集按目标 DBMS 中的数据类型进行筛选。  
  
`[ @defaults_only = ] defaults_only` 如果只返回默认数据类型映射，则为。 *defaults_only* 为 **bit**，默认值为 **0**。 **1** 表示只返回默认数据类型映射。 **0** 表示返回默认值和任何用户定义的数据类型映射。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|说明|  
|-----------------|-----------------|  
|**mapping_id**|标识数据类型映射。|  
|**source_dbms**|源 DBMS 的名称和版本号。|  
|**source_type**|源 DBMS 中的数据类型。|  
|**destination_dbms**|目标 DBMS 的名称。|  
|**destination_type**|目标 DBMS 中的数据类型。|  
|**is_default**|指示映射是默认映射还是替代映射。 值 **0** 表示此映射是用户定义的。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpdatatypemap** 定义从非 SQL Server 发布服务器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器到非订阅服务器的数据类型映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 如果不支持指定的源 DBMS 和目标 DBMS 组合， **sp_helpdatatypemap** 将返回一个空结果集。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员或分发数据库上 **db_owner** 固定数据库角色的成员才能执行 **sp_helpdatatypemap**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_getdefaultdatatypemapping &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)  
  
  
