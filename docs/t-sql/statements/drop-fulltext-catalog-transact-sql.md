---
description: DROP FULLTEXT CATALOG (Transact-SQL)
title: DROP FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_FULLTEXT_CATALOG_TSQL
- DROP FULLTEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- dropping full-text catalogs
- removing full-text catalogs
- full-text catalogs [SQL Server], removing
- deleting full-text catalogs
- DROP FULLTEXT CATALOG statement
ms.assetid: b54efb0b-400b-49ce-923b-ce20a2a12255
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6842d3f4d61d66a8762d48793975c5facbee3144
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204658"
---
# <a name="drop-fulltext-catalog-transact-sql"></a>DROP FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从数据库中删除全文目录。 在删除目录之前，必须先删除与该目录关联的所有全文索引。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
DROP FULLTEXT CATALOG catalog_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 catalog_name  
 要删除的目录名。 如果 catalog_name 不存在，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回一个错误，并且不执行 DROP 操作。 要使该命令成功执行，不能将全文目录的文件组标记为 OFFLINE 或 READONLY。  
  
## <a name="permissions"></a>权限  
 用户必须对全文目录有 DROP 权限，或者是 db_owner 或 db_ddladmin 固定数据库角色的成员。  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_catalogs (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)  
  
  
