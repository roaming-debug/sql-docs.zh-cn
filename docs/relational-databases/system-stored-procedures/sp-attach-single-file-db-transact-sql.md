---
description: sp_attach_single_file_db (Transact-SQL)
title: sp_attach_single_file_db (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_single_file_db
- sp_attach_single_file_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_single_file_db
ms.assetid: 13bd1044-9497-4293-8390-1f12e6b8e952
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8fd050ff5706cc2406fcb4502c5d93a8253bc4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197708"
---
# <a name="sp_attach_single_file_db-transact-sql"></a>sp_attach_single_file_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将只有一个数据文件的数据库附加到当前服务器。 **sp_attach_single_file_db** 不能用于多个数据文件。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 建议改为使用 CREATE DATABASE *database_name* 作为附加。 有关详细信息，请参阅 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)。 不要针对复制数据库使用此过程。  
  
> [!IMPORTANT]  
>  建议您不要附加或还原来自未知或不可信源的数据库。 此类数据库可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构或物理数据库结构导致错误。 使用来自未知源或不可信源的数据库前，请在非生产服务器上针对数据库运行 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ，然后检查数据库中的代码，例如存储过程或其他用户定义代码。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_attach_single_file_db [ @dbname= ] 'dbname'  
    , [ @physname= ] 'physical_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 要附加到服务器的数据库的名称。 该名称必须是唯一的。 *dbname* 的值为 **sysname**，默认值为 NULL。  
  
`[ @physname = ] 'physical_name'` 是数据库文件的物理名称，包括路径。 *physical_name* 为 **nvarchar (260)**，默认值为 NULL。  
  
> [!NOTE]  
>  此参数映射到 CREATE DATABASE 语句的 FILENAME 参数。 有关详细信息，请参阅 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)。  
  
 当将包含全文目录文件的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库附加到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器实例上时，会将目录文件从其以前的位置与其他数据库文件一起附加，这与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中的情况相同。 有关详细信息，请参阅 [全文搜索升级](../../relational-databases/search/upgrade-full-text-search.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 仅在以前从服务器分离的数据库上，使用显式 **sp_detach_db** 操作或在复制的数据库上使用 **sp_attach_single_file_db** 。  
  
 **sp_attach_single_file_db** 仅适用于具有单个日志文件的数据库。 当 **sp_attach_single_file_db** 将数据库附加到服务器时，它将生成新的日志文件。 如果该数据库是只读数据库，则会在日志文件的先前位置生成日志文件。  
  
> [!NOTE]  
>  不能分离或附加数据库快照。  
  
 不要针对复制数据库使用此过程。  
  
## <a name="permissions"></a>权限  
 有关附加数据库时如何处理权限的信息，请参阅 [CREATE database &#40;SQL Server transact-sql&#41;](../../t-sql/statements/create-database-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 以下示例分离 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]，然后将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 中的一个文件附加到当前服务器。  
  
```  
USE master;  
GO  
EXEC sp_detach_db @dbname = 'AdventureWorks2012';  
EXEC sp_attach_single_file_db @dbname = 'AdventureWorks2012',   
    @physname =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf';  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
