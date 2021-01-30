---
description: sp_foreignkeys (Transact-SQL)
title: sp_foreignkeys (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_foreignkeys_TSQL
- sp_foreignkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_foreignkeys
ms.assetid: 935fe385-19ff-41a4-8d0b-30618966991d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 377ef64c76238e2e840107dbbe6b32dc9ba4c00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189566"
---
# <a name="sp_foreignkeys-transact-sql"></a>sp_foreignkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回引用链接服务器中的表的主键的外键。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_foreignkeys [ @table_server = ] 'table_server'   
     [ , [ @pktab_name = ] 'pktab_name' ]   
     [ , [ @pktab_schema = ] 'pktab_schema' ]   
     [ , [ @pktab_catalog = ] 'pktab_catalog' ]   
     [ , [ @fktab_name = ] 'fktab_name' ]   
     [ , [ @fktab_schema = ] 'fktab_schema' ]   
     [ , [ @fktab_catalog = ] 'fktab_catalog' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @table_server = ] 'table_server'` 要为其返回表信息的链接服务器的名称。 *table_server* **sysname**，无默认值。  
  
`[ @pktab_name = ] 'pktab_name'` 具有主键的表的名称。 *pktab_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @pktab_schema = ] 'pktab_schema'` 包含主键的架构的名称。 *pktab_schema* 的默认值为 **sysname**，默认值为 NULL。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，该名称包含所有者名称。  
  
`[ @pktab_catalog = ] 'pktab_catalog'` 具有主键的目录的名称。 *pktab_catalog* 的默认值为 **sysname**，默认值为 NULL。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，该名称包含数据库名称。  
  
`[ @fktab_name = ] 'fktab_name'` 带有外键的表的名称。 *fktab_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @fktab_schema = ] 'fktab_schema'` 包含外键的架构的名称。 *fktab_schema* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @fktab_catalog = ] 'fktab_catalog'` 包含外键的目录的名称。 *fktab_catalog* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 各种 DBMS 产品支持表的三部分命名 (_目录_**。**_架构_**。**_表_) ，在结果集中表示。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**PKTABLE_CAT**|**sysname**|主键所在表的目录。|  
|**PKTABLE_SCHEM**|**sysname**|主键所在表的架构。|  
|**PKTABLE_NAME**|**sysname**|与主键)  (的表的名称。 此字段始终返回值。|  
|**PKCOLUMN_NAME**|**sysname**|返回的每个 **TABLE_NAME** 列的主键列或列的名称。 此字段始终返回值。|  
|**FKTABLE_CAT**|**sysname**|外键所在表的目录。|  
|**FKTABLE_SCHEM**|**sysname**|外键所在表的架构。|  
|**FKTABLE_NAME**|**sysname**|带有外键)  (表的名称。 此字段始终返回值。|  
|**FKCOLUMN_NAME**|**sysname**|外键列的名称，针对 TABLE_NAME 返回的每一列。 此字段始终返回值。|  
|**KEY_SEQ**|**smallint**|多列主键中列的序列号。 此字段始终返回值。|  
|**UPDATE_RULE**|**smallint**|当 SQL 操作是更新操作时应用于外键的操作。 对于这些列，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回 0、1 或 2：<br /><br /> 0=对外键的 CASCADE 更改。<br /><br /> 1=NO ACTION 更改（如果有外键）。<br /><br /> 2=SET_NULL；将外键设置为 NULL。|  
|**DELETE_RULE**|**smallint**|当 SQL 操作是删除操作时应用于外键的操作。 对于这些列，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回 0、1 或 2：<br /><br /> 0=对外键的 CASCADE 更改。<br /><br /> 1=NO ACTION 更改（如果有外键）。<br /><br /> 2=SET_NULL；将外键设置为 NULL。|  
|**FK_NAME**|**sysname**|外键标识符。 如果不适用于数据源，则为 NULL。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回 FOREIGN KEY 约束名称。|  
|**PK_NAME**|**sysname**|主键标识符。 如果不适用于数据源，则为 NULL。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回 PRIMARY KEY 约束名称。|  
|**DEFERRABILITY**|**smallint**|指示是否可延迟约束检查。|  
  
 在结果集中，FK_NAME 和 PK_NAME 列总是返回 NULL。  
  
## <a name="remarks"></a>备注  
 **sp_foreignkeys** 查询与 *table_server* 对应的 OLE DB 提供程序的 **IDBSchemaRowset** 接口的 FOREIGN_KEYS 行集。 将 *table_name*、 *table_schema*、 *table_catalog* 和 *列* 参数传递到此接口，以限制返回的行数。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="examples"></a>示例  
 下面的示例返回有关链接服务器 `Department` 上 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的 `Seattle1` 表中的外键的信息。  
  
```  
EXEC sp_foreignkeys @table_server = N'Seattle1',   
   @pktab_name = N'Department',   
   @pktab_catalog = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_catalogs &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_indexes &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
