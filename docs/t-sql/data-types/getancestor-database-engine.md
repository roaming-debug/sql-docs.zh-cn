---
description: GetAncestor（数据库引擎）
title: GetAncestor（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetAncestor_TSQL
- GetAncestor
dev_langs:
- TSQL
helpviewer_keywords:
- GetAncestor [Database Engine]
ms.assetid: b96a986f-d5e4-4034-8013-de7974594ee9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0a306814f0146fdb0dc1833d0d6f98d84cae8f1a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172549"
---
# <a name="getancestor-database-engine"></a>GetAncestor（数据库引擎）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回表示 this 的第 n 个祖先的 hierarchyid。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL syntax  
child.GetAncestor ( n )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetAncestor ( int n )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*n*  
int，表示在层次结构中上升的级别数。
  
## <a name="return-types"></a>返回类型
SQL Server 返回类型：hierarchyid
  
CLR 返回类型：SqlHierarchyId
  
## <a name="remarks"></a>备注  
用于测试输出中的每个节点是否将当前节点作为指定级别的祖先。
  
如果传递的数字大于 [GetLevel()](../../t-sql/data-types/getlevel-database-engine.md)，则返回 NULL。
  
如果传递的是负数，则引发异常。
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-child-nodes-of-a-parent"></a>A. 查找父级的子节点  
`GetAncestor(1)` 返回以 `david0` 作为其直接祖先（其父级）的员工。 下面的示例使用 `GetAncestor(1)`。
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(1) = @CurrentEmployee ;  
```  
  
### <a name="b-returning-the-grandchildren-of-a-parent"></a>B. 返回父级的孙级  
`GetAncestor(2)` 返回层次结构中从当前节点下降两个级别的员工。 这些员工节点是当前节点的孙级。 下面的示例使用 `GetAncestor(2)`。
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\ken0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(2) = @CurrentEmployee ;  
```  
  
### <a name="c-returning-the-current-row"></a>C. 返回当前行  
若要使用 `GetAncestor(0)` 返回当前节点，请执行以下代码。
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(0) = @CurrentEmployee ;  
```  
  
### <a name="d-returning-a-hierarchy-level-if-a-table-isnt-present"></a>D. 如果表不存在，则返回层次结构级别  
即使表不存在，`GetAncestor` 也会返回层次结构中所选的级别。 例如，以下代码指定当前员工，并在不引用表的情况下返回当前员工的祖先的 `hierarchyid`。
  
```sql
DECLARE @CurrentEmployee hierarchyid ;  
DECLARE @TargetEmployee hierarchyid ;  
SELECT @CurrentEmployee = '/2/3/1.2/5/3/' ;  
SELECT @TargetEmployee = @CurrentEmployee.GetAncestor(2) ;  
SELECT @TargetEmployee.ToString(), @TargetEmployee ;  
```  
  
### <a name="e-calling-a-common-language-runtime-method"></a>E. 调用公共语言运行时方法  
下面的代码段调用 `GetAncestor()` 方法。
  
```sql
this.GetAncestor(1)  
```  
  
## <a name="see-also"></a>另请参阅
[IsDescendantOf（数据库引擎）](../../t-sql/data-types/isdescendantof-database-engine.md)  
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
