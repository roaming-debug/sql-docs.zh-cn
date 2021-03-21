---
title: 利用 bcp 指定数据文件中的前缀长度
description: 本文介绍前缀字段，该字段对字段的长度进行编码以提供紧凑的文件存储空间，以便使用本机格式批量导出到数据文件。
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], prefix length
- prefix length [SQL Server]
- lengths [SQL Server], prefix characters
- data formats [SQL Server], prefix length
ms.assetid: ce32dd1a-26f1-4f61-b9fa-3f1feea9992e
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: f57d14262410c3b469cc4576e35f2eb0d6c625f6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745727"
---
# <a name="specify-prefix-length-in-data-files-using-bcp-sql-server"></a>使用 bcp 指定数据文件中的前缀长度 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  当将本机格式的数据批量导出到数据文件中时，为了使文件存储空间最为紧凑， **bcp** 命令会在每个字段前面使用一个或多个字符来指明字段长度。 这些字符称为“长度前缀字符”  。  
  
## <a name="the-bcp-prompt-for-prefix-length"></a>bcp 的前缀长度提示  
 如果交互式 **bcp** 命令包含不带格式化文件开关 ( **-f** ) 或数据格式开关（ **-n** 、 **-c**、 **-w** 或 **-N**）的 **in** 或 **out** 选项，则该命令会提示输入每个数据字段的前缀长度，如下所示：  
  
 `Enter prefix length of field <field_name> [<default>]:`  
  
 如果你指定 0，则 **bcp** 会提示输入字段长度（对于字符数据类型）或字段终止符（对于本机非字符数据类型）。  
  
> [!NOTE]  
>  在你以交互方式指定 **bcp** 命令中的所有字段后，该命令会提示你将自己对每个字段的响应保存到一个非 XML 格式化文件中。 有关非 XML 格式文件的详细信息，请参阅[非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)。  
  
## <a name="overview-of-prefix-length"></a>前缀长度概述  
 若要存储字段的前缀长度，您需要有足够的字节来表示字段的最大长度。 另外，所需的字节数还取决于文件存储类型、列是否可以为 Null 以及数据是以本机格式还是字符格式存储在数据文件中。 例如， **text** 或 **image** 数据类型需要四个前缀字符存储字段长度，而 **varchar** 数据类型需要两个字符。 在数据文件中，这些长度前缀字符以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的内部二进制数据格式存储。  
  
> [!IMPORTANT]  
>  使用本机格式时，请使用长度前缀而不要使用字段终止符。 本机格式数据可能会与终止符相冲突，因为本机格式数据文件是以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内部二进制数据格式存储的。  
  
##  <a name="prefix-lengths-for-bulk-export"></a><a name="PrefixLengthsExport"></a> 大容量导出时的前缀长度  
  
> [!NOTE]  
>  导出字段时前缀长度提示中提供的默认值指明的是该字段的最有效前缀长度。  
  
 以空字段表示 Null 值。 若要指示字段为空（表示 NULL），字段前缀应包含值 -1，也就是说，字段前缀至少需要 1 个字节。 请注意，如果某个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表列允许 Null 值，则根据文件存储类型的不同，该列需要的前缀长度为 1 或更大。  
  
 在大容量导出数据并以本机数据类型或字符格式存储数据时，请使用下表中显示的前缀长度。  
  
|SQL Server<br /><br /> 数据类型|本机格式<br /><br /> NOT NULL|本机格式<br /><br /> Null|字符格式<br /><br /> NOT NULL|字符格式<br /><br /> Null|  
|------------------------------|--------------------------------|----------------------------|-----------------------------------|-------------------------------|  
|**char**|2|2|2|2|  
|**varchar**|2|2|2|2|  
|**nchar**|2|2|2|2|  
|**nvarchar**|2|2|2|2|  
|**text***|4|4|4|4|  
|**ntext***|4|4|4|4|  
|**binary**|2|2|2|2|  
|**varbinary**|2|2|2|2|  
|**image***|4|4|4|4|  
|**datetime**|0|1|0|1|  
|**smalldatetime**|0|1|0|1|  
|**decimal**|1|1|1|1|  
|**numeric**|1|1|1|1|  
|**float**|0|1|0|1|  
|**real**|0|1|0|1|  
|**int**|0|1|0|1|  
|**bigint**|0|1|0|1|  
|**smallint**|0|1|0|1|  
|**tinyint**|0|1|0|1|  
|**money**|0|1|0|1|  
|**smallmoney**|0|1|0|1|  
|**bit**|0|1|0|1|  
|**uniqueidentifier**|1|1|0|1|  
|**timestamp**|1|1|1|1|  
|**varchar(max)**|8|8|8|8|  
|**varbinary(max)**|8|8|8|8|  
|**UDT** （用户定义的数据类型）|8|8|8|8|  
|**XML**|8|8|8|8|  
|**sql_variant**|8|8|8|8|  
  
 \***的未来版本中将删除** ntext **、** text **和** image [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据类型。 请避免在新开发工作中使用这些数据类型，并考虑修改当前使用这些数据类型的应用程序。 请改用 **nvarchar(max)** 、 **varchar(max)** 和 **varbinary(max)** 。  
  
##  <a name="prefix-lengths-for-bulk-import"></a><a name="PrefixLengthsImport"></a> 大容量导入时的前缀长度  
 大容量导入数据时，前缀长度为最初创建数据文件时指定的值。 如果数据文件不是由 **bcp** 命令创建，那么可能没有长度前缀字符。 在这种情况下，将前缀长度指定为 0。  
  
> [!NOTE]  
>  若要在不是使用 **bcp** 创建的数据文件中指定前缀长度，请使用本主题前面的 [批量导出时的前缀长度](#PrefixLengthsExport)中介绍的长度。  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [使用 bcp 指定字段长度 (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  
