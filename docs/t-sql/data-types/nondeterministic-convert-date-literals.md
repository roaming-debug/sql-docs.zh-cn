---
description: 文字日期字符串转换为日期值的不确定性转换
title: 日期文字的不确定性转换 | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 74e4df5fc33b90ae9e455d566b41e6eae71408ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187779"
---
# <a name="nondeterministic-conversion-of-literal-date-strings-into-date-values"></a>文字日期字符串转换为日期值的不确定性转换

允许字符串转换为日期数据类型时请格外小心。 这是因为这种转换通常具有不确定性。

可以通过考虑使用 [SET LANGUAGE](../statements/set-language-transact-sql.md) 和 [SET DATEFORMAT](../statements/set-dateformat-transact-sql.md) 的设置来控制这些不确定性转换。



## <a name="set-language-example-month-name-in-polish"></a>SET LANGUAGE 示例：波兰语中的月份名称

- `SET LANGUAGE Polish;`

字符串可以是某月份的名称。 但是该名称是英语、波兰语、克罗地亚语还是另一种语言？ 以及，用户的会话是否将被设置为正确的相应语言？

例如，可以考虑使用单词 listopad，这是月份的名称。 但它究竟是哪一月要取决于 SQL 系统选择使用的语言：
- 如果是波兰语，那么 listopad 将被翻译为 11 月（用英语表达为“November”）。
- 如果是克罗地亚语，那么 listopad 将被翻译为 10 月（用英语表达为“October”）。

#### <a name="code-example-of-set-language"></a>SET LANGUAGE 的代码示例

```sql
--SELECT alias FROM sys.syslanguages ORDER BY alias;

DECLARE @yourInputDate  NVARCHAR(32) = '28 listopad 2018';

SET LANGUAGE Polish;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Polish];

SET LANGUAGE Croatian;
SELECT CONVERT(DATE, @yourInputDate) AS [SL_Croatian];

SET LANGUAGE English;


/***  Actual output:  For the two months, note the 11 versus the 10.
SL_Polish
2018-11-28

SL_Croatian
2018-10-28
**_/
```



## <a name="set-dateformat-example"></a>SET DATEFORMAT 示例

- `SET DATEFORMAT dmy;`

前面的 dmy 格式表示示例日期字符串“01-03-2018”将被解释为表示 2018 年 3 月的第 1 天。

如果指定为 mdy，那么相同的“01-03-2018”字符串将代表 2018 年 1 月的第 3 天。

如果指定为 ymd，则无法保证输出的结果是什么。 “2018”的数值太大无法表示为一天。
<!--
The preceding claim of "no guarantee" might be incorrect, in the minds of the SQL query engine Developer team?
-->

#### <a name="specific-countries"></a>特定国家/地区

在日本和中国，使用 ymd 的 DATEFORMAT。 格式的部分为从最大单位到最小单位的合理顺序。 因此，此格式排序良好。 此格式被视为国际性格式。 之所以称其为国际性格式，是因为年份的四位数字是明确的，并且地球上没有一个国家/地区使用古老的 ydm 格式。

在其他国家/地区（例如德国和法国），DATEFORMAT 为 dmy，意思是“dd-mm-yyyy”。 该 dmy 格式排序不好，但它是最小单位到最大单位的合理顺序。

美国和密克罗尼西亚联邦是唯一使用 mdy 的国家/地区，此格式并不排序。 此格式的混合顺序与口述日期中的口语模式相匹配。

#### <a name="code-example-of-set-dateformat-mdy-versus-dmy"></a>SET DATEFORMAT 的代码示例：mdy 和 dmy

下面的 Transact-SQL 代码示例使用具有三种不同 DATEFORMAT 设置的相同日期字符串。 运行代码会生成注释中显示的输出：

```sql
DECLARE @yourDateString NVARCHAR(10) = '12-09-2018';
PRINT @yourDateString + '  = the input.';

SET DATEFORMAT dmy;
SELECT CONVERT(DATE, @yourDateString) AS [DMY-Interpretation-of-input-format];

SET DATEFORMAT mdy;
SELECT CONVERT(DATE, @yourDateString) AS [MDY-Interpretation-of-input-format];

SET DATEFORMAT ymd;
SELECT CONVERT(DATE, @yourDateString) AS [YMD-Interpretation--?--NotGuaranteed];


/***  Actual output:
12-09-2018  = the input.

DMY-Interpretation-of-input-format
2018-09-12

MDY-Interpretation-of-input-format
2018-12-09

YMD-Interpretation--?--NotGuaranteed
2018-12-09
**_/
```

在上述代码示例中，最后一个示例的格式 ymd 与输入字符串之间不一致。 输入字符串的第三个节点代表一个太大而无法表示一天的数值。 Microsoft 不保证这种不匹配输出的值。

#### <a name="convert-offers-explicit-codes-for-_deterministic_-control-of-date-formats"></a>CONVERT 提供确定性日期格式控制的显式代码

CAST 和 CONVERT 文档文章列出了可以与 CONVERT 函数一起使用以确定性地控制日期转换的显式代码。 每个月这篇文章都有最高的页面浏览量。

- [CAST 和 CONVERT (Transact SQL)：日期和时间样式](../functions/cast-and-convert-transact-sql.md#date-and-time-styles)
- [CAST 和 CONVERT (Transact-SQL)：某些日期时间的转换具有不确定性](../functions/cast-and-convert-transact-sql.md#certain-datetime-conversions-are-nondeterministic)



## <a name="compatibility-level-90-and-above"></a>兼容性级别 90 及以上

在 SQL Server 2000 中，兼容性级别为 80。 对于级别设置为 80 或以下的，隐式日期的转换是确定性的。

从 SQL Server 2005 开始，兼容性级别为 90，隐式日期的转换已变为不确定性了。 从级别 90 开始，日期转换变成取决于 SET LANGUAGE 和 SET DATEFORMAT。

#### <a name="unicode"></a>Unicode

<!-- The next live sentence needs an explanatory example!  N'somethingHere?'.
-->
非 Unicode 字符数据在排序规则间的转换也被视为具有不确定性。



## <a name="see-also"></a>另请参阅

- [设置会话语言](../../relational-databases/collations/set-a-session-language.md)
- [日期和时间数据类型及函数 (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)
- [FORMAT (Transact-SQL)](../functions/format-transact-sql.md)
- [ISDATE (Transact-SQL)](../functions/isdate-transact-sql.md)



<!--
This new article is linked-to by the following articles (at least initially on 2018/11/19).....
...
* docs/relational-databases/views/create-indexed-views.md
* docs/relational-databases/indexes/indexes-on-computed-columns.md
* docs/t-sql/functions/cast-and-convert-transact-sql.md
...
As a reaction to public PR 1279, this approach of creating a new article to link to is a better alternative than a docs/includes/ approach.
GeneMi (MightyPen), 2018/11/19
-->

