---
description: DECOMPRESS (Transact-SQL)
title: DECOMPRESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECOMPRESS
- DECOMPRESS_TSQL
helpviewer_keywords:
- DECOMPRESS function
ms.assetid: 738d56be-3870-4774-b112-3dce27becc11
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9d6851056673a896e1b07a26ee5d50142272a31a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96116255"
---
# <a name="decompress-transact-sql"></a>DECOMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

此函数将使用 GZIP 算法解压缩输入表达式值。 `DECOMPRESS` 将返回字节数组（VARBINARY(MAX) 类型）。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
DECOMPRESS ( expression )  
```  
  
## <a name="arguments"></a>参数
 *expression*  
varbinary(n)、varbinary(max) 或 binary(n) 值。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
数据类型 varbinary(max) 的值。 `DECOMPRESS` 将使用 ZIP 算法解压缩输入参数。 如有必要，用户应显式将结果强制转换为目标类型。  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
  
### <a name="a-decompress-data-at-query-time"></a>A. 在查询时解压缩数据  
此示例演示如何返回已压缩的表数据：  
  
```sql  
SELECT _id, name, surname, datemodified,  
             CAST(DECOMPRESS(info) AS NVARCHAR(MAX)) AS info  
FROM player;  
```  
  
### <a name="b-display-compressed-data-using-computed-column"></a>B. 使用计算列显示已压缩数据  
此示例演示如何创建用于解压缩数据存储的表：  
  
```sql  
CREATE TABLE example_table (  
    _id INT PRIMARY KEY IDENTITY,  
    name NVARCHAR(max),  
    surname NVARCHAR(max),  
    info VARBINARY(max),  
    info_json as CAST(DECOMPRESS(info) as NVARCHAR(max))  
);  
```  
  
## <a name="see-also"></a>另请参阅  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [COMPRESS (Transact-SQL)](../../t-sql/functions/compress-transact-sql.md)  
  
  
