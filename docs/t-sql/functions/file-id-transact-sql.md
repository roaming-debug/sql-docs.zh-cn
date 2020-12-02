---
description: FILE_ID (Transact-SQL)
title: FILE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILE_ID
- FILE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IDs [SQL Server], files
- file IDs [SQL Server]
- FILE_ID function
- names [SQL Server], files
- identification numbers [SQL Server], files
- file names [SQL Server], FILE_ID
ms.assetid: 6a7382cf-a360-4d62-b9d2-5d747f56f076
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9cdd99caf69ce254e1a42d345526515d2fb0472f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124696"
---
# <a name="file_id-transact-sql"></a>FILE_ID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

对于当前数据库的组件文件的给定逻辑名称，此函数返回文件标识 (ID) 号。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改用 [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
FILE_ID ( file_name )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
file_name  
类型 sysname 的表达式，表示文件的逻辑名称，文件 ID 值 `FILE_ID` 将返回此逻辑名称。  
  
## <a name="return-types"></a>返回类型  
**smallint**  
  
## <a name="remarks"></a>备注  
file_name 对应于 sys.master_files 或 sys.database_files 目录视图的 name 列中所显示的逻辑文件名。  

如果 file_name 不对应当前数据库组件文件的逻辑名称，`FILE_ID` 将返回 `NULL`。
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，分配给全文目录的文件标识号大于 32767。 因为 `FILE_ID` 函数具有 smallint 返回类型，`FILE_ID` 将不支持全文文件。 改用 [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md)。  
  
## <a name="examples"></a>示例  
此示例返回 `AdventureWorks_Data` 文件的文件 ID 值，它是 `ADVENTUREWORKS2012` 数据库的组件文件。  

```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_ID('AdventureWorks2012_Data')AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
1  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 中不推荐使用的数据库引擎功能](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [FILE_NAME (Transact-SQL)](../../t-sql/functions/file-name-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
