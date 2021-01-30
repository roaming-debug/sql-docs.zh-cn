---
description: sp_getdefaultdatatypemapping (Transact-SQL)
title: sp_getdefaultdatatypemapping (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getdefaultdatatypemapping_TSQL
- sp_getdefaultdatatypemapping
helpviewer_keywords:
- sp_getdefaultdatatypemapping
ms.assetid: b8401de1-f135-41d0-ba79-ce8fe1f48c00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64345f8471caf5d757e0c8fbdbc843b2a18680ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183167"
---
# <a name="sp_getdefaultdatatypemapping-transact-sql"></a>sp_getdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库管理系统 (DBMS) 之间的指定数据类型的默认映射的信息。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_getdefaultdatatypemapping [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
        , [ @source_type = ] 'source_type'    
    [ , [ @source_length = ] source_length ]  
    [ , [ @source_precision = ] source_precision ]  
    [ , [ @source_scale = ] source_scale ]  
    [ , [ @source_nullable = ] source_nullable ]  
        , [ @destination_dbms = ] 'destination_dbms'   
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' OUTPUT ]  
    [ , [ @destination_length = ] destination_length OUTPUT ]  
    [ , [ @destination_precision = ] destination_precision OUTPUT ]  
    [ , [ @destination_scale = ] destination_scale OUTPUT ]  
    [ , [ @destination_nullable = ] source_nullable OUTPUT ]  
    [ , [ @dataloss = ] dataloss OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @source_dbms = ] 'source_dbms'` 要从中映射数据类型的 DBMS 的名称。 *source_dbms* **sysname**，可以是下列值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|源为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|源为 Oracle 数据库。|  
  
 必须指定此参数。  
  
`[ @source_version = ] 'source_version'` 源 DBMS 的版本号。 *source_version* 是 **varchar (10)**，默认值为 NULL。  
  
`[ @source_type = ] 'source_type'` 源 DBMS 中的数据类型。 *source_type* **sysname**，无默认值。  
  
`[ @source_length = ] source_length` 源 DBMS 中的数据类型的长度。 *source_length* 为 **bigint**，默认值为 NULL。  
  
`[ @source_precision = ] source_precision` 源 DBMS 中的数据类型的精度。 *source_precision* 为 **bigint**，默认值为 NULL。  
  
`[ @source_scale = ] source_scale` 源 DBMS 中的数据类型的小数位数。 *source_scale* 的值为 **int**，默认值为 NULL。  
  
`[ @source_nullable = ] source_nullable` 如果源 DBMS 中的数据类型支持 NULL 值，则为。 *source_nullable* 的数据类型为 **bit**，默认值为 **1**，表示支持 NULL 值。  
  
`[ @destination_dbms = ] 'destination_dbms'` 目标 DBMS 的名称。 *destination_dbms* **sysname**，可以是下列值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|目标为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|目标为 Oracle 数据库。|  
|**DB2**|目标为 IBM DB2 数据库。|  
|**SYBASE**|目标为 Sybase 数据库。|  
  
 必须指定此参数。  
  
`[ @destination_version = ] 'destination_version'` 目标 DBMS 的产品版本。 *destination_version* 是 **varchar (10)**，默认值为 NULL。  
  
`[ @destination_type = ] 'destination_type' OUTPUT` 目标 DBMS 中列出的数据类型。 *destination_type* 的值为 **sysname**，默认值为 NULL。  
  
`[ @destination_length = ] destination_length OUTPUT` 目标 DBMS 中的数据类型的长度。 *destination_length* 为 **bigint**，默认值为 NULL。  
  
`[ @destination_precision = ] destination_precision OUTPUT` 目标 DBMS 中的数据类型的精度。 *destination_precision* 为 **bigint**，默认值为 NULL。  
  
`[ @destination_scale = ] _destination_scaleOUTPUT` 目标 DBMS 中的数据类型的小数位数。 *destination_scale* 的值为 **int**，默认值为 NULL。  
  
`[ @destination_nullable = ] _destination_nullableOUTPUT` 如果目标 DBMS 中的数据类型支持 NULL 值，则为。 *destination_nullable* 为 **bit**，默认值为 NULL。 **1** 表示支持 NULL 值。  
  
`[ @dataloss = ] _datalossOUTPUT` 如果映射有可能丢失数据，则为。 *数据丢失* 的值为 **bit**，默认值为 NULL。 **1** 表示可能会丢失数据。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_getdefaultdatatypemapping** 用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与非 DBMS 之间的所有复制类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 **sp_getdefaultdatatypemapping** 返回与指定的源数据类型最匹配的默认目标数据类型。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_getdefaultdatatypemapping** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_helpdatatypemap &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)  
  
  
