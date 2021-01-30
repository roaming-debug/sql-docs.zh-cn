---
description: sp_setdefaultdatatypemapping (Transact-SQL)
title: sp_setdefaultdatatypemapping (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setdefaultdatatypemapping
- sp_setdefaultdatatypemapping_TSQL
helpviewer_keywords:
- sp_setdefaultdatatypemapping
ms.assetid: 7394e8ca-4ce1-4e99-a784-205007c2c248
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 46b17720b7d57110dc334a425976bf621a1395bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209251"
---
# <a name="sp_setdefaultdatatypemapping-transact-sql"></a>sp_setdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将和非数据库管理系统之间的现有数据类型映射标记 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (DBMS) 作为默认值。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_setdefaultdatatypemapping [ [ @mapping_id = ] mapping_id ]  
    [ , [ @source_dbms = ] 'source_dbms' ]  
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @source_length_min = ] source_length_min ]  
    [ , [ @source_length_max = ] source_length_max ]  
    [ , [ @source_precision_min = ] source_precision_min ]  
    [ , [ @source_precision_max = ] source_precision_max ]  
    [ , [ @source_scale_min = ] source_scale_min ]  
    [ , [ @source_scale_max = ] source_scale_max ]  
    [ , [ @source_nullable = ] source_nullable ]  
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @destination_length = ] destination_length ]  
    [ , [ @destination_precision = ] destination_precision ]  
    [ , [ @destination_scale = ] destination_scale ]  
    [ , [ @destination_nullable = ] source_nullable ]  
```  
  
## <a name="arguments"></a>参数  
`[ @mapping_id = ] mapping_id` 标识现有数据类型映射。  *mapping_id* 为 **int**，默认值为 NULL。 如果指定 *mapping_id*，则不需要剩余的参数。  
  
`[ @source_dbms = ] 'source_dbms'` 要从中映射数据类型的 DBMS 的名称。 *source_dbms* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|源为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|源为 Oracle 数据库。|  
|NULL（默认值）||  
  
 如果 *mapping_id* 为 NULL，则必须指定此参数。  
  
`[ @source_version = ] 'source_version'` 源 DBMS 的版本号。 *source_version* 是 **varchar (10)**，默认值为 NULL。  
  
`[ @source_type = ] 'source_type'` 源 DBMS 中的数据类型。 *source_type* **sysname**。 如果 *mapping_id* 为 NULL，则必须指定此参数。  
  
`[ @source_length_min = ] source_length_min` 源 DBMS 中的数据类型的最小长度。 *source_length_min* 为 **bigint**，默认值为 NULL。  
  
`[ @source_length_max = ] source_length_max` 源 DBMS 中的数据类型的最大长度。 *source_length_max* 为 **bigint**，默认值为 NULL。  
  
`[ @source_precision_min = ] source_precision_min` 源 DBMS 中的数据类型的最小精度。 *source_precision_min* 为 **bigint**，默认值为 NULL。  
  
`[ @source_precision_max = ] source_precision_max` 源 DBMS 中的数据类型的最大精度。 *source_precision_max* 为 **bigint**，默认值为 NULL。  
  
`[ @source_scale_min = ] source_scale_min` 源 DBMS 中的数据类型的最小小数位数。 *source_scale_min* 的值为 **int**，默认值为 NULL。  
  
`[ @source_scale_max = ] source_scale_max` 源 DBMS 中的数据类型的最大刻度。 *source_scale_max* 的值为 **int**，默认值为 NULL。  
  
`[ @source_nullable = ] source_nullable` 如果源 DBMS 中的数据类型支持 NULL 值，则为。 *source_nullable* 为 **bit**，默认值为 NULL。 **1** 表示支持 NULL 值。  
  
`[ @destination_dbms = ] 'destination_dbms'` 目标 DBMS 的名称。 *destination_dbms* **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**MSSQLSERVER**|目标为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**联手**|目标为 Oracle 数据库。|  
|**DB2**|目标为 IBM DB2 数据库。|  
|**SYBASE**|目标为 Sybase 数据库。|  
|NULL（默认值）||  
  
`[ @destination_version = ] 'destination_version'` 目标 DBMS 的产品版本。 *destination_version* 是 **varchar (10)**，默认值为 NULL。  
  
`[ @destination_type = ] 'destination_type'` 目标 DBMS 中列出的数据类型。 *destination_type* 的值为 **sysname**，默认值为 NULL。  
  
`[ @destination_length = ] destination_length` 目标 DBMS 中的数据类型的长度。 *destination_length* 为 **bigint**，默认值为 NULL。  
  
`[ @destination_precision = ] destination_precision` 目标 DBMS 中的数据类型的精度。 *destination_precision* 为 **bigint**，默认值为 NULL。  
  
`[ @destination_scale = ] destination_scale` 目标 DBMS 中的数据类型的小数位数。 *destination_scale* 的值为 **int**，默认值为 NULL。  
  
`[ @destination_nullable = ] destination_nullable` 如果目标 DBMS 中的数据类型支持 NULL 值，则为。 *destination_nullable* 为 **bit**，默认值为 NULL。 **1** 表示支持 NULL 值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_setdefaultdatatypemapping** 用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与非 DBMS 之间的所有复制类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 默认数据类型映射适用于所有包含指定 DBMS 的复制拓扑。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_setdefaultdatatypemapping** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [指定 Oracle 发布服务器的数据类型映射](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [sp_getdefaultdatatypemapping &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_helpdatatypemap &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  
