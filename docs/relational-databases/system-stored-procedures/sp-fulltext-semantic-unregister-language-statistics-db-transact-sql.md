---
description: sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)
title: sp_fulltext_semantic_unregister_language_statistics_db (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_semantic_unregister_language_statistics_db_TSQL
- sp_fulltext_semantic_unregister_language_statistics_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_semantic_unregister_language_statistics_db
ms.assetid: 1426ca4a-9a76-489e-98da-8f6d13ff9732
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2d81bc4f43e93cb58b425224fb39f31a27fe7f90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189557"
---
# <a name="sp_fulltext_semantic_unregister_language_statistics_db-transact-sql"></a>sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例中撤消注册现有语义语言统计数据库并删除所有关联元数据。  
  
 此语句不分离数据库，也不会从文件系统中删除物理数据库文件。 撤消注册该数据库后，您可以分离该数据库并删除物理数据库文件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 此过程不需要任何参数。 由于一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例仅有一个语义语言统计数据库，所以不必标识该数据库。  
  
## <a name="return-code-value"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-set"></a>结果集  
 无。  
  
## <a name="general-remarks"></a>一般备注  
 撤消注册语义语言统计数据库后，与之关联的所有元数据也随之删除。  
  
 **sp_fulltext_semantic_unregister_language_statistics_db** 执行以下步骤：  
  
1.  确保当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例未在进行语义填充。  
  
2.  删除与指定的语义语言统计数据库关联的所有元数据。  

 有关详细信息，请参阅 [安装和配置语义搜索](../../relational-databases/search/install-and-configure-semantic-search.md)。  
  
## <a name="metadata"></a>元数据  
 有关实例上安装的语义语言统计信息数据库的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请 [&#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md)查询目录视图 sys.fulltext_semantic_language_statistics_database。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要具有 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何通过调用 **sp_fulltext_semantic_unregister_language_statistics_db** 取消注册语义语言统计信息数据库。  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安装和配置语义搜索](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
