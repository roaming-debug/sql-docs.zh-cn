---
description: SOUNDEX (Transact-SQL)
title: SOUNDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SOUNDEX
- SOUNDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SOUNDEX function
- comparing string data
- strings [SQL Server], similarity
- strings [SQL Server], comparing
- SOUNDEX values
ms.assetid: 8f1ed34e-8467-4512-a211-e0f43dee6584
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22cd09ddfcd42223f2dd675dcd7ca76e6abcad48
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468158"
---
# <a name="soundex-transact-sql"></a>SOUNDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回一个由四个字符组成的代码 (SOUNDEX)，用于评估两个字符串的相似性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SOUNDEX ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *character_expression*  
 字符数据的字母数字[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 character_expression 可以是常量、变量或列。  
  
## <a name="return-types"></a>返回类型  
 **varchar**  
  
## <a name="remarks"></a>备注  
 SOUNDEX 会根据字符串的发音，将字母数字字符串转换成一个由四个字符组成的代码。 该代码的第一个字符是 character_expression 的第一个字符，已转换为大写。 代码的第二个字符到第四个字符是表示表达式中的字母的数字。 除非字母 A、E、I、O、U、H、W 和 Y 是字符串的首字母，否则将忽略这些字母。 如果需要生成一个四字符代码，将在末尾添加零。 有关 SOUNDEX 代码的详细信息，请参阅 [Soundex 索引系统](https://www.archives.gov/research/census/soundex.html)。  
  
 可比较不同字符串中的 SOUNDEX 代码以查看这些字符串发音的相似度。 DIFFERENCE 函数在两个字符串上执行一个 SOUNDEX，并返回一个整数，表示这些字符串的 SOUNDEX 代码的相似度。  
  
 SOUNDEX 区分排序规则。 可以嵌套字符串函数。  
  
## <a name="soundex-compatibility"></a>SOUNDEX 兼容性  
 在之前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，SOUNDEX 函数应用了 SOUNDEX 规则的子集。 在数据库兼容级别 110 或更高级别下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用一组更完整的规则。  
  
 在升级到兼容级别 110 或更高级别后，可能需要重新生成使用 SOUNDEX 函数的索引、堆或 CHECK 约束。  
  
-   包含使用 SOUNDEX 定义的持久化计算列的堆无法查询，直到通过运行语句 `ALTER TABLE <table> REBUILD` 重新生成该堆。  
  
-   在升级后禁用使用 SOUNDEX 定义的 CHECK 约束。 若要启用该约束，请运行语句 `ALTER TABLE <table> WITH CHECK CHECK CONSTRAINT ALL`。  
  
-   包含使用 SOUNDEX 定义的持久化计算列的索引（包括索引视图）无法查询，直到通过运行语句 `ALTER INDEX ALL ON <object> REBUILD` 重新生成该索引。  
  
## <a name="examples"></a>示例  
 以下示例显示了 SOUNDEX 函数及相关的 DIFFERENCE 函数。 在第一个示例中，返回所有辅音字母的标准 `SOUNDEX` 值。 对 `SOUNDEX` 和 `Smith` 运行 `Smythe` 会返回相同的结果，因为不会包括所有元音字母、字母 `y`、双写字母和字母 `h`。  
  
```sql
-- Using SOUNDEX  
SELECT SOUNDEX ('Smith'), SOUNDEX ('Smythe');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] 对 Latin1_General 排序规则有效。  
  
```  
S530  S530    
```  
  
 `DIFFERENCE` 函数用于比较 `SOUNDEX` 模式结果的差异。 以下示例显示两个仅元音字母不同的字符串。 返回的差异为 `4`（可能的最小差异）。  
  
```sql
-- Using DIFFERENCE  
SELECT DIFFERENCE('Smithers', 'Smythers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] 对 Latin1_General 排序规则有效。  
  
```  
4             
```  
  
 在以下示例中，字符串的辅音字母不同；所以，返回的差异为 `2`，表示差异更大。  
  
```sql
SELECT DIFFERENCE('Anothers', 'Brothers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] 对 Latin1_General 排序规则有效。  
  
```  
2             
```  
  
## <a name="see-also"></a>另请参阅  
 [DIFFERENCE (Transact-SQL)](../../t-sql/functions/difference-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
  

