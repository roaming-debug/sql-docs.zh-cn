---
description: SQL Server 排序规则名称 (Transact-SQL)
title: SQL Server 排序规则名称 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], SQL collations
- SQL collations
- names [SQL Server], collations
ms.assetid: 56483d24-add7-483d-9b96-c6fda460ddbc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 092edef1c9e3c03b2d9c8b96569c131048a064d1
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489447"
---
# <a name="sql-server-collation-name-transact-sql"></a>SQL Server 排序规则名称 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则指定排序规则名称的单个字符串。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 Windows 排序规则。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还支持有限数量（小于 80 个）的排序规则（称为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则），这些规则是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的 Windows 排序规则之前开发的。 仍然支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则只是为了实现向后兼容性，不应将这些排序规则用于新开发工作。 有关 Windows 排序规则的详细信息，请参阅 [Windows 排序规则名称](../../t-sql/statements/windows-collation-name-transact-sql.md)。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
<SQL_collation_name> :: =
SQL_SortRules[_Pref]_CPCodepage_<ComparisonStyle>

<ComparisonStyle> ::=
_CaseSensitivity_AccentSensitivity | _BIN
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

SortRules 标识字母表或语言的字符串，当指定按字典排序时应用该字母表或语言的排序规则。 例如 Latin1_General 或波兰语。

Pref 指定大写字母优先。 即使比较时不区分大小写，在没有其他区别的情况下，大写字母也将排在小写字母之前。

Codepage 指定用于标识排序规则所使用的代码页的 1 至 4 位数号码。 **CP1** 指定代码页 1252，对于其他所有代码页，则需指定完整的代码页编号。 例如，**CP1251** 指定代码页 1251，**CP850** 指定代码页 850。

CaseSensitivity
CI 指定不区分大小写，CS 指定区分大小写。

AccentSensitivity
 AI 指定不区分重音，AS 指定区分重音。

BIN 指定使用二进制排序顺序。

## <a name="remarks"></a>注解

若要列出您的服务器支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则，请执行以下查询。

```sql
SELECT * FROM sys.fn_helpcollations()
WHERE name LIKE 'SQL%';
```

> [!NOTE]
> 对于排序顺序 ID 80，请使用代码页为 1250 的任何 Window 排序规则，并使用二进制顺序。 例如：Albanian_BIN、Croatian_BIN、Czech_BIN、Romanian_BIN、Slovak_BIN、Slovenian_BIN。

## <a name="see-also"></a>另请参阅

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [常量](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [table](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
