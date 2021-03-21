---
description: nchar 和 nvarchar (Transact-SQL)
title: nchar 和 nvarchar (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nvarchar data type
- nchar data type
ms.assetid: 81ee5637-ee31-4c4d-96d0-56c26a742354
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 11205430c39bf127d7875e820158ea9bfe7cfbe2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747757"
---
# <a name="nchar-and-nvarchar-transact-sql"></a>nchar 和 nvarchar (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

字符数据类型 nchar（大小固定）或 nvarchar（大小可变）。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 起，使用启用了[补充字符 (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) 的排序规则时，这些数据类型会存储 [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) 字符数据的整个范围，并使用 [UTF-16](https://www.wikipedia.org/wiki/UTF-16) 字符编码。 若指定了非 SC 排序规则，则这些数据类型仅会存储 [UCS-2](https://www.wikipedia.org/wiki/Universal_Coded_Character_Set#Encoding_forms) 字符编码支持的字符数据子集。

## <a name="arguments"></a>参数
**nchar** [ ( n ) ]  
固定大小字符串数据。 n 用于定义字符串大小（以双字节为单位），并且它必须为 1 到 4,000 之间的值。 存储大小为 n 字节的两倍。 对于 [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF) 编码，存储大小为 n 个字节的两倍，并且可存储的字符数也为 n。 对于 UTF-16 编码，存储大小仍为 n 个字节的两倍，但可存储的字符数可能小于 n，因为补充字符使用两个双字节（也称为[代理项对](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)）。 nchar 的 ISO 同义词是 national char 和 national character。
  
**nvarchar** [ ( n | **max** ) ]  
可变大小字符串数据。 n 用于定义字符串大小（以双字节为单位），并且它可能为 1 到 4,000 之间的值。 max 指示最大存储大小是 2^30-1 个字符 (2 GB)。 存储大小为 n 字节的两倍 + 2 个字节。 对于 [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF) 编码，存储大小为 n 个字节的两倍 + 2 个字节，并且可存储的字符数也为 n。 对于 UTF-16 编码，存储大小仍为 n 个字节的两倍 + 2 个字节，但可存储的字符数可能小于 n，因为补充字符使用两个双字节（也称为[代理项对](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)）。 nvarchar 的 ISO 同义词是 national char varying 和 national character varying。
  
## <a name="remarks"></a>注解  
一个常见误解是，认为在 [NCHAR(n) 和 NVARCHAR(n)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 中，n 定义字符数。 但在 [NCHAR(n) 和 NVARCHAR(n)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 中，n 定义字符串的长度（以双字节为单位）(0-4,000)。 n 不会定义可存储的字符数  。 这类似于 [CHAR(n) 和 VARCHAR(n)](../../t-sql/data-types/char-and-varchar-transact-sql.md) 的定义。   
出现此误解的原因是，使用在 Unicode 范围 0-65,535 中定义的字符时，每个双字节可以存储一个字符。 但是，在较高的 Unicode 范围 (65,536-1,114,111) 中，一个字符可能使用两个双字节。 例如，在定义为 NCHAR(10) 的列中，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 可以存储使用一个双字节（Unicode 范围 0-65,535）的 10 个字符，但在使用两个双字节（Unicode 范围 65,536-1,114,111）时，将不超过 10 个字符。 有关 Unicode 存储和字符范围的详细信息，请参阅 [UTF-8 与 UTF-16 之间的存储差异](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences)。     

如果没有在数据定义或变量声明语句中指定 n，则默认长度为 1。 如果没有使用 CAST 函数指定 n，则默认长度为 30。

若使用 nchar 或 nvarchar，则建议：
- 如果列数据项的大小一致，则使用 nchar。  
- 如果列数据项的大小差异相当大，则使用 nvarchar。  
- 如果列数据项大小相差很大，而且字符串长度可能超过 4,000 双字节，请使用 nvarchar(max)。  
  
sysname 是系统提供的用户定义数据类型，除了不可为 Null 外，在功能上与 nvarchar(128) 相同。 sysname 用于引用数据库对象名。
  
为使用 char 或 varchar 的对象分配的是默认的数据库排序规则，但可使用 COLLATE 子句分配特定的排序规则。
  
SET ANSI_PADDING 对于 nchar 和 nvarchar 始终为 ON。 SET ANSI_PADDING OFF 不适用于 nchar 或 nvarchar 数据类型。
  
在 Unicode 字符字符串常量的前面加上字母 N 作为前缀来表示 UCS-2 或 UTF-16 输入，具体取决于是否使用了 SC 排序规则。 如果没有 N 前缀，则字符串被转换为数据库的默认代码页，而该数据库可能无法识别某些字符。 从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，若使用启用了 UTF-8 的排序规则，则默认代码页能够存储 UNICODE UTF-8 字符集。 
 
> [!NOTE]  
> 在字符串常量前面添加字母 N 作为前缀时，如果要转换的常量不超过 nvarchar 字符串数据类型的最大长度 (4,000)，则隐式转换将生成 UCS-2 或 UTF-16 字符串。 否则，隐式转换将生成大值 nvarchar(max)。
  
> [!WARNING]  
> 每个非 null varchar(max) 或 nvarchar(max) 列都需要 24 个字节的附加固定分配，这将在执行排序操作期间根据 8,060 字节行限制进行计数 。 这些附加的字节可在表中为非 null varchar(max) 或 nvarchar(max) 列数创建隐式限制。 在以下情况下不提供特殊错误：创建表格（最大行大小超过允许的最大 8,060 字节时出现的一般警告除外）时，或插入数据时。 此较大行大小可能会导致用户在某些正常操作期间未意料到的错误（如错误 512）。  操作的两个示例是聚集索引密钥更新或完整列集排序。
  
## <a name="converting-character-data"></a>转换字符数据  
有关转换字符数据的信息，请参阅 [char 和 varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md)。
  
## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[COLLATE (Transact-SQL)](../statements/collations.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)  
[SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)    
[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)     
[单字节和多字节字符集](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
