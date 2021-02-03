---
description: sql_variant (Transact-SQL)
title: sql_variant (Transact-SQL)
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql_variant
- sql_variant_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sql_variant comparisons
- storing data [SQL Server], sql_variant
- sql_variant data type
- storage [SQL Server], sql_variant
ms.assetid: 01229779-8bc1-4c7d-890a-8246d4899250
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 3787a49e6488a2b43ba8ef6e7b67c2a94b6347b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179701"
---
# <a name="sql_variant-transact-sql"></a>sql_variant (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

一种数据类型，用于存储 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的各种数据类型的值。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
sql_variant  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注  
sql_variant 可以用在列、参数、变量和用户定义函数的返回值中  。 借助 sql_variant，这些数据库对象可以支持其他数据类型的值  。
  
类型为 sql_variant 的列可能包含不同数据类型的行  。 例如，定义为 sql_variant 的列可以存储 int、binary 和 char 类型的值     。
  
sql_variant 的最大长度可以是 8016 个字节  。 这包括基类型信息和基类型值。 实际基类型值的最大长度是 8,000 个字节。
  
对于 sql_variant 数据类型，必须先将它转换为其基本数据类型值，然后才能参与诸如加减这类运算  。
  
可以为 sql_variant 分配默认值  。 该数据类型还可以将 NULL 作为其基础值，但是 NULL 值没有关联的基类型。 此外，sql_variant 不能使用其他 sql_variant 作为其基础类型   。
  
唯一键、主键或外键可能包含类型为 sql_variant 的列，但是，组成指定行的键的数据值的总长度不应大于索引的最大长度  。 该最大长度是 900 个字节。
  
一个表可以包含任意多个 sql_variant 列  。
  
不能在 CONTAINSTABLE 和 FREETEXTTABLE 中使用 sql_variant  。
  
ODBC 不完全支持 sql_variant  。 因此，使用 Microsoft OLE DB Provider for ODBC (MSDASQL) 时，sql_variant 列的查询将作为二进制数据返回  。 例如，包含字符串数据 'PS2091' 的 sql_variant 列将作为 0x505332303931 返回  。
  
## <a name="comparing-sql_variant-values"></a>比较 sql_variant 值  
sql_variant 数据类型在用于转换的数据类型层次结构列表中位于顶部  。 为了进行 sql_variant 比较，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型层次结构顺序划分为多个数据类型系列。
  
|数据类型层次结构|数据类型系列|  
|---|---|
|**sql_variant**|sql_variant |  
|**datetime2**|日期和时间|  
|**datetimeoffset**|日期和时间|  
|**datetime**|日期和时间|  
|**smalldatetime**|日期和时间|  
|**date**|日期和时间|  
|**time**|日期和时间|  
|**float**|近似数值|  
|**real**|近似数值|  
|**decimal**|精确数值|  
|**money**|精确数值|  
|**smallmoney**|精确数值|  
|**bigint**|精确数值|  
|**int**|精确数值|  
|**smallint**|精确数值|  
|**tinyint**|精确数值|  
|**bit**|精确数值|  
|**nvarchar**|Unicode|  
|**nchar**|Unicode|  
|**varchar**|Unicode|  
|**char**|Unicode|  
|**varbinary**|Binary|  
|**binary**|Binary|  
|**uniqueidentifier**|Uniqueidentifier |  
  
下列规则适用于 sql_variant 比较  ：
-   当不同基本数据类型的 sql_variant 值进行比较，而且基本数据类型属于不同的数据类型系列时，则在层次结构图中数据类型系列较高的值被认为在两个值中较大  。  
-   当不同基本数据类型的sql_variant 值进行比较，而且基本数据类型属于相同的数据类型系列时，则在层次结构图中基本数据类型较低的值先隐式转换为其他数据类型，然后再进行比较  。  
-   在比较 char、varchar、nchar 或 nvarchar 数据类型的 sql_variant 值时，将首先基于以下条件来比较这些值的排序规则：LCID、LCID 版本、比较标志和排序 ID      。 其中的每个条件都按所列出的顺序作为整数值进行比较。 如果所有这些条件都相等，则将按照排序规则来比较实际的字符串值。  
  
## <a name="converting-sql_variant-data"></a>转换 sql_variant 数据  
当处理 sql_variant 数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持将其他数据类型的对象隐式转换为 sql_variant 类型。 但是，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持从 sql_variant 数据隐式转换为其他数据类型的对象  。
  
## <a name="restrictions"></a>限制

下表列出了无法使用 sql_variant 存储的值的类型：

- **datetimeoffset**<sup>1</sup>
- **地理**
- **geometry**
- **hierarchyid**
- **图像**
- **ntext**
- **nvarchar(max)**
- **rowversion** (**timestamp**)
- **text**
- **varchar(max)**
- **varbinary(max)**
- **sql_variant**
- 用户定义类型
- **xml**

<sup>1</sup> SQL Server 2012 及更高版本均不限制 datetimeoffset  。

## <a name="examples"></a>示例  

### <a name="a-using-a-sql_variant-in-a-table"></a>A. 在表中使用 sql_variant  
 下面的示例创建一个 sql_variant 数据类型的表。 然后该示例检索有关 `colA` 值 `46279.1` 的 `SQL_VARIANT_PROPERTY` 信息，其中，`colB` =`1689`，并假设 `tableA` 有类型为 `sql_variant` 和 `colB` 的 `colA`。  
  
```sql    
CREATE TABLE tableA(colA sql_variant, colB INT)  
INSERT INTO tableA values ( CAST(46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE     colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]请注意，这三个值中的每一个都是 sql_variant  。  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. 使用 sql_variant 作为变量   
 下面的示例使用 sql_variant 数据类型创建了一个变量，然后检索名为 @v1 的变量的 `SQL_VARIANT_PROPERTY` 信息。  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```    


## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[SQL_VARIANT_PROPERTY (Transact-SQL)](../../t-sql/functions/sql-variant-property-transact-sql.md)
  
  
