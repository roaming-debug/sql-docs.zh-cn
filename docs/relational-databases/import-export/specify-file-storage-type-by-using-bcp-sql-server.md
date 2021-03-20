---
title: 使用 bcp 指定文件存储类型
description: 使用 bcp 将数据作为其数据库表类型、以字符表示形式或作为支持隐式转换的数据类型导出到文件中。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], file storage types
- importing data, file storage types
- native data format [SQL Server]
- file storage types [SQL Server]
- data formats [SQL Server], file storage types
ms.assetid: 85e12df8-1be7-4bdc-aea9-05aade085c06
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: ffcde703125ecc798177461343b98da6077c534f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752267"
---
# <a name="specify-file-storage-type-using-bcp-sql-server"></a>使用 bcp 指定文件存储类型 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  “文件存储类型”  说明数据在数据文件中的存储方式。 数据可以按其数据库表类型（本机格式）、字符表示形式（字符格式）或支持隐式转换的任何数据类型导出到数据文件中；例如，以 **int** 形式复制 **smallint**。用户定义的数据类型将按其基类型导出。  
  
## <a name="the-bcp-prompt-for-file-storage-type"></a>用于文件存储类型的 bcp 提示符  
 如果某个交互式 **bcp** 命令包含不带格式化文件开关 ( **-f** ) 或数据格式开关（ **-n** 、 **-c**、 **-w** 或 **-N**）的 **in** 或 **out** 选项，则该命令会提示输入每个数据字段的文件存储类型，如下所示：  
  
 `Enter the file storage type of field <field_name> [<default>]:`  
  
 您对此提示符的响应取决于要执行的任务，如下所示：  
  
-   若要以尽可能大的压缩存储（本机数据格式）将数据从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例批量导出到数据文件中，请接受 bcp 提供的默认文件存储类型  。 有关本机文件存储类型的列表，请参阅本主题后面所述的“本机文件存储类型”。  
  
-   若要以字符格式将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例大容量导出到数据文件中，请指定 **char** 作为表中所有列的文件存储类型。  
  
-   若要将数据从数据文件大容量导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，对于以字符格式存储的类型，请将文件存储类型指定为 **char** ，而对于以本机数据类型格式存储的数据，请按需指定以下文件存储类型之一：  
  
    |文件存储类型|在命令提示符下输入|  
    |-----------------------|-----------------------------|  
    |**char**|**c**[**har**]|  
    |**varchar**|**c[har]**|  
    |**nchar**|**w**|  
    |**nvarchar**|**w**|  
    |**text**|**T**[**ext**]|  
    |**ntext2**|**W**|  
    |**binary**|**x**|  
    |**varbinary**|**x**|  
    |**图像**|**I**[**mage**]|  
    |**datetime**|**d[ate]**|  
    |**smalldatetime**|**D**|  
    |**time**|**te**|  
    |**date**|**de**|  
    |**datetime2**|**d2**|  
    |**datetimeoffset**|**do**|  
    |**decimal**|**n**|  
    |**numeric**|**n**|  
    |**float**|**f[loat]**|  
    |**real**|**r**|  
    |**Int**|**i[nt]**|  
    |**bigint**|**B[igint]**|  
    |**smallint**|**s[mallint]**|  
    |**tinyint**|**t[inyint]**|  
    |**money**|**m[oney]**|  
    |**smallmoney**|**M**|  
    |**bit**|**b[it]**|  
    |**uniqueidentifier**|**u**|  
    |**sql_variant**|**V[ariant]**|  
    |**timestamp**|**x**|  
    |**UDT** （用户定义的数据类型）|**U**|  
    |**XML**|**X**|  
  
     \*字段长度、前缀长度和终止符一起决定了在以 **char** 文件存储类型导出的非字符数据的数据文件中分配的存储空间量。  
  
     \*\***的未来版本中将删除** ntext **、** text **和** image [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据类型。 在新的开发工作中，请避免使用这些数据类型，并修改当前使用它们的应用程序。 请改用 **nvarchar(max)** 、 **varchar(max)** 和 **varbinary(max)** 。  
  
## <a name="native-file-storage-types"></a>本机文件存储类型  
 在格式化文件中，每种本机文件存储类型都记录为相应的宿主文件数据类型。  
  
|文件存储类型|宿主文件数据类型|  
|-----------------------|-------------------------|  
|**char**|SQLCHAR|  
|**varchar**|SQLCHAR|  
|**nchar**|SQLNCHAR|  
|**nvarchar**|SQLNCHAR|  
|**text**|SQLCHAR|  
|**ntext**|SQLNCHAR|  
|**binary**|SQLBINARY|  
|**varbinary**|SQLBINARY|  
|**图像**|SQLBINARY|  
|**datetime**|SQLDATETIME|  
|**smalldatetime**|SQLDATETIM4|  
|**decimal**|SQLDECIMAL|  
|**numeric**|SQLNUMERIC|  
|**float**|SQLFLT8|  
|**real**|SQLFLT4|  
|**int**|SQLINT|  
|**bigint**|SQLBIGINT|  
|**smallint**|SQLSMALLINT|  
|**tinyint**|SQLTINYINT|  
|**money**|SQLMONEY|  
|**smallmoney**|SQLMONEY4|  
|**bit**|SQLBIT|  
|**uniqueidentifier**|SQLUNIQUEID|  
|**sql_variant**|SQLVARIANT|  
|**timestamp**|SQLBINARY|  
|UDT（用户定义的数据类型）|SQLUDT|  
  
 \*以字符格式存储的数据文件使用 **char** 作为文件存储类型。 因此，对于字符数据文件，SQLCHAR 是唯一出现在格式化文件中的数据类型。  
  
 \*\*你无法将数据批量导入到具有 DEFAULT 值的 **text**、 **ntext** 和 **image** 列。  
  
## <a name="additional-considerations-for-file-storage-types"></a>文件存储类型的其他注意事项  
 当您将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例大容量导出到数据文件时：  
  
-   您始终可以指定 **char** 作为文件存储类型。  
  
-   如果您输入的文件存储类型表示无效的隐式转换， **bcp** 将失败；例如，尽管您可以为 **int** 数据指定 **smallint** 类型，但是如果您为 **smallint** 数据指定 **int** 类型，将导致溢出错误。  
  
-   当非字符数据类型（如 **float**、 **money**、 **datetime** 或 **int** ）存储为其数据库类型时，数据将写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机格式的数据文件。  
  
    > [!NOTE]  
    >  在你以交互方式指定 **bcp** 命令中的所有字段后，该命令会提示你将自己对每个字段的响应保存到一个非 XML 格式化文件中。 有关非 XML 格式文件的详细信息，请参阅[ 非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [使用 bcp 指定字段长度 (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
  
