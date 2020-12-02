---
description: GetDescendant（数据库引擎）
title: GetDescendant（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GetDescendant_TSQL
- GetDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- GetDescendant [Database Engine]
ms.assetid: f5f39596-033e-4243-acbc-caa188b45b03
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c5e76db626c0129253f343075894825c114da4ec
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92037150"
---
# <a name="getdescendant-database-engine"></a>GetDescendant（数据库引擎）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回父级的一个子节点。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL syntax  
parent.GetDescendant ( child1 , child2 )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetDescendant ( SqlHierarchyId child1 , SqlHierarchyId child2 )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
child1  
NULL 或当前节点的子节点的 hierarchyid。
  
child2  
NULL 或当前节点的子节点的 hierarchyid。
  
## <a name="return-types"></a>返回类型  
SQL Server 返回类型：hierarchyid
  
CLR 返回类型：SqlHierarchyId
  
## <a name="remarks"></a>备注  
返回作为父节点的后代的一个子节点。
-   如果父级为 NULL，则返回 NULL。  
-   如果父级不为 NULL，而 child1 和 child2 为 NULL，则返回父级的子级。  
-   如果父级和 child1 不为 NULL，而 child2 为 NULL，则返回一个大于 child1 的父级的子级。  
-   如果父级和 child2 不为 NULL，而 child1 为 NULL，则返回一个小于 child2 的父级的子级。  
-   如果父级、child1 和 child2 都不为 NULL，则返回一个大于 child1 且小于 child2 的父级的子级。  
-   如果 child1 不为 NULL 且不是父级的子级，则引发异常。  
-   如果 child2 不为 NULL 且不是父级的子级，则引发异常。  
-   如果 child1 >= child2，则引发异常。  
  
GetDescendant 是确定性的。 因此，如果使用相同的输入调用 GetDescendant，它将始终生成相同的输出。 不过，生成的子级的确切身份可能因其与其他节点的关系而异，如示例 C 所示。
  
## <a name="examples"></a>示例  
  
### <a name="a-inserting-a-row-as-the-least-descendant-node"></a>A. 插入一行作为最低级别的后代节点  
新雇用了一名员工，该员工将成为位于 `/3/1/` 节点的现有员工的下属。 执行以下代码，使用不含参数的 GetDescendant 方法插入新行，以将新行节点指定为 `/3/1/1/`：
  
```sql
DECLARE @Manager hierarchyid;   
SET @Manager = CAST('/3/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(NULL, NULL),  
'adventure-works\FirstNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="b-inserting-a-row-as-a-greater-descendant-node"></a>B. 插入一行作为较高级别的后代节点  
新雇用了另一名员工，该员工同样将成为示例 A 中的经理的下属。执行以下代码，使用带有子级 1 参数的 GetDescendant 方法插入新行，以指定新行的节点将紧跟示例 A 中的节点，成为 `/3/1/2/`：
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, NULL),  
'adventure-works\SecondNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="c-inserting-a-row-between-two-existing-nodes"></a>C. 在两个现有节点之间插入一行  
雇用了第三名员工，该员工同样将成为示例 A 中的经理的下属。此示例将在比示例 A 中的 `FirstNewEmployee` 高、比示例 B 中的 `SecondNewEmployee` 低的节点中插入新行。使用 GetDescendant 方法执行以下代码。 同时使用 child1 参数和 child2 参数指定新行的节点将成为节点 `/3/1/1.1/`：
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid, @Child2 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
SET @Child2 = CAST('/3/1/2/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, @Child2),  
'adventure-works\ThirdNewEmployee', 'Application Intern', '3/11/07') ;  
  
```  
  
完成示例 A、B 和 C 后，添加到表中的节点将成为具有下列 hierarchyid 值的对等方：
  
`/3/1/1/`
  
`/3/1/1.1/`
  
`/3/1/2/`
  
节点 `/3/1/1.1/` 高于节点 `/3/1/1/`，但是在层次结构中处于同一级别。
  
### <a name="d-scalar-examples"></a>D. 标量示例  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持随意插入和删除任何 hierarchyid 节点。 使用 GetDescendant()，始终可以在任何两个 hierarchyid 节点之间生成一个节点。 执行以下代码，使用 `GetDescendant` 生成示例节点：
  
```sql
DECLARE @h hierarchyid = hierarchyid::GetRoot();  
DECLARE @c hierarchyid = @h.GetDescendant(NULL, NULL);  
SELECT @c.ToString();  
DECLARE @c2 hierarchyid = @h.GetDescendant(@c, NULL);  
SELECT @c2.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
SET @c = @h.GetDescendant(@c, @c2);  
SELECT @c.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
```  
  
### <a name="e-clr-example"></a>E. CLR 示例  
下面的代码段调用 `GetDescendant()` 方法：
  
```sql
SqlHierarchyId parent, child1, child2;  
parent = SqlHierarchyId.GetRoot();  
child1 = parent.GetDescendant(SqlHierarchyId.Null, SqlHierarchyId.Null);  
child2 = parent.GetDescendant(child1, SqlHierarchyId.Null);  
Console.Write(parent.GetDescendant(child1, child2).ToString());  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
