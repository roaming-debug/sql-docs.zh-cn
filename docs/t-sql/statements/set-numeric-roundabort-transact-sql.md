---
description: SET NUMERIC_ROUNDABORT (Transact-SQL)
title: SET NUMERIC_ROUNDABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NUMERIC_ROUNDABORT
- SET_NUMERIC_ROUNDABORT_TSQL
- SET NUMERIC_ROUNDABORT
- NUMERIC_ROUNDABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- precision [SQL Server], rounded expressions
- expressions [SQL Server], rounding
- NUMERIC_ROUNDABORT
- SET NUMERIC_ROUNDABORT statement
ms.assetid: d20e74f1-b8da-466c-b180-9d8a8b851a77
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cac574403a39550d7f838544a9d88b42f8f9fdf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100734"
---
# <a name="set-numeric_roundabort-transact-sql"></a>SET NUMERIC_ROUNDABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

指定当表达式中的舍入导致精度损失时生成的错误报告级别。  
  
![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

```syntaxsql

SET NUMERIC_ROUNDABORT { ON | OFF }
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
当 SET NUMERIC_ROUNDABORT 为 ON 时，在表达式中出现精度损失时将生成错误。 如果设置为 OFF，精度的降低不会生成错误消息。 结果会根据存储结果的列或变量的精度进行舍入。  
  
在精度较低的列或变量中，当尝试以固定精度存储值时，会出现精度降低的情况。  
  
如果 SET NUMERIC_ROUNDABORT 为 ON，则 SET ARITHABORT 决定生成错误的严重级别。 该表显示当出现精度损失时这两个设置的效果。  
  
|设置|SET NUMERIC_ROUNDABORT ON|SET NUMERIC_ROUNDABORT OFF|
|-------------|--------------------------------|---------------------------------|
|SET ARITHABORT ON|生成错误；不返回结果集。|没有错误和警告；对结果进行舍入。|  
|SET ARITHABORT OFF|返回警告；表达式返回 NULL。|没有错误和警告；对结果进行舍入。|  

SET NUMERIC_ROUNDABORT 的设置是在执行或运行时设置，而不是在分析时设置。

在对计算列或索引视图创建或更改索引时，SET NUMERIC_ROUNDABORT 必须为 OFF。 如果 SET NUMERIC_ROUNDABORT 为 ON，则对包含计算列或索引视图中索引的表执行以下语句将失败：

- CREATE 
- UPDATE 
- INSERT 
- DELETE 

有关计算列的索引视图和索引所需的 SET 选项设置的详细信息，请参阅[使用 SET 语句时的注意事项](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements)。
  
要查看此设置的当前设置，请运行以下查询：
  
```sql
DECLARE @NUMERIC_ROUNDABORT VARCHAR(3) = 'OFF';  
IF ( (8192 & @@OPTIONS) = 8192 ) SET @NUMERIC_ROUNDABORT = 'ON';  
SELECT @NUMERIC_ROUNDABORT AS NUMERIC_ROUNDABORT;  
  
```  
  
## <a name="permissions"></a>权限  
要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
以下示例显示精确到四个小数位的两个值。 它们会添加并存储到精确到两个小数位的变量中。 这些表达式演示了不同的 `SET NUMERIC_ROUNDABORT` 和 `SET ARITHABORT` 设置所造成的影响。  
  
```sql
-- SET NOCOUNT to ON,   
-- SET NUMERIC_ROUNDABORT to ON, and SET ARITHABORT to ON.  
SET NOCOUNT ON;  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to ON and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to ON.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
[SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
[SET ARITHABORT (Transact-SQL)](../../t-sql/statements/set-arithabort-transact-sql.md)  
  
  
