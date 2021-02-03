---
description: ToString（数据库引擎）
title: ToString（数据库引擎）
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString
- ToString_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ToString [Database Engine]
ms.assetid: 5fc11ca5-c26d-4518-9512-67aa0270f110
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc4af80c3e3c05009eb488a87caa0107f3185e87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179637"
---
# <a name="tostring-database-engine"></a>ToString（数据库引擎）

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回具有 this 逻辑表示形式的字符串  。 进行从 hierarchyid 到字符串类型的转换时将隐式调用 ToString  。 作用与 [Parse（数据库引擎）](../../t-sql/data-types/parse-database-engine.md)相反。
  
## <a name="syntax"></a>语法  

```syntaxsql
-- Transact-SQL syntax
node.ToString  ( )
-- This is functionally equivalent to the following syntax  
-- which implicitly calls ToString():  
CAST(node AS nvarchar(4000))  
```  
  
```syntaxsql
-- CLR syntax
string ToString  ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型

SQL Server 返回类型：nvarchar(4000)
  
CLR 返回类型：String
  
## <a name="remarks"></a>备注  
返回层次结构中的逻辑位置。 例如，`/2/1/` 表示以下文件系统层次结构的第四行 ([!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])：
  
```sql
/        C:\  
/1/      C:\Database Files  
/2/      C:\Program Files  
/2/1/    C:\Program Files\Microsoft SQL Server  
/2/2/    C:\Program Files\Microsoft Visual Studio  
/3/      C:\Windows  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-transact-sql-example-in-a-table"></a>A. 表中的 Transact-SQL 示例  
下面的示例以 hierarchyid 数据类型和可读性更强的字符串格式返回 `OrgNode` 列  ：
  
```sql
SELECT OrgNode,  
OrgNode.ToString() AS Node  
FROM HumanResources.EmployeeDemo  
ORDER BY OrgNode ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
OrgNode   Node  
0x        /  
0x58      /1/  
0x5AC0    /1/1/  
0x5B40    /1/2/  
0x5BC0    /1/3/  
0x5C20    /1/4/  
...  
```  
  
### <a name="b-converting-transact-sql-values-without-a-table"></a>B. 不使用表转换 Transact-SQL 值  
下面的代码示例使用 `ToString` 将 hierarchyid 值转换为字符串，并使用 `Parse` 将字符串值转换为 hierarchyid。
  
```sql
DECLARE @StringValue AS nvarchar(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="c-clr-example"></a>C. CLR 示例  
下面的代码段调用 ToString() 方法：
  
```sql
this.ToString()  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
