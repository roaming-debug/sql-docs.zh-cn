---
description: ntext、text 和 image (Transact-SQL)
title: ntext、text 和 image (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b7b067831ca56d3dc5797aa68e4810a7e9969946
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88459951"
---
# <a name="ntext-text-and-image-transact-sql"></a>ntext、text 和 image (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

用于存储大型非 Unicode 字符、Unicode 字符及二进制数据的固定长度数据类型和可变长度数据类型。 Unicode 数据使用 UNICODE UCS-2 字符集。
  
>**重要说明！**  SQL Server 的未来版本中将删除 ntext、text 和 image 数据类型。 请避免在新开发工作中使用这些数据类型，并考虑修改当前使用这些数据类型的应用程序。 请改用 [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)、 [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)和 [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) 。  
  
## <a name="arguments"></a>参数
**ntext**  
长度可变的 Unicode 数据，字符串最大长度为 2^30 - 1 (1,073,741,823) 个字节。 存储大小是所输入字符串长度的两倍（以字节为单位）。 ntext 的 ISO 同义词为 national text。
  
**text**  
服务器代码页中长度可变的非 Unicode 数据，字符串最大长度为 2^31-1 (2,147,483,647) 个字节。 当服务器代码页使用双字节字符时，存储仍是 2,147,483,647 字节。 根据字符串，存储大小可能小于 2,147,483,647 字节。
  
**图像**  
长度可变的二进制数据，从 0 到 2^31-1 (2,147,483,647) 个字节。
  
## <a name="remarks"></a>备注  
以下函数和语句可与 ntext、text 或 image 数据一起使用。
  
|函数|语句|  
|---|---|
|[DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT (Transact-SQL)](../../t-sql/queries/readtext-transact-sql.md)|  
|[PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE (Transact-SQL)](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SUBSTRING (Transact-SQL)](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR (Transact-SQL)](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID (Transact-SQL)](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)

