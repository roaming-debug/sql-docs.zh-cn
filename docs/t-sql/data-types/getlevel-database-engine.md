---
description: GetLevel（数据库引擎）
title: GetLevel（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GetLevel
- GetLevel_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetLevel [Database Engine]
ms.assetid: 81577d7e-8ff6-4e73-b7f4-94c03d4921e7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f0aa604a88902dc8bfba522556f11b267fa1e184
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92037158"
---
# <a name="getlevel-database-engine"></a>GetLevel（数据库引擎）

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回一个表示节点 this 在树中的深度的整数。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL syntax  
node.GetLevel ( )   
```  
  
```syntaxsql
-- CLR syntax  
SqlInt16 GetLevel ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型  
SQL Server 返回类型：smallint
  
CLR 返回类型：SqlInt16
  
## <a name="remarks"></a>备注  
用于确定一个或多个节点的级别或者筛选指定级别的成员的节点。 层次结构的根节点为级别 0。
  
GetLevel 对于广度优先搜索索引有用。 有关详细信息，请参阅[Hierarchical Data (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)。
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-hierarchy-level-as-a-column"></a>A. 将层次结构级别返回为列  
下面的示例返回 hierarchyid 的文本表示形式，然后将层次结构级别作为表中所有行的 EmpLevel 列返回：
  
```sql
SELECT OrgNode.ToString() AS Text_OrgNode,   
OrgNode.GetLevel() AS EmpLevel, *  
FROM HumanResources.EmployeeDemo;  
```  
  
### <a name="b-returning-all-members-of-a-hierarchy-level"></a>B. 返回层次结构级别的所有成员  
下面的示例返回表中层次结构级别为 2 的所有行：
  
```sql
SELECT OrgNode.ToString() AS Text_OrgNode,   
OrgNode.GetLevel() AS EmpLevel, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetLevel() = 2;  
```  
  
### <a name="c-returning-the-root-of-the-hierarchy"></a>C. 返回层次结构的根节点  
下面的示例返回层次结构级别的根节点：
  
```sql
SELECT OrgNode.ToString() AS Text_OrgNode,   
OrgNode.GetLevel() AS EmpLevel, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetLevel() = 0;  
```  
  
### <a name="d-clr-example"></a>D. CLR 示例  
下面的代码段调用 GetLevel() 方法：
  
```sql
this.GetLevel()  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
