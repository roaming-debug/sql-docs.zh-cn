---
description: GetRoot（数据库引擎）
title: GetRoot（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetRoot
- GetRoot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetRoot [Database Engine]
ms.assetid: 240b70f1-eeda-44ab-b4bb-9e4af80fa7c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6a61597b95b51e731b1a27f02d23b2f7765476cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211748"
---
# <a name="getroot-database-engine"></a>GetRoot（数据库引擎）

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回层次结构树的根。 GetRoot() 是静态方法。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL syntax  
hierarchyid::GetRoot ( )   
```  
  
```syntaxsql
-- CLR syntax  
static SqlHierarchyId GetRoot ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型  
SQL Server 返回类型：hierarchyid
  
CLR 返回类型：SqlHierarchyId
  
## <a name="remarks"></a>备注  
用于确定层次结构树中的根节点。
  
## <a name="examples"></a>示例  
  
### <a name="a-transact-sql-example"></a>A. Transact-SQL 示例  
下面的示例返回层次结构树的根节点：
  
```sql
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = hierarchyid::GetRoot()  
```  
  
### <a name="b-clr-example"></a>B. CLR 示例  
下面的代码段调用 GetRoot() 方法：
  
```sql
SqlHierarchyId.GetRoot()  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
