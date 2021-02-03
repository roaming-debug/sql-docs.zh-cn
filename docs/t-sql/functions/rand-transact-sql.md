---
description: RAND (Transact-SQL)
title: RAND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RAND
- RAND_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RAND function
- values [SQL Server], random float
- random float value
ms.assetid: 363c84d6-b9fa-49ba-9a75-e44f27535ff6
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea82aba7de9d9f306518398b3cdc9312aa972607
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182339"
---
# <a name="rand-transact-sql"></a>RAND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  返回一个介于 0 到 1（不包括 0 和 1）之间的伪随机 float 值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  

```syntaxsql  
RAND ( [ seed ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数
 seed  
 提供种子值的整数[表达式](../../t-sql/language-elements/expressions-transact-sql.md)（tinyint、smallint 或 int）。 如果未指定 seed，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 随机分配种子值。 对于指定的种子值，返回的结果始终相同。  
  
## <a name="return-types"></a>返回类型  
 **float**  
  
## <a name="remarks"></a>备注  
 使用同一个种子值重复调用 RAND() 会返回相同的结果。  
  
 对于一个连接，如果使用指定的种子值调用 RAND()，则 RAND() 的所有后续调用将基于使用该指定种子值的 RAND() 调用生成结果。 例如，以下查询将始终返回相同的数字序列。  
  
```sql  
SELECT RAND(100), RAND(), RAND()   
```  
  
## <a name="examples"></a>示例  
 以下示例将产生由 RAND 函数生成的四个不同的随机数。  
  
```sql  
DECLARE @counter SMALLINT;  
SET @counter = 1;  
WHILE @counter < 5  
   BEGIN  
      SELECT RAND() Random_Number  
      SET @counter = @counter + 1  
   END;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  
  
  
