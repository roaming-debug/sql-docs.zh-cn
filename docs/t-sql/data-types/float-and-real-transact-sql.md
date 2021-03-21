---
description: float 和 real (Transact-SQL)
title: float 和 real (Transact-SQL)
ms.custom: ''
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- float
- real_TSQL
- real
- float_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- numeric data type, floating point
- float data type
- floating point data [SQL Server]
- real data type
ms.assetid: 08ea66b7-624e-4d8b-86bc-750ff76cdfc5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab78f62c182c2daaa9de67d1e19a5711dc932d94
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752727"
---
# <a name="float-and-real-transact-sql"></a>float 和 real (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用于表示浮点数值数据的大致数值数据类型。 浮点数据为近似值；因此，并非数据类型范围内的所有值都能精确地表示。 real 的 ISO 同义词为 float(24)   。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
float [ (_n_) ] 其中 n 为用于存储 float 数值尾数的位数（以科学记数法表示），因此可以确定精度和存储大小  。 如果指定了 n，则它必须是介于 1 和 53 之间的某个值    。 n 的默认值为 53   。
  
|n 值|Precision|存储大小|  
|---|---|---|
|**1-24**|7 位数|4 个字节|  
|**25-53**|15 位数|8 字节|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将 n 视为下列两个可能值之一  。 如果 1<=n<=24，将 n 视为 24     。 如果 25<=n<=53，将 n 视为 53     。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] float[(n)] 数据类型从 1 到 53 之间的所有 n 值均符合 ISO 标准      。 double precision 的同义词是 float(53)   。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注  
  
|数据类型|范围|存储|  
|---|---|---|
|**float**|-1.79E + 308 至 -2.23E - 308、0 以及 2.23E - 308 至 1.79E + 308|取决于 n 的值|  
|**real**|-3.40E + 38 至 -1.18E - 38、0 以及 1.18E - 38 至 3.40E + 38|4 个字节|  
  
##  <a name="converting-float-and-real-data"></a>转换 float 和 real 数据  
如果将 float 值转换为任一整数类型，这些值将被截断  。
  
若要将 float 或 real 转换为字符数据，使用 STR 字符串函数通常比使用 CAST( ) 更有用   。 这是因为 STR 能够对格式进行更严格的控制。 有关详细信息，请参阅 [STR (Transact-SQL)](../../t-sql/functions/str-transact-sql.md) 和[函数 (Transact-SQL)](../../t-sql/functions/functions.md)。
  
在进行 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 之前，浮点值到 decimal 或 numeric 的转换仅限于精度为 17 位数字的值    。 小于 5E-18 的任何浮点  值（使用 5E-18 的科学计数法或 0.0000000000000000050000000000000005 十进制表示法设置时）都舍入为 0。 从 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 起，不再具有该限制。
  
## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
