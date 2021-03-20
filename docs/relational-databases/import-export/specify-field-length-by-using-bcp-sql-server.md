---
title: 使用 bcp 指定字段长度 (SQL Server) | Microsoft Docs
description: 在 SQL Server 中，如有必要，bcp 会提示输入字段长度、默认字段长度以及字段长度对包含 char 数据文件中数据存储的影响。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- default field lengths
- field length [SQL Server]
- data formats [SQL Server], field length
- bcp utility [SQL Server], field length
ms.assetid: 240f33ca-ef4a-413a-a4de-831885cb505b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a7b5f7e99bf8b50522af3c0429e664077629ddb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752327"
---
# <a name="specify-field-length-by-using-bcp-sql-server"></a>使用 bcp 指定字段长度 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  字段长度指示以字符格式表示数据时所要求的最大字符数。 如果数据以本机格式存储，则字段长度就是已知的，例如， **int** 数据类型占 4 个字节。 如果指示前缀长度为 0，则 **bcp** 命令会提示输入字段长度、默认字段长度以及字段长度对包含 **char** 数据的数据文件中数据存储的影响。  
  
## <a name="the-bcp-prompt-for-field-length"></a>bcp 提示输入字段长度  
 如果某个交互式 **bcp** 命令包含不带格式化文件开关 ( **-f**) 或数据格式开关（ **-n**、 **-c**、 **-w** 或 **-N**）的 **in** 或 **out** 选项，则该命令会提示输入每个数据字段的字段长度，如下所示：  
  
 `Enter length of field <field_name> [<default>]:`  
  
 有关在上下文中显示此提示符的示例，请参阅[在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)。  
  
> [!NOTE]  
>  在你以交互方式指定 **bcp** 命令中的所有字段后，该命令会提示你将自己对每个字段的响应保存到一个非 XML 格式化文件中。 有关非 XML 格式文件的详细信息，请参阅[ 非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)。  
  
 **bcp** 命令是否提示输入字段长度取决于若干因素，如下所示：  
  
-   当复制非固定长度的数据类型并指定前缀长度为 0 时， **bcp** 命令会提示输入字段长度。  
  
-   当将非字符数据转换为字符数据时， **bcp** 会建议一个足以存储该数据的默认字段长度。  
  
-   如果文件存储类型为非字符类型，则 **bcp** 命令不会提示输入字段长度。 数据将以 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机数据表示形式（本机格式）存储。  
  
## <a name="using-default-field-lengths"></a>使用默认字段长度  
 通常， [!INCLUDE[msCoName](../../includes/msconame-md.md)] 会建议你接受 **bcp** 建议的默认字段长度值。 如果已创建了字符模式数据文件，则使用默认字段长度可确保数据不会被截断，并且不会发生数字溢出错误。  
  
 如果指定的字段长度不正确，可能会发生问题。 例如，当复制数字数据并且为该数据指定的字段长度过短时， **bcp** 实用工具将显示一条溢出消息而不复制数据。 另外，当导出 **datetime** 数据并且为该字符串指定的字段长度少于 26 个字节时， **bcp** 实用工具将截断数据而不显示任何错误消息。  
  
> [!IMPORTANT]  
>  使用默认大小选项时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要读取整个字符串。 在某些情况下，使用默认字段长度可能导致“意外的文件结尾”错误。 通常情况下，当使用 **money** 和 **datetime** 数据类型而数据文件中只出现了所需字段的一部分时，就会发生此类错误；例如指定了 **mm** dd *yy*/*格式中的*/*datetime* 值而没有指定时间部分，因而该值少于以 **char** 格式表示 **datetime** 值所需的 24 个字符。 若要避免此类错误，请使用字段终止符或具有固定长度的数据字段，或者通过指定其他值来更改默认字段长度。  
  
### <a name="default-field-lengths-for-character-file-storage"></a>字符文件存储的默认字段长度  
 下表列出了要存储为字符文件存储类型的数据的默认字段长度。 可为空值的数据与非空数据的长度相同。  
  
|数据类型|默认长度（字符数）|  
|---------------|-----------------------------------|  
|**char**|定义的列长度|  
|**varchar**|定义的列长度|  
|**nchar**|定义的列长度的两倍|  
|**nvarchar**|定义的列长度的两倍|  
|**Text**|0|  
|**ntext**|0|  
|**bit**|1|  
|**binary**|定义的列长度的两倍 + 1|  
|**varbinary**|定义的列长度的两倍 + 1|  
|**图像**|0|  
|**datetime**|24|  
|**smalldatetime**|24|  
|**float**|30|  
|**real**|30|  
|**int**|12|  
|**bigint**|19|  
|**smallint**|7|  
|**tinyint**|5|  
|**money**|30|  
|**smallmoney**|30|  
|**decimal**|41*|  
|**numeric**|41*|  
|**uniqueidentifier**|37|  
|**timestamp**|17|  
|**varchar(max)**|0|  
|**varbinary(max)**|0|  
|**nvarchar(max)**|0|  
|UDT|用户定义的字词 (UDT) 列长度|  
|XML|0|  
  
 \*有关 **decimal** 和 **numeric** 数据类型的详细信息，请参阅 [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)。  
  
> [!NOTE]  
>  类型为 **tinyint** 的列的值介于 0 到 255 之间；表示该范围内的任意数值所需的最大字符数是三（用于表示 100 到 255 之间的值）。  
  
### <a name="default-field-lengths-for-native-file-storage"></a>本机文件存储的默认字段长度  
 下表列出了要存储为本机文件存储类型的数据的默认字段长度。 可为空值的数据与非空数据的长度相同，并且字符数据始终以字符格式存储。  
  
|数据类型|默认长度（字符数）|  
|---------------|-----------------------------------|  
|**bit**|1|  
|**binary**|定义的列长度|  
|**varbinary**|定义的列长度|  
|**图像**|0|  
|**datetime**|8|  
|**smalldatetime**|4|  
|**float**|8|  
|**real**|4|  
|**int**|4|  
|**bigint**|8|  
|**smallint**|2|  
|**tinyint**|1|  
|**money**|8|  
|**smallmoney**|4|  
|**decimal**|*|  
|**numeric**|*|  
|**uniqueidentifier**|16|  
|**timestamp**|8|  
  
 \*有关 **decimal** 和 **numeric** 数据类型的详细信息，请参阅 [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)。  
  
 在上述所有情况中，若要创建日后要重新加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据文件并使存储空间保持最小，请使用长度前缀，以及默认文件存储类型和默认字段长度。  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)   
 [使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)   
 [在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
  
