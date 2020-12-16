---
title: 数据类型 (Transact-SQL) | Microsoft Docs
description: 本文概要介绍 SQL Server 中可用的不同数据类型。
ms.date: 09/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system data types [SQL Server]
- data types [SQL Server]
- data types [SQL Server], about data types
ms.assetid: a54f7373-b247-4d61-8fb8-7f2ec7a8d0a4
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5039e7ea667c80b051413b93fbb34d2f4adc1038
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484329"
---
# <a name="data-types-transact-sql"></a>数据类型 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，每个列、局部变量、表达式和参数都具有一个相关的数据类型。 数据类型是一种属性，用于指定对象可保存的数据的类型：整数数据、字符数据、货币数据、日期和时间数据、二进制字符串等。
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供系统数据类型集，该类型集定义了可与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一起使用的所有数据类型。 还可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 定义自己的数据类型。 别名数据类型基于系统提供的数据类型。 有关别名数据类型的详细信息，请参阅 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)。 用户定义类型从您使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 支持的编程语言之一创建的类的方法和运算符中获取它们的特征。
  
当两个具有不同数据类型、排序规则、精度、小数位数或长度的表达式通过运算符进行组合时，结果的特征由以下规则确定：
-   结果的数据类型是通过将数据类型的优先顺序规则应用到输入表达式的数据类型来确定的。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
-   当结果数据类型为 char、varchar、text、nchar、nvarchar 或 ntext 时，结果的排序规则由排序规则的优先顺序规则确定       。 有关详细信息，请参阅[排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。  
-   结果的精度、小数位数及长度取决于输入表达式的精度、小数位数及长度。 有关详细信息，请参阅[精度、小数位数和长度 (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了数据类型同义词，以便实现 ISO 兼容性。 有关详细信息，请参阅[数据类型同义词 (Transact-SQL)](../../t-sql/data-types/data-type-synonyms-transact-sql.md)。
  
## <a name="data-type-categories"></a>数据类型类别
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据类型归纳为下列类别：
  
:::row:::
    :::column:::
        精确数字
    :::column-end:::
    :::column:::
        Unicode 字符串
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        近似数字
    :::column-end:::
    :::column:::
        二进制字符串
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        日期和时间
    :::column-end:::
    :::column:::
        其他数据类型
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        字符串
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，根据其存储特征，某些数据类型被指定为属于下列各组：
-   大值数据类型：varchar(max) 和 nvarchar(max)    
-   大型对象数据类型：text、ntext、image、varbinary(max) 和 xml       
  
    > [!NOTE]  
    >  sp_help 返回 -1 作为较大值数据类型和 xml 数据类型的长度  。  
  
### <a name="exact-numerics"></a>精确数字
  
:::row:::
    :::column:::
        [bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
    :::column:::
        [numeric](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [bit](../../t-sql/data-types/bit-transact-sql.md)
    :::column-end:::
    :::column:::
        [smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [decimal](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)
    :::column-end:::
    :::column:::
        [smallmoney](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
    :::column:::
        [tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="approximate-numerics"></a>近似数字
  
:::row:::
    :::column:::
        [float](../../t-sql/data-types/float-and-real-transact-sql.md)
    :::column-end:::
    :::column:::
        [real](../../t-sql/data-types/float-and-real-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="date-and-time"></a>日期和时间
  
:::row:::
    :::column:::
        [date](../../t-sql/data-types/date-transact-sql.md)
    :::column-end:::
    :::column:::
        [datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [datetime2](../../t-sql/data-types/datetime2-transact-sql.md)
    :::column-end:::
    :::column:::
        [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [datetime](../../t-sql/data-types/datetime-transact-sql.md)
    :::column-end:::
    :::column:::
        [time](../../t-sql/data-types/time-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
### <a name="character-strings"></a>字符串
  
:::row:::
    :::column:::
        [char](../../t-sql/data-types/char-and-varchar-transact-sql.md)
    :::column-end:::
    :::column:::
        [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [text](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 
### <a name="unicode-character-strings"></a>Unicode 字符串
  
:::row:::
    :::column:::
        [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)
    :::column-end:::
    :::column:::
        [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  

### <a name="binary-strings"></a>二进制字符串
  
:::row:::
    :::column:::
        [binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)
    :::column-end:::
    :::column:::
        [varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [图像](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="other-data-types"></a>其他数据类型

:::row:::
    :::column:::
        [cursor](../../t-sql/data-types/cursor-transact-sql.md)
    :::column-end:::
    :::column:::
        [rowversion](../../t-sql/data-types/rowversion-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [hierarchyid](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
    :::column-end:::
    :::column:::
        [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)
    :::column-end:::
    :::column:::
        [xml](../../t-sql/xml/xml-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [空间几何类型](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md) 
    :::column-end:::
    :::column:::
        [空间地理类型](../../t-sql/spatial-geography/spatial-types-geography.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [table](../../t-sql/data-types/table-transact-sql.md) 
    :::column-end:::
    :::column:::
         
    :::column-end:::
:::row-end:::

  
## <a name="see-also"></a>另请参阅
[CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)  
[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
[函数 (Transact-SQL)](../../t-sql/functions/functions.md)  
[LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)  
[sp_droptype (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droptype-transact-sql.md)  
[sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)  
[sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
  
  
