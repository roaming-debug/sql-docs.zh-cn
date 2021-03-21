---
description: 从程序变量执行大容量复制
title: 从程序变量大容量复制 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], program variables
- bcp_bind function
- mapping data types [ODBC]
- SQL Server Native Client ODBC driver, bulk copy
- data types [ODBC], bulk copying
- ODBC, bulk copy operations
- program variables [ODBC]
ms.assetid: e4284a1b-7534-4b34-8488-b8d05ed67b8c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92ba50ba85056dc9a55bc68f91a370015ec91f2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754767"
---
# <a name="bulk-copying-from-program-variables"></a>从程序变量执行大容量复制
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  可以直接从程序变量执行大容量复制。 分配用于保存行数据的变量并调用 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) 启动大容量复制后，为每一列调用 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) ，以指定与列关联的程序变量的位置和格式。 用数据填充每个变量，然后调用 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) 将一行数据发送到服务器。 重复填充变量并调用 **bcp_sendrow** 的过程，直到所有行都发送到服务器，然后调用 [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md) 以指定操作已完成。  
  
 **Bcp_bind**_pData_ 参数包含绑定到列的变量的地址。 每列的数据都可以通过以下两种方式之一存储：  
  
-   分配一个变量以保存数据。  
  
-   分配指示器变量，后面紧随数据变量。  
  
 指示器变量指示可变长度列的数据长度，如果列允许 NULL 值，还指示 NULL 值。 如果仅使用数据变量，则此变量的地址存储在 **bcp_bind**_pData_ 参数中。 如果使用指示器变量，则指示器变量的地址存储在 **bcp_bind**_pData_ 参数中。 大容量复制函数通过添加 **bcp_bind**_cbIndicator_ 和 *pData* 参数来计算数据变量的位置。  
  
 **bcp_bind** 支持三种方法来处理可变长度数据：  
  
-   仅将 *cbData* 与数据变量一起使用。 将数据的长度放置在 *cbData* 中。 每次要大容量复制的数据的长度更改时，调用 [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)重置 *cbData*。 如果使用其他两种方法之一，则为 *cbData* 指定 SQL_VARLEN_DATA。 如果为列提供的所有数据值都为 NULL，则为 *cbData* 指定 SQL_NULL_DATA。  
  
-   使用指示器变量。 随着每个新数据值移入数据变量，将该值的长度存储在指示器变量中。 如果使用另一种方法，请将 *cbIndicator* 指定为0。  
  
-   使用终止符指针。 加载 **bcp_bind**_pTerm_ 参数，其中包含终止数据的位模式的地址。 如果使用其他两种方法之一，则为 *pTerm* 指定 NULL。  
  
 这三种方法都可以在相同的 **bcp_bind** 调用上使用，在这种情况下，将使用导致复制的数据量最少的规范。  
  
 **Bcp_bind**_类型_ 参数使用 DB-Library 数据类型标识符，而非 ODBC 数据类型标识符。 DB-Library 在 sqlncli.msi 中定义的数据类型标识符与 ODBC **bcp_bind** 函数一起使用。  
  
 大容量复制函数并不支持所有 ODBC C 数据类型。 例如，大容量复制函数不支持 ODBC SQL_C_TYPE_TIMESTAMP 结构，因此请使用 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) 或 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 将 odbc SQL_TYPE_TIMESTAMP 数据转换为 SQL_C_CHAR 变量。 如果随后将 **bcp_bind** 与 SQLCHARACTER 的 *类型* 参数结合使用，将变量绑定到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** 列，则大容量复制函数会将字符变量中的 timestamp 转义子句转换为正确的日期时间格式。  
  
 下表列出了从 ODBC SQL 数据类型映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型时推荐使用的数据类型。  
  
|ODBC SQL 数据类型|ODBC C 数据类型|bcp_bind *类型* 参数|SQL Server 数据类型|  
|-----------------------|----------------------|--------------------------------|--------------------------|  
|SQL_CHAR|SQL_C_CHAR|SQLCHARACTER|**character**<br /><br /> **char**|  
|SQL_VARCHAR|SQL_C_CHAR|SQLCHARACTER|**varchar**<br /><br /> **字符变化**<br /><br /> **char varying**<br /><br /> **sysname**|  
|SQL_LONGVARCHAR|SQL_C_CHAR|SQLCHARACTER|**text**|  
|SQL_WCHAR|SQL_C_WCHAR|SQLNCHAR|**nchar**|  
|SQL_WVARCHAR|SQL_C_WCHAR|SQLNVARCHAR|**nvarchar**|  
|SQL_WLONGVARCHAR|SQL_C_WCHAR|SQLNTEXT|**ntext**|  
|SQL_DECIMAL|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **十进制**<br /><br /> **money**<br /><br /> **smallmoney**|  
|SQL_NUMERIC|SQL_C_NUMERIC|SQLNUMERICN|**numeric**|  
|SQL_BIT|SQL_C_BIT|SQLBIT|**bit**|  
|SQL_TINYINT（有符号）|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_TINYINT（无符号）|SQL_C_UTINYINT|SQLINT1|**tinyint**|  
|SQL_SMALL_INT（有符号）|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_SMALL_INT（无符号）|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER（有符号）|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER（无符号）|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **十进制**|  
|SQL_BIGINT（有符号和无符号）|SQL_C_CHAR|SQLCHARACTER|**bigint**|  
|SQL_REAL|SQL_C_FLOAT|SQLFLT4|**real**|  
|SQL_FLOAT|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_DOUBLE|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_BINARY|SQL_C_BINARY|SQLBINARY|**binary**<br /><br /> **timestamp**|  
|SQL_VARBINARY|SQL_C_BINARY|SQLBINARY|**varbinary**<br /><br /> **二进制改变**|  
|SQL_LONGVARBINARY|SQL_C_BINARY|SQLBINARY|**图像**|  
|SQL_TYPE_DATE|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIME|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIMESTAMP|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_GUID|SQL_C_GUID|SQLUNIQUEID|**uniqueidentifier**|  
|SQL_INTERVAL_|SQL_C_CHAR|SQLCHARACTER|**char**|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不具有有符号 **tinyint**、无符号 **smallint** 或无符号 **整数** 数据类型。 若要在迁移这些数据类型时防止丢失数据值，应使用第二大整数数据类型创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 若要防止用户以后添加原始数据类型所允许范围之外的值，应对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列应用一条规则，将允许的值限定在原始源中的数据类型所支持的范围之内：  
  
```  
CREATE TABLE Sample_Ints(STinyIntCol   SMALLINT,  
USmallIntCol INT)  
GO  
CREATE RULE STinyInt_Rule  
AS   
@range >= -128 AND @range <= 127  
GO  
CREATE RULE USmallInt_Rule  
AS   
@range >= 0 AND @range <= 65535  
GO  
sp_bindrule STinyInt_Rule, 'Sample_Ints.STinyIntCol'  
GO  
sp_bindrule USmallInt_Rule, 'Sample_Ints.USmallIntCol'  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不直接支持 interval 数据类型。 不过，应用程序可以将 interval 转义序列作为字符串存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 字符列中。 该应用程序可以读取它们供以后使用，但是它们无法用在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中。  
  
 可以使用大容量复制函数将从 ODBC 数据源中读取的数据快速加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中。 使用 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) 将结果集的列绑定到程序变量，然后使用 **bcp_bind** 将相同的程序变量绑定到大容量复制操作。 然后，调用 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) 或 **SQLFetch** 将数据从 ODBC 数据源提取到程序变量中，并调用 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) 将数据从程序变量大容量复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 应用程序可以在任何需要更改最初在 **bcp_bind** _pData_ 参数中指定的数据变量的地址时使用 [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md)函数。 应用程序可以在任何需要更改最初在 **bcp_bind**_cbData_ 参数中指定的数据长度时使用 [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)函数。  
  
 无法使用大容量复制功能将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取到程序变量中；没有与 "bcp_readrow" 函数类似的功能。 只能将数据从应用程序发送到服务器。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
