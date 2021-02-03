---
title: STRING_SPLIT (Transact-SQL) | Microsoft Docs
description: STRING_SPLIT 函数的 Transact-SQL 参考。 此表值函数根据字符分隔符将字符串拆分为子字符串。
ms.date: 11/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_SPLIT
- STRING_SPLIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_SPLIT function
ms.assetid: 3273dbf3-0b4f-41e1-b97e-b4f67ad370b9
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 472da04cc9a93a8f7846927ad9ae4595766e9116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207884"
---
# <a name="string_split-transact-sql"></a>STRING_SPLIT (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

一个表值函数，它根据指定的分隔符将字符串拆分为子字符串行。

#### <a name="compatibility-level-130"></a>兼容性级别为 130

STRING_SPLIT 要求兼容性级别至少为 130。 级别低于 130 时，SQL Server 找不到 STRING_SPLIT 函数。

若要更改数据库的兼容性级别，请参阅[查看或更改数据库的兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)。

> [!NOTE]
> 在 Azure Synapse Analytics 中，无需对 STRING_SPLIT 进行兼容性配置。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  

```syntaxsql
STRING_SPLIT ( string , separator )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

 *string*  
 任何字符类型（例如 nvarchar、varchar、nchar 或 char）的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)     。  
  
 separator   
 任何字符类型（例如 nvarchar(1)、varchar(1)、nchar(1) 或 char(1)）的单字符[表达式](../../t-sql/language-elements/expressions-transact-sql.md)，用作串联子字符串的分隔符     。  
  
## <a name="return-types"></a>返回类型  

返回其行是子字符串的单列表。 该列名为“value”  。 如果任何输入参数为 nvarchar 或 nchar，则返回 nvarchar    。 否则，返回 varchar  。 返回类型的长度与字符串参数的长度相同。  
  
## <a name="remarks"></a>备注  

STRING_SPLIT 输入包含分隔子字符串的字符串，并输入一个字符用作分隔符  。 STRING_SPLIT 输出其行包含子字符串的单列表。 输出列的名称为“value”  。

输出行可以按任意顺序排列。 顺序不保证与输入字符串中的子字符串顺序匹配  。 可以通过在 SELECT 语句中使用 ORDER BY 子句覆盖最终排序顺序 (`ORDER BY value`)。

0x0000 (char(0)) 是 Windows 排序规则中未定义的字符，不能包括在 STRING_SPLIT 中  。

当输入字符串包含两个或多个连续出现的分隔符字符时，将出现长度为零的空子字符串。 空子字符串的处理方式与普通子字符串相同。 可以通过使用 WHERE 子句筛选出包含空的子字符串的任何行 (`WHERE value <> ''`)。 如果输入字符串为 NULL，则 STRING_SPLIT 表值函数返回一个空表。  

例如，以下 SELECT 语句使用空格字符作为分隔符：

```sql
SELECT value FROM STRING_SPLIT('Lorem ipsum dolor sit amet.', ' ');
```

在实践运行中，前面的 SELECT 返回以下结果表：  
  
|值|  
| :-- |  
|Lorem|  
|ipsum|  
|dolor|  
|sit|  
|amet.|  
| &nbsp; |

## <a name="examples"></a>示例  
  
### <a name="a-split-comma-separated-value-string"></a>A. 拆分逗号分隔值字符串

分析逗号分隔值列表，并返回所有非空标记：  

```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike'  
  
SELECT value  
FROM STRING_SPLIT(@tags, ',')  
WHERE RTRIM(value) <> '';
```

如果分隔符之间没有任何内容，STRING_SPLIT 将返回空字符串。 条件 RTRIM(value) <> '' 将删除空标记。  
  
### <a name="b-split-comma-separated-value-string-in-a-column"></a>B. 拆分一列中的逗号分隔值字符串

生产表中的某一列为逗号分隔的标记列表，如以下示例所示：  
  
|ProductId|名称|Tags|  
|---------------|----------|----------|  
|1|Full-Finger Gloves|clothing,road,touring,bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike,mountain|  
  
下面的查询转换每个标记列表，并将它们与原始行联接起来：  

```sql  
SELECT ProductId, Name, value  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',');  
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|ProductId|名称|值|  
|---------------|----------|-----------|  
|1|Full-Finger Gloves|clothing|  
|1|Full-Finger Gloves|道路|  
|1|Full-Finger Gloves|touring|  
|1|Full-Finger Gloves|bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike|  
|3|HL Mountain Frame|mountain|  

  >[!NOTE]
  > 输出的顺序不保证与输入字符串中的子字符串顺序匹配  。
  
### <a name="c-aggregation-by-values"></a>C. 按值聚合

用户必须创建一个报表，表中显示每个标记的产品数量并按产品数量排序，然后只筛选出产品数量在两个以上的标记。  

```sql  
SELECT value as tag, COUNT(*) AS [Number of articles]  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',')  
GROUP BY value  
HAVING COUNT(*) > 2  
ORDER BY COUNT(*) DESC;  
```

### <a name="d-search-by-tag-value"></a>D. 按标记值搜索

开发人员必须创建按关键字查找文章的查询。 可以使用以下查询：  
  
查找具有单个标记 (clothing) 的产品：  

```sql
SELECT ProductId, Name, Tags  
FROM Product  
WHERE 'clothing' IN (SELECT value FROM STRING_SPLIT(Tags, ','));  
```

查找具有两个指定标记（clothing 和 road）的产品：  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE EXISTS (SELECT *  
    FROM STRING_SPLIT(Tags, ',')  
    WHERE value IN ('clothing', 'road'));  
```

### <a name="e-find-rows-by-list-of-values"></a>E. 按一系列值查找行

开发人员必须创建一个按 ID 列表查找文章的查询。 可以使用以下查询：  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
JOIN STRING_SPLIT('1,2,3',',')
    ON value = ProductId;  
```  

上述 STRING_SPLIT 使用情况是常见反模式的替代。 此类反模式可能涉及在应用程序层或 Transact-SQL 中创建动态 SQL 字符串。 或者可以通过使用 LIKE 运算符来实现反模式。 请参阅以下示例 SELECT 语句：

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE ',1,2,3,' LIKE '%,' + CAST(ProductId AS VARCHAR(20)) + ',%';  
```

## <a name="see-also"></a>另请参阅

- [LEFT (Transact-SQL)](../../t-sql/functions/left-transact-sql.md)
- [LTRIM (Transact-SQL)](../../t-sql/functions/ltrim-transact-sql.md)
- [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)
- [RTRIM (Transact-SQL)](../../t-sql/functions/rtrim-transact-sql.md)
- [SUBSTRING (Transact-SQL)](../../t-sql/functions/substring-transact-sql.md)
- [TRIM (Transact-SQL)](../../t-sql/functions/trim-transact-sql.md)
- [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)
