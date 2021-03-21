---
description: '将 SQL Server 索引拖放 (Native Client OLE DB 提供程序) '
title: 将 SQL Server 索引拖放 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- removing indexes
- deleting indexes
- DropIndex function
- dropping indexes
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
ms.assetid: add3ba14-10b1-4723-b7c0-3e83689e9fdd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26c14af7fd452b93df93ef5c59b777b4057e3279
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748637"
---
# <a name="dropping-a-sql-server-native-client-index"></a>删除 SQL Server Native Client 索引

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序公开 **IIndexDefinition：:D ropindex** 函数。 这允许使用者从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中删除某一索引。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序将一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 主键和 UNIQUE 约束公开为索引。 表所有者、数据库所有者或某些管理角色成员可以修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，以及删除约束。 默认情况下，只有表所有者才能删除现有索引。 因此，DropIndex 成功或失败不仅取决于应用程序用户的访问权限，而且取决于指示的索引的类型  。  
  
 在 pTableID 参数的 uName 联合的 pwszName 成员中，使用者将表名指定为 Unicode 字符串    。 pTableID 的 eKind 成员必须是 DBKIND_NAME   。  
  
 在 pIndexID 参数的 uName 联合的 pwszName成员中，使用者将索引名指定为 Unicode 字符串    。 pIndexID 的 eKind 成员必须是 DBKIND_NAME   。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 *pIndexID* 为 null 时，Native Client OLE DB 提供程序不支持删除表上的所有索引的 OLE DB 功能。 如果 pIndexID 为 Null，则返回 E_INVALIDARG  。  
  
## <a name="see-also"></a>另请参阅  
 [表和索引](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
