---
description: COL_LENGTH (Transact-SQL)
title: COL_LENGTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COL_LENGTH
- COL_LENGTH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lengths [SQL Server], columns
- COL_LENGTH function
- column properties [SQL Server]
- column length [SQL Server]
ms.assetid: cf891206-c49f-40eb-858e-eefd2b638a33
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1204ea1a23a8548e8d221fccb27f5190de45eec7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211479"
---
# <a name="col_length-transact-sql"></a>COL_LENGTH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回定义的列长度（以字节为单位）。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
COL_LENGTH ( 'table' , 'column' )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**'** *table* **'**  
要确定其列长度信息的表的名称。 table 是 nvarchar 类型的表达式。
  
**'** *column* **'**  
要确定其长度的列名称。 column 是 nvarchar 类型的表达式。
  
## <a name="return-type"></a>返回类型
**smallint**
  
## <a name="exceptions"></a>例外  
出现错误时或调用方没有查看对象的正确权限时将返回 NULL。
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看其所拥有的安全对象的元数据，或者已对其授予权限的安全对象的元数据。 这意味着，如果用户对该对象没有正确的权限，那些发出元数据的内置函数（如 COL_LENGTH）则可能会返回 NULL。 有关详细信息，请参阅[元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md)。
  
## <a name="remarks"></a>备注  
对于使用 max 说明符 (varchar(max)) 声明的 varchar 列，COL_LENGTH 将返回值 -1。
  
## <a name="examples"></a>示例  
此示例将显示类型为 `varchar(40)` 和 `nvarchar(40)` 的列的返回值：
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE t1(c1 VARCHAR(40), c2 NVARCHAR(40) );  
GO  
SELECT COL_LENGTH('t1','c1')AS 'VarChar',  
      COL_LENGTH('t1','c2')AS 'NVarChar';  
GO  
DROP TABLE t1;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
VarChar     NVarChar  
40          80  
```  
  
## <a name="see-also"></a>另请参阅
[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
[元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
[COL_NAME (Transact-SQL)](../../t-sql/functions/col-name-transact-sql.md)  
[COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md)
  
  
