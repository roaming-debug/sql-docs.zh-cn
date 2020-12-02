---
description: SCHEMA_ID (Transact-SQL)
title: SCHEMA_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SCHEMA_ID
- SCHEMA_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], schemas
- schemas [SQL Server], IDs
- SCHEMA_ID function
- IDs [SQL Server], schemas
- default schema IDs
ms.assetid: c8e34df5-3eea-459f-ae40-050909ce9fda
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2cd9fc9bc52a69fb5200abaf2bd5f678fe4a1f73
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379948"
---
# <a name="schema_id-transact-sql"></a>SCHEMA_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回与架构名称关联的架构 ID。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
SCHEMA_ID ( [ schema_name ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
  
|术语|定义|  
|----------|----------------|  
|*schema_name*|是架构的名称。 schema_name 为 sysname。 如果未指定 schema_name，则 SCHEMA_ID 将返回调用方的默认架构的 ID。|  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
 如果 schema_name 不是有效架构，将返回 NULL。  
  
## <a name="remarks"></a>备注  
 SCHEMA_ID 将返回系统架构的 ID 和用户定义架构的 ID。 SCHEMA_ID 可以在选择列表、WHERE 子句和任何允许使用表达式的地方调用。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-default-schema-id-of-a-caller"></a>A. 返回调用方的默认架构 ID  
  
```sql  
SELECT SCHEMA_ID();  
```  
  
### <a name="b-returning-the-schema-id-of-a-named-schema"></a>B. 返回命名架构的架构 ID  
  
```sql  
SELECT SCHEMA_ID('dbo');  
```  
  
## <a name="see-also"></a>另请参阅  
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SCHEMA_NAME (Transact-SQL)](../../t-sql/functions/schema-name-transact-sql.md)   
 [sys.schemas (Transact-SQL)](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)  
  
  

