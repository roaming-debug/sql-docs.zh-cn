---
description: sys.sp_cdc_disable_table (Transact-SQL)
title: sys.sp_cdc_disable_table (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_disable_table
- sp_cdc_disable_table
- sys.sp_cdc_disable_table_TSQL
- sp_cdc_disable_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_table
- sys.sp_cdc_disable_table
- change data capture [SQL Server], disabling tables
ms.assetid: da2156c0-504e-4d76-b9a0-4448becf9bda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 53984d18923cef297e267374045f0f4cb4364419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206016"
---
# <a name="syssp_cdc_disable_table-transact-sql"></a>sys.sp_cdc_disable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对当前数据库中指定的源表和捕获实例禁用变更数据捕获。 并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本中均提供变更数据捕获功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_disable_table   
  [ @source_schema = ] 'source_schema' ,   
  [ @source_name = ] 'source_name'  
  [ , [ @capture_instance = ] 'capture_instance' | 'all' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @source_schema = ] 'source\_schema'` 包含源表的架构的名称。 *source_schema* **sysname**，无默认值，且不能为 NULL。  
  
 当前数据库中必须存在 *source_schema* 。  
  
`[ @source_name = ] 'source\_name'` 要禁用变更数据捕获的源表的名称。 *source_name* **sysname**，无默认值，且不能为 NULL。  
  
 当前数据库中必须存在 *source_name* 。  
  
`[ @capture_instance = ] 'capture\_instance' | 'all'` 要为指定的源表禁用的捕获实例的名称。 *capture_instance* 为 **sysname** ，且不能为 NULL。  
  
 当指定 "all" 时，将禁用为 *source_name* 定义的所有捕获实例。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **sys.sp_cdc_disable_table** 删除与指定的源表和捕获实例相关联的变更数据捕获更改表和系统函数。 它从变更数据捕获系统表中删除与指定捕获实例相关联的任何行，并将 [sys.databases](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)目录视图中的表条目的 **is_tracked_by_cdc** 列设置为0。  
  
## <a name="permissions"></a>权限  
 需要 **db_owner** 固定数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 下例对 `HumanResources.Employee` 表禁用了变更数据捕获。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_table   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee',  
    @capture_instance = N'HumanResources_Employee';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_enable_table &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)  
  
  
