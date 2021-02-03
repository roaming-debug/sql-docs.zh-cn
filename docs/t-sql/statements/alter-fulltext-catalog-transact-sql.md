---
description: ALTER FULLTEXT CATALOG (Transact-SQL)
title: ALTER FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_FULLEXT_CATALOG_TSQL
- ALTER FULLEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- modifying full-text catalogs
- full-text catalogs [SQL Server], rebuilding
- accent sensitivity
- ALTER FULLTEXT CATALOG statement
- full-text catalogs [SQL Server], modifying
- full-text catalogs [SQL Server], reorganizing
ms.assetid: 31a47aaf-6c7f-48a4-a86a-d57aec66c9cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aedb9f32a845a7b25d856c8bfa5aff20facbf4a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204235"
---
# <a name="alter-fulltext-catalog-transact-sql"></a>ALTER FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  更改全文目录的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
ALTER FULLTEXT CATALOG catalog_name   
{ REBUILD [ WITH ACCENT_SENSITIVITY = { ON | OFF } ]  
| REORGANIZE  
| AS DEFAULT   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 catalog_name  
 指定要修改的目录的名称。 如果具有指定名称的目录不存在，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回错误，并且不执行 ALTER 操作。  
  
 REBUILD  
 告知 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新生成整个目录。 重新生成目录时，将删除现有目录，并在其原位置创建一个新目录。 具有全文索引引用的所有表都与此新目录相关联。 重新生成会重置数据库系统表中的全文元数据。  
  
 WITH ACCENT_SENSITIVITY = {ON|OFF}  
 指定要更改的全文索引和查询的目录是区分重音的，还是不区分重音的。  
  
 若要确定全文目录当前的区分重音属性的设置，请对 catalog_name 使用具有 accentsensitivity 属性值的 FULLTEXTCATALOGPROPERTY 函数。 如果函数返回“1”，则全文目录是区分重音的；如果函数返回“0”，则目录是不区分重音的。  
  
 目录和数据库的默认重音区分设置是相同的。  
  
 REORGANIZE  
 告知 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行“主合并”，以将在索引进程中创建的各个较小的索引合并成一个大型索引。 合并全文索引碎片可以提高性能，并释放磁盘和内存资源。 如果全文目录频繁发生更改，则请定期使用该命令重新组织全文目录。  
  
 REORGANIZE 还可以优化内部索引和目录结构。  
  
 请注意，根据索引数据的数量，完成主合并操作可能要花费一些时间。 对大量数据进行主合并会创建一个长时间运行的事务，在检查点期间延迟事务日志的截断。 在这种情况下，事务日志可能会在完整恢复模式下显著增长。 作为最佳实践，在使用完整恢复模式的数据库中重新组织较大的全文索引之前，应确保事务日志中包含足够的空间用于长时间运行的事务。 有关详细信息，请参阅 [管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)。  
  
 AS DEFAULT  
 指定此目录为默认目录。 如果创建全文索引时没有指定目录，则将使用默认目录。 如果存在默认全文目录，则以 AS DEFAULT 设置该目录将覆盖现有默认设置。  
  
## <a name="permissions"></a>权限  
 用户必须对全文目录具有 ALTER 权限，或者是 db_owner、db_ddladmin 固定数据库角色或 sysadmin 固定服务器角色的成员。  
  
> [!NOTE]  
>  若要使用 ALTER FULLTEXT CATALOG AS DEFAULT，用户必须对全文目录具有 ALTER 权限，对数据库具有 CREATE FULLTEXT CATALOG 权限。  
  
## <a name="examples"></a>示例  
 下面的示例更改默认全文目录 `accentsensitivity`（区分重音）的 `ftCatalog` 属性。  
  
```sql  
--Change to accent insensitive  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog   
REBUILD WITH ACCENT_SENSITIVITY=OFF;  
GO  
-- Check Accentsensitivity  
SELECT FULLTEXTCATALOGPROPERTY('ftCatalog', 'accentsensitivity');  
GO  
--Returned 0, which means the catalog is not accent sensitive.  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_catalogs (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)  
  
  
