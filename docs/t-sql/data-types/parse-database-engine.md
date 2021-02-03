---
description: Parse（数据库引擎）
title: Parse（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Parse
- Parse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Parse [Database Engine]
ms.assetid: b37e28b6-6e2e-470a-945b-ce5252da743a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b74df820ef7cd0bebacf77d5c67344bedac2b5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187852"
---
# <a name="parse-database-engine"></a>Parse（数据库引擎）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Parse 将 hierarchyid 的规范字符串表示形式转换为 hierarchyid 值。 当发生从字符串类型到 hierarchyid 的转换时，将隐式调用 Parse。 作用与 [ToString](../../t-sql/data-types/tostring-database-engine.md) 相反。 Parse() 是静态方法。
  
## <a name="syntax"></a>语法  
  
```sql
-- Transact-SQL syntax  
hierarchyid::Parse ( input )  
-- This is functionally equivalent to the following syntax   
-- which implicitly calls Parse():  
CAST ( input AS hierarchyid )  
```  
  
```csharp
-- CLR syntax  
static SqlHierarchyId Parse ( SqlString input )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*input*  
[!INCLUDE[tsql](../../includes/tsql-md.md)]：要转换的字符数据类型值。
  
CLR：要计算的字符串值。
  
## <a name="return-types"></a>返回类型  
SQL Server 返回类型：hierarchyid
  
CLR 返回类型：SqlHierarchyId
  
## <a name="remarks"></a>备注  
如果 Parse 收到的值不是 hierarchyid 的有效字符串表示形式，则会引发异常。 例如，如果 char 数据类型包含尾随空格，则会引发异常。
  
## <a name="examples"></a>示例  
  
### <a name="a-converting-transact-sql-values-without-a-table"></a>A. 不使用表转换 Transact-SQL 值  
下面的代码示例使用 `ToString` 将 hierarchyid 值转换为字符串，并使用 `Parse` 将字符串值转换为 hierarchyid。
  
```sql
DECLARE @StringValue AS NVARCHAR(4000), @hierarchyidValue AS hierarchyid  
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
  
### <a name="b-clr-example"></a>B. CLR 示例  
下面的代码段调用 Parse() 方法：
  
```csharp
string input = "/1/2/";  
SqlHierarchyId.Parse(input);  
```  
  
## <a name="see-also"></a>另请参阅
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)  
[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
