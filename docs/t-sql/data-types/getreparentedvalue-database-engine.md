---
description: GetReparentedValue（数据库引擎）
title: GetReparentedValue（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Reparent_TSQL
- Reparent
dev_langs:
- TSQL
helpviewer_keywords:
- Reparent [Database Engine]
ms.assetid: f47f8e25-08ef-498b-84f4-a317aca1f358
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 836502425ba64c9168b53f7242ab3c74775d80f1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92037495"
---
# <a name="getreparentedvalue-database-engine"></a>GetReparentedValue（数据库引擎）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回其相对于根的路径是到 _newRoot_ 的路径的节点，后跟相对于 _oldRoot_ 的路径。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Transact-SQL syntax  
node. GetReparentedValue ( oldRoot, newRoot )  
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetReparentedValue ( SqlHierarchyId oldRoot , SqlHierarchyId newRoot )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
oldRoot  
hierarchyid，它是表示将要修改的层次结构级别的节点。
  
newRoot  
表示节点的 **hierarchyid**。 替换当前节点的 _oldRoot_ 部分以移动该节点。
  
## <a name="return-types"></a>返回类型  
SQL Server 返回类型：hierarchyid
  
CLR 返回类型：SqlHierarchyId
  
## <a name="remarks"></a>备注  
用于通过将节点从 _oldRoot_ 移动到 _newRoot_ 来修改树。 GetReparentedValue 用于将层次结构节点移动到层次结构中的新位置。 **hierarchyid** 数据类型表示层次结构，但并不强制实现层次结构。 用户必须确保 hierarchyid 针对新的位置适当地结构化。 hierarchyid 数据类型的唯一索引有助于避免重复条目。 有关移动整个子树的示例，请参阅[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)。
  
## <a name="examples"></a>示例  
  
### <a name="a-comparing-two-node-locations"></a>A. 比较两个节点位置  
以下示例显示了节点的当前 hierarchyid。 它还显示了如果移动节点使其成为 \@NewParent 节点的后代，节点的 hierarchyid 将会是什么。 它使用 `ToString()` 方法来显示层次结构关系。
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ;  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- who is /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- who is /2/3/  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
(@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) ).ToString() AS Proposed_OrgNode_AS_Text,  
OrgNode AS Current_OrgNode,  
@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) AS Proposed_OrgNode,  
*  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = @SubjectEmployee ;  
GO  
```  
  
### <a name="b-updating-a-node-to-a-new-location"></a>B. 将节点更新到新位置  
下面的示例在 UPDATE 语句中使用 `GetReparentedValue()` 将节点从层次结构中的旧位置移到新位置。
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ; -- Node /1/1/2/  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- Node /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- Node /2/3/  
  
UPDATE HumanResources.EmployeeDemo  
SET OrgNode = @SubjectEmployee. GetReparentedValue(@OldParent, @NewParent)   
WHERE OrgNode = @SubjectEmployee ;  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
*  
FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\gail0' ; -- Now node /2/3/2/  
```  
  
### <a name="c-clr-example"></a>C. CLR 示例  
下面的代码段调用 GetReparentedValue () 方法：
  
```sql
this. GetReparentedValue(oldParent, newParent)  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
