---
description: TYPE_ID (Transact-SQL)
title: TYPE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TYPE_ID
- TYPE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IDs [SQL Server], types
- TYPE_ID function
- type IDs [SQL Server]
- data types [SQL Server], IDs
ms.assetid: 647d17ef-b878-4922-b446-56642322ebad
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4192aa80f1cbc2d7106d8893f135387c40b5bef5
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379449"
---
# <a name="type_id-transact-sql"></a>TYPE_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回指定数据类型名称的 ID。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TYPE_ID ( [ schema_name ] type_name )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 type_name  
 数据类型的名称。 type_name 的数据类型为 nvarchar。 type_name 可以是系统数据类型或用户定义的数据类型。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看其拥有的安全对象的元数据，或者已对其授予权限的安全对象的元数据。 也就是说，如果用户对该对象没有任何权限，则那些会生成元数据的内置函数（如 TYPE_ID）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>注解  
 如果类型名称无效，或调用方没有足够权限来引用类型，则 TYPE_ID 返回 NULL。  
  
## <a name="examples"></a>示例  
  
### <a name="a-looking-up-the-type-id-values-for-single--and-two-part-type-names"></a>A. 查找由一部分构成的和由两部分构成的类型名称的 TYPE ID 值  
 下面的示例返回由一部分构成的和由两部分构成的类型名称的类型 ID。  
  
```sql
USE tempdb;  
GO  
CREATE TYPE NewType FROM int;  
GO  
CREATE SCHEMA NewSchema;  
GO  
CREATE TYPE NewSchema.NewType FROM int;  
GO  
SELECT TYPE_ID('NewType') AS [1 Part Data Type ID],  
       TYPE_ID('NewSchema.NewType') AS [2 Part Data Type ID];  
GO  
```  
  
### <a name="b-looking-up-the-type-id-of-a-system-data-type"></a>B. 查找系统数据类型的 TYPE ID  
 下面的示例返回 `TYPE ID` 系统数据类型的 `datetime`。  
  
```sql
SELECT TYPE_NAME(TYPE_ID('datetime')) AS [TYPE_NAME]  
    ,TYPE_ID('datetime') AS [TYPE_ID];  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-looking-up-the-type-id-of-a-system-data-type"></a>C. 查找系统数据类型的 TYPE ID  
 下面的示例返回 `TYPE ID` 系统数据类型的 `datetime`。  
  
```sql
SELECT TYPE_NAME(TYPE_ID('datetime')) AS typeName,   
    TYPE_ID('datetime') AS typeID FROM table1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [TYPE_NAME (Transact-SQL)](../../t-sql/functions/type-name-transact-sql.md)   
 [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)   
 [sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
  
  

