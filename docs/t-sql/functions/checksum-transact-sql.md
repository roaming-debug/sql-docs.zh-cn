---
description: CHECKSUM (Transact-SQL)
title: CHECKSUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_TSQL
- CHECKSUM
dev_langs:
- TSQL
helpviewer_keywords:
- hash indexes
- CHECKSUM function
- checksum values
ms.assetid: e26d3339-845c-49c2-9d89-243376874c13
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f27205e76df6ba323ae586a84a19dcf05b654aa9
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067366"
---
# <a name="checksum-transact-sql"></a>CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

`CHECKSUM` 函数返回按照表的某一行或一组表达式计算出来的校验和值。 使用 `CHECKSUM` 来生成哈希索引。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CHECKSUM ( * | expression [ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数
\*  
此参数指定涵盖了所有表列的校验和计算。 如果有任一列具有非可比数据类型，则 `CHECKSUM` 返回错误。 非可比数据类型包括：

- **cursor**
- **图像**
- **ntext**
- **text**
- **XML**

另一个非可比数据类型为，以上述任一数据类型作为基类型的 sql_variant。
  
*expression*  
除非可比数据类型之外的任何类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。
  
## <a name="return-types"></a>返回类型
 **int**  
  
## <a name="remarks"></a>备注  
`CHECKSUM` 对其参数列表计算一个称为校验和的哈希值。 使用哈希值生成哈希索引。 如果 `CHECKSUM` 函数具有列参数，则结果是一个哈希索引，并且对计算的 `CHECKSUM` 值生成索引。 它可用于对列进行等价搜索。
  
`CHECKSUM` 函数满足哈希函数的属性：`CHECKSUM` 在使用等于 (=) 运算符比较时，如果两个列表的相应元素具有相同数据类型且对应的元素相等，则在任何两个表达式列表上应用的 BINARY_CHECKSUM 将返回同一值。 因为 `CHECKSUM` 函数，指定类型的 Null 值被定义为相等进行比较。 如果表达式列表中的至少一个值发生更改，则列的校验和很可能也会更改。 但是，这一点无法保证。 因此，若要检测值是否更改，建议仅当应用程序可以容忍偶然错过更改时，使用 `CHECKSUM`。 否则，请考虑改用 `HASHBYTES`。 使用指定的 MD5 哈希算法时，`HASHBYTES` 为两个不同输入返回相同结果的可能性要比 `CHECKSUM` 小得多。
  
表达式顺序会影响计算 `CHECKSUM` 值。 用于 `CHECKSUM(*)` 的列顺序是在表或视图定义中指定的列顺序。 其中包括计算列。
  
`CHECKSUM` 值取决于排序规则。 使用不同排序规则存储的相同值将返回一个不同的 `CHECKSUM` 值。
  
`CHECKSUM ()` 不保证结果唯一。

## <a name="examples"></a>示例  
这些示例显示了如何使用 `CHECKSUM` 来生成哈希索引。
  
为生成哈希索引，第一个示例将已计算的校验和列添加到我们想要索引的表。 然后在校验和列生成索引。 
  
```sql
-- Create a checksum index.  

SET ARITHABORT ON;  
USE AdventureWorks2012;   
GO  
ALTER TABLE Production.Product  
ADD cs_Pname AS CHECKSUM(Name);  
GO  
CREATE INDEX Pname_index ON Production.Product (cs_Pname);  
GO  
```  
  
此示例显示将校验和索引用作哈希索引。 当索引的列为长字符列时，这可加快索引的速度。 校验和索引可用于等价搜索。
  
```sql
/*Use the index in a SELECT query. Add a second search   
condition to catch stray cases where checksums match,   
but the values are not the same.*/  

SELECT *   
FROM Production.Product  
WHERE CHECKSUM(N'Bearing Ball') = cs_Pname  
AND Name = N'Bearing Ball';  
GO  
```  
  
对计算列的索引创建将具体化为校验和列，对 `ProductName` 值所做的任何更改都将传播到校验和列。 或者，我们可以在想要索引的列上直接生成索引。 但是，对于长密钥值，常规索引的执行很可能不如校验和索引。
  
## <a name="see-also"></a>另请参阅
[CHECKSUM_AGG (Transact-SQL)](../../t-sql/functions/checksum-agg-transact-sql.md)  
[HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  (Transact-SQL)](../../t-sql/functions/binary-checksum-transact-sql.md)
  
  
