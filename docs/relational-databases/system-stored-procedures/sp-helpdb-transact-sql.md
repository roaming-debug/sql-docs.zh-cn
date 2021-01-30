---
description: sp_helpdb (Transact-SQL)
title: sp_helpdb (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdb
- sp_helpdb_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdb
ms.assetid: 4c3e3302-6cf1-4b2b-8682-004049b578c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb199d0fc846bc37a8f7c1272563de99507c795
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176441"
---
# <a name="sp_helpdb-transact-sql"></a>sp_helpdb (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  报告有关指定数据库或所有数据库的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdb [ [ @dbname= ] 'name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'name'` 要报告其信息的数据库的名称。 *名称* 为 **sysname**，无默认值。 如果未指定 *名称*，则在 **sys.databases** 目录视图中的所有数据库上 **sp_helpdb** 报告。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|数据库名称。|  
|**db_size**|**nvarchar (13)**|数据库总计大小。|  
|**owner**|**sysname**|数据库所有者，如 **sa**。|  
|**dbid**|**smallint**|数据库 ID。|  
|**created**|**nvarchar(11)**|数据库创建的日期。|  
|**status**|**nvarchar (600)**|以逗号分隔的值列表，这些值是当前在数据库上设置的数据库选项的值。<br /><br /> 只有启用布尔值选项时，才将这些选项列出。 非布尔选项以 *option_name* 值的形式列出了其相应的值 = 。<br /><br /> 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。|  
|**compatibility_level**|**tinyint**|数据库兼容级别：60、65、70、80 或 90。|  
  
 如果指定 *name* ，则会出现一个额外的结果集，其中显示指定数据库的文件分配。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name |**nchar(128)**|逻辑文件名。|  
|**fileid**|**smallint**|文件 ID。|  
|**filename**|**nchar (260)**|操作系统文件名（物理文件名称）。|  
|**文件**|**nvarchar(128)**|文件所属的文件组。<br /><br /> NULL = 文件为日志文件。 它决不是文件组的一部分。|  
|**大小**|**nvarchar (18)**|文件大小 (MB)。|  
|**maxsize**|**nvarchar (18)**|文件大小可达到的最大值。 此字段中的 UNLIMITED 值表示文件可以一直增长到磁盘变满为止。|  
|**年**|**nvarchar (18)**|文件的增量。 此值指示每次需要新空间时添加到文件中的空间量。|  
|**使用情况**|**varchar (9)**|文件用法。 对于数据文件，该值为 **"仅数据"** ; 对于日志文件，该值为 **"仅记录"**。|  
  
## <a name="remarks"></a>备注  
 结果集中的 " **状态** " 列报告了数据库中的哪些选项已设置为 ON。 " **状态** " 列不报告所有数据库选项。 若要查看当前数据库选项设置的完整列表，请使用 **sys.databases** 目录视图。  
  
## <a name="permissions"></a>权限  
 如果指定了单一数据库，则需要在数据库中具有 **公共** 角色的成员身份。 如果未指定数据库，则需要在 **master** 数据库中拥有 **公共** 角色的成员身份。  
  
 如果数据库无法访问， **sp_helpdb** 将显示错误消息15622，并尽可能多地显示有关数据库的信息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-information-about-a-single-database"></a>A. 返回有关单个数据库的信息  
 以下示例显示有关 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的信息。  
  
```sql  
EXEC sp_helpdb N'AdventureWorks2012';  
```  
  
### <a name="b-returning-information-about-all-databases"></a>B. 返回有关所有数据库的信息  
 以下示例显示有关运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的服务器上所有数据库的信息。  
  
```sql  
EXEC sp_helpdb;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.master_files &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
