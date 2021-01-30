---
description: sp_fulltext_catalog (Transact-SQL)
title: sp_fulltext_catalog (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_catalog_TSQL
- sp_fulltext_catalog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_catalog
ms.assetid: e49b98e4-d1f1-42b2-b16f-eb2fc7aa1cf5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7f2d26fe07a6b5c224de6984dbf47d0ae6bc3b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185447"
---
# <a name="sp_fulltext_catalog-transact-sql"></a>sp_fulltext_catalog (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  创建和删除全文目录，并启动和停止目录的索引操作。 可为每个数据库创建多个全文目录。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 " [创建全文目录](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)"、" [更改全文](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)目录" 和 " [删除全文目录](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) "。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_fulltext_catalog [ @ftcat= ] 'fulltext_catalog_name' ,   
     [ @action= ] 'action'   
     [ , [ @path= ] 'root_directory' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @ftcat = ] 'fulltext_catalog_name'` 全文目录的名称。 每个数据库的目录名称必须是唯一的。 *fulltext_catalog_name* **sysname**。  
  
`[ @action = ] 'action'` 要执行的操作。 *操作* 是 **varchar (20)**，可以是下列值之一。  
  
> [!NOTE]  
>  可根据需要创建、删除和修改全文目录。 但请避免同时对多个目录的架构进行更改。 可以使用 **sp_fulltext_table** 存储过程来执行这些操作，这是建议的方法。  
  
|值|说明|  
|-----------|-----------------|  
|**创建**|在文件系统中创建一个空的新全文目录，并将 **sysfulltextcatalogs** 中的关联行添加到 *fulltext_catalog_name* 并 *root_directory*（如果存在）值。 *fulltext_catalog_name* 在数据库中必须是唯一的。|  
|**击落**|删除 *fulltext_catalog_name* ，方法是将其从文件系统中删除，并删除 **sysfulltextcatalogs** 中的关联行。 如果此目录中包含一个或多个表的索引，则此操作将失败。 **sp_fulltext_table** 应执行 "*table_name*"，以从目录中删除表。<br /><br /> 如果目录不存在，则会显示错误。|  
|**start_incremental**|开始 *fulltext_catalog_name* 的增量填充。 如果目录不存在，则会显示错误。 如果一个全文索引填充已经是活动的，则会显示一个警告，而不发生填充操作。 使用增量填充时，将检索全文索引的已更改行，前提是表中存在一个要进行全文索引的 **时间戳** 列。|  
|**start_full**|启动 *fulltext_catalog_name* 的完全填充。 即使与此全文目录相关联的每一个表的每一行都进行过索引，也会对其检索全文索引。|  
|**停止**|停止 *fulltext_catalog_name* 的索引填充。 如果目录不存在，则会显示错误。 如果已经停止了填充，则不会显示警告。|  
|**重新生成**|重新生成 *fulltext_catalog_name*。 重新生成目录时，将删除现有目录，并在其原位置创建一个新目录。 具有全文索引引用的所有表都与此新目录相关联。 重新生成会重置数据库系统表中的全文元数据。<br /><br /> 如果更改跟踪为 OFF，重新生成并不会导致重新填充新创建的全文目录。 在这种情况下，若要重新填充，请执行 **sp_fulltext_catalog** ，并 **start_full** 或 **start_incremental** 操作。|  
  
`[ @path = ] 'root_directory'` 是根目录 (**创建** 操作的完整物理路径) 。 *root_directory* 为 **nvarchar (100)** ，其默认值为 NULL，表示使用安装时指定的默认位置。 这是 Mssql 目录中的 Ftdata 子目录;例如，C:\Program Files\Microsoft SQL Server\MSSQL13。MSSQLSERVER\MSSQL\FTData. 指定的根目录必须驻留在同一台计算机的某一驱动器上，它不能只包含驱动器号，也不能是相对路径。 不支持使用网络驱动器、可移动驱动器、软盘和 UNC 路径。 全文目录必须在与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例相关联的本地硬盘上创建。  
  
 只有在 **创建***操作* 时， **\@ 路径** 才有效。 对于 **创建** (**停止**、**重新) 生成** 等操作以外的操作， **\@ 路径** 必须为 NULL 或省略。  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例是群集中的虚拟服务器，则指定的目录所在目录需要位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源所属的共享磁盘驱动器上。 如果 @path 未指定，则默认目录目录的位置位于共享磁盘驱动器上，在安装虚拟服务器时指定的目录中。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **Start_full** 操作用于在 *fulltext_catalog_name* 中创建全文数据的完整快照。 **Start_incremental** 操作用于只对数据库中已更改的行重新编制索引。 仅当表包含一个类型为 **timestamp** 的列时，才能应用增量填充。 如果全文目录中的表不包含 **timestamp** 类型的列，则该表将进行完全填充。  
  
 全文目录及索引数据存储在某些文件中，这些文件是在全文目录所在目录中创建的。 如果未指定 **\@ 路径**，则将全文目录目录创建为在 **\@ path** 中指定的目录的子目录或在 "服务器默认全文目录目录" 中指定的目录。 生成全文目录所在目录名称的方式可以保证它在服务器上是唯一的。 因此，一个服务器上所有的全文目录所在目录可以共享相同的路径。  
  
## <a name="permissions"></a>权限  
 调用方需要成为 **db_owner** 角色的成员。 根据所请求的操作，不应拒绝调用方 (**db_owner** 在目标全文目录中具有) 的更改或控制权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-create-a-full-text-catalog"></a>A. 创建全文目录  
 此示例在 **AdventureWorks2012** 数据库中创建 **Cat_Desc** 的空全文目录。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'create';  
GO  
```  
  
### <a name="b-to-rebuild-a-full-text-catalog"></a>B. 重新生成全文目录  
 此示例重新生成 **AdventureWorks2012** 数据库中的现有全文目录 **Cat_Desc**。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'rebuild';  
GO  
```  
  
### <a name="c-start-the-population-of-a-full-text-catalog"></a>C. 启动全文目录的填充  
 此示例将开始对 **Cat_Desc** 目录的完全填充。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'start_full';  
GO  
```  
  
### <a name="d-stop-the-population-of-a-full-text-catalog"></a>D. 停止全文目录的填充  
 此示例将停止 **Cat_Desc** 目录的填充。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'stop';  
GO  
```  
  
### <a name="e-to-remove-a-full-text-catalog"></a>E. 删除全文目录  
 此示例将删除 **Cat_Desc** 目录。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'drop';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [FULLTEXTCATALOGPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_database &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)   
 [sp_help_fulltext_catalogs_cursor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)  
  
  
