---
description: sys.sp_cdc_get_ddl_history (Transact-SQL)
title: sys.sp_cdc_get_ddl_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_get_ddl_history
- sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history_TSQL
- sys.sp_cdc_get_ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sp_cdc_get_ddl_history
- sys.sp_cdc_get_ddl_history
ms.assetid: 4dee5e2e-d7e5-4fea-8037-a4c05c969b3a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b83e018c8e8e7c5e2f4daa704e5b7a13e7789f0b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205956"
---
# <a name="syssp_cdc_get_ddl_history-transact-sql"></a>sys.sp_cdc_get_ddl_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回自对指定的捕获实例启用变更数据捕获后与该捕获实例关联的数据定义语言 (DDL) 更改历史记录。 并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本中均提供变更数据捕获功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_get_ddl_history [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>参数  
 [ @capture_instance =] "*capture_instance*"  
 与源表关联的捕获实例的名称。 *capture_instance* 为 **sysname** ，且不能为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|源表架构的名称。|  
|source_table|**sysname**|源表的名称。|  
|capture_instance|**sysname**|捕获实例的名称。|  
|required_column_update|**bit**|表示 DDL 更改要求对更改表中的列进行更改以反映对源列所做的数据类型更改。|  
|ddl_command|**nvarchar(max)**|应用到源表的 DDL 语句。|  
|ddl_lsn|**binary(10)**|与 DDL 更改关联的日志序列号 (LSN)。|  
|ddl_time|**datetime**|与 DDL 更改关联的时间。|  
  
## <a name="remarks"></a>备注  
 对更改源表列结构的源表（如添加或删除列，或更改现有列的数据类型）进行 DDL 修改会在 [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) 表中进行维护。 您可使用此存储过程报告以上更改。 当捕获进程读取日志中的 DDL 事务时，将向 cdc.ddl_history 中添加项。  
  
## <a name="permissions"></a>权限  
 要求拥有 db_owner 固定数据库角色的成员身份以返回针对数据库中所有捕获实例的行。 对于所有其他用户，要求对源表中的所有已捕获列具有 SELECT 权限；如果已定义捕获实例的访问控制角色，则还要求具有该数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将列添加到源表 `HumanResources.Employee` 中，然后运行 `sys.sp_cdc_get_ddl_history` 存储过程来报告应用到与捕获实例 `HumanResources_Employee` 关联的源表的 DDL 更改。  
  
```  
USE AdventureWorks2012;  
GO  
ALTER TABLE HumanResources.Employee  
ADD Test_Column int NULL;  
GO  
-- Pause 10 seconds to allow the event to be logged.   
WAITFOR DELAY '00:00:10';  
GO   
EXECUTE sys.sp_cdc_get_ddl_history   
    @capture_instance = 'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
