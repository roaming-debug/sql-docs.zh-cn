---
title: COPY INTO (Transact-SQL)
titleSuffix: (Azure Synapse Analytics) - SQL Server
description: 在 Azure Synapse Analytics 中使用 COPY 语句从外部存储帐户加载数据。
ms.date: 09/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COPY_TSQL
- COPY INTO
- COPY
- LOAD
dev_langs:
- TSQL
author: kevinvngo
ms.author: kevin
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 43a5cc755eb07fe80a0d33d6b5b892e2a65ab21e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638723"
---
# <a name="copy-transact-sql"></a>COPY (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

本文介绍如何在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中使用 COPY 语句从外部存储帐户加载数据。 COPY 语句为 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的高吞吐量数据引入提供了最大的灵活性。 使用 COPY 可以实现以下功能：

- 权限较低的用户在加载时，不需要对数据仓库有严格的控制权限
- 执行单个 T-SQL 语句，不需要创建任何其他数据库对象
- 正确分析和加载 CSV 文件，其中分隔符（字符串、字段、行）在字符串分隔列中进行转义
- 指定更精细的权限模型，无需使用共享访问签名 (SAS) 来公开存储帐户密钥
- 为 ERRORFILE 位置 (REJECTED_ROW_LOCATION) 使用一个不同的存储帐户
- 为每个目标列自定义默认值，并指定要加载到特定目标列中的源数据字段
- 为 CSV 文件指定自定义行终止符
- 对 CSV 文件使用 SQL Server 日期格式
- 在存储位置路径中指定通配符和多个文件

请访问以下文档，了解使用 COPY 语句的综合示例和快速入门：

- [快速入门：使用 COPY 语句批量加载数据](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [快速入门：关于使用 COPY 语句及其支持的身份验证方法的示例](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [快速入门：使用丰富的 Synapse Studio UI 创建 COPY 语句](/azure/synapse-analytics/quickstart-load-studio-sql-pool)

## <a name="syntax"></a>语法  

```syntaxsql
COPY INTO [schema.]table_name
[(Column_list)] 
FROM '<external_location>' [,...n]
WITH  
 ( 
 [FILE_TYPE = {'CSV' | 'PARQUET' | 'ORC'} ]
 [,FILE_FORMAT = EXTERNAL FILE FORMAT OBJECT ]  
 [,CREDENTIAL = (AZURE CREDENTIAL) ]
 [,ERRORFILE = '[http(s)://storageaccount/container]/errorfile_directory[/]]' 
 [,ERRORFILE_CREDENTIAL = (AZURE CREDENTIAL) ]
 [,MAXERRORS = max_errors ] 
 [,COMPRESSION = { 'Gzip' | 'DefaultCodec'| 'Snappy'}] 
 [,FIELDQUOTE = 'string_delimiter'] 
 [,FIELDTERMINATOR =  'field_terminator']  
 [,ROWTERMINATOR = 'row_terminator']
 [,FIRSTROW = first_row]
 [,DATEFORMAT = 'date_format'] 
 [,ENCODING = {'UTF8'|'UTF16'}] 
 [,IDENTITY_INSERT = {'ON' | 'OFF'}]
)
```

## <a name="arguments"></a>参数  

*schema_name*  
如果执行操作的用户的默认架构是指定表的架构，则该参数是可选的。 如果未指定 *schema*，并且执行复制操作的用户的默认架构不同于指定表的架构，则将取消复制，并返回一条错误消息。  

*table_name*  
要将数据复制到其中的表的名称。 目标表可以是临时或永久表，并且必须已存在于数据库中。 

*(column_list)*  
包含一列或多列的可选列表，用于将源数据字段映射到目标表列以加载数据。 必须用括号将 column_list 括起来，并且用逗号进行分隔。 列列表的格式如下：

[(Column_name [Default_value] [Field_number] [,...n])]

- *Column_name*：目标表中列的名称。
- *Default_value*：将替换输入文件中的任何 NULL 值的默认值。 默认值适用于所有文件格式。 如果省略列列表中的某一列或者某个输入文件字段为空，则 COPY 将尝试从输入文件中加载 NULL。
- *Field_number*：将映射到目标列名称的输入文件字段编号。
- 字段索引从 1 开始。

如果未指定列列表，则 COPY 将根据源顺序和目标顺序映射列：输入字段 1 将映射到目标列 1，字段 2 将映射到列 2，依此类推。

*External locations(s)*</br>
包含数据的文件的暂存位置。 目前支持 Azure Data Lake Storage (ADLS) Gen2 和 Azure Blob 存储：

- Blob 存储的 *外部位置*： https://<account>.blob.core.windows.net/<container>/<path>
- ADLS Gen2 的 *外部位置*： https://<account>。 dfs.core.windows.net/<container>/<path>

> [!NOTE]  
> .blob 终结点也可用于 ADLS Gen2，并且当前可获得最佳性能。 当身份验证方法不需要 .dfs 时，请使用 .blob 终结点。

- *Account*：存储帐户名称

- *Container*：blob 容器名称

- *Path*：数据的文件夹或文件路径。 位置从容器开始。 如果指定了文件夹，则 COPY 将从该文件夹及其所有子文件夹中检索所有文件。 除非在路径中显式指定，否则 COPY 会忽略隐藏文件夹，并且不返回以下划线 (_) 或句点 (.) 开头的文件。 即使使用通配符指定路径也是如此。

可以在路径中使用通配符：

- 通配符路径名称匹配区分大小写
- 可以使用反斜杠字符 (\\) 对通配符进行转义
- 通配符扩展以递归方式应用。 例如，在以下示例中，将加载 Customer1（包括 Customer1 的子目录）下的所有 CSV 文件：‘Account/Container/Customer1/*.csv’

> [!NOTE]  
> 为了获得最佳性能，请避免指定通配符，因为通配符会扩展成更多文件。 如有可能，请列出多个文件位置，而不是指定通配符。

只能通过逗号分隔列表从同一存储帐户和容器中指定多个文件位置，例如：

- ‘ https://<account>.blob.core.windows.net/<container>/<path>’, ‘ https://<account>.blob.core.windows.net/<container>/<path>’…

*FILE_TYPE = { ‘CSV’ | ‘PARQUET’ | ‘ORC’ }*</br>
*FILE_TYPE* 指定外部数据的格式。

- CSV：指定符合 [RFC 4180](https://tools.ietf.org/html/rfc4180) 标准的逗号分隔值文件。
- PARQUET：指定 Parquet 格式。
- ORC：指定优化行纵栏表 (ORC) 格式。

>[!NOTE]  
>Polybase 中的文件类型“分隔文本”被替换为“CSV”文件格式，后者可以通过 FIELDTERMINATOR 参数配置默认逗号分隔符。 

*FILE_FORMAT = external_file_format_name*</br>
*FILE_FORMAT* 仅适用于 Parquet 和 ORC 文件，用于指定为外部数据存储文件类型和压缩方法的外部文件格式对象的名称。 若要创建外部文件格式，请使用 [CREATE EXTERNAL FILE FORMAT](create-external-file-format-transact-sql.md)。

*CREDENTIAL (IDENTITY = ‘’, SECRET = ‘’)*</br>
*CREDENTIAL* 指定访问外部存储帐户的身份验证机制。 身份验证方法包括：

|                          |                CSV                |                      Parquet                       |                        ORC                         |
| :----------------------: | :-------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
|  **Azure blob 存储**  | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD |                      SAS/KEY                       |                      SAS/KEY                       |
| **Azure Data Lake Gen2** | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD |

1：此身份验证方法需要在外部位置路径使用 .blob 终结点 (.blob.core.windows.net)。

2：此身份验证方法需要在外部位置路径使用 .dfs 终结点 (.dfs.core.windows.net)。


> [!NOTE]  
>
> - 使用 AAD 或公共存储帐户进行身份验证时，无需指定 CREDENTIAL。 
> - 如果存储帐户与 VNet 相关联，则必须使用 MSI（托管标识）进行身份验证。

- 使用共享访问签名 (SAS) 进行身份验证
  
  - *IDENTITY：一个值为“共享访问签名”的常量*
  - *SECRET：[共享访问签名](/azure/storage/common/storage-sas-overview)对存储帐户中的资源提供委托访问*  。
  -  所需的最低权限：READ 和 LIST
  
- 使用 [*服务主体*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)进行身份验证

  - *IDENTITY：<ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET：AAD 应用程序服务主体密钥*
  -  所需的最小 RBAC 角色：存储 blob 数据参与者、存储 blob 数据所有者或存储 blob 数据读取者

- 使用存储帐户密钥进行身份验证
  
  - *IDENTITY：一个值为“存储帐户密钥”的常量*
  - *SECRET：存储帐户密钥*
  
- 使用[托管标识](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional)（VNet 服务终结点）进行身份验证
  
  - *IDENTITY：一个值为“托管标识”的常量*
  - 所需的最小 RBAC 角色：已注册 AAD 的 SQL Database 服务器的存储 blob 数据参与者或存储 blob 数据所有者
  
- 使用 AAD 用户进行身份验证
  
  - *不需要 CREDENTIAL*
  - 所需的最小 RBAC 角色：AAD 用户的存储 blob 数据参与者或存储 blob 数据所有者

*ERRORFILE = Directory Location*</br>
*ERRORFILE* 仅适用于 CSV。 指定 COPY 语句中的目录，应在该目录中写入被拒绝的行和相应的错误文件。 可以指定存储帐户的完整路径，也可以指定容器的相对路径。 如果指定的路径不存在，系统将代你创建一个。 创建名称为“_rejectedrows”的子目录。除非在位置参数中明确命名，否则，“_ ”字符将确保对该目录转义以进行其他数据处理。 

在此目录中，存在根据负荷提交时间创建的文件夹，采用 YearMonthDay-HourMinuteSecond 格式（例如， 20180330-173205）。 在此文件夹中，将写入两种类型的文件，即原因（错误）文件和数据（行）文件，每个文件都预先追加 queryID、distributionID 和文件 GUID。 数据和原因位于不同的文件中，因此相应的文件具有匹配的前缀。

如果 ERRORFILE 定义了存储帐户的完整路径，则会使用 ERRORFILE_CREDENTIAL 连接到该存储。 否则，将使用为 CREDENTIAL 指定的值。

*ERRORFILE_CREDENTIAL = (IDENTITY= ‘’, SECRET = ‘’)*</br>
*ERRORFILE_CREDENTIAL* 仅适用于 CSV 文件。 支持的数据源和身份验证方法包括：

- Azure Blob 存储 - SAS/SERVICE PRINCIPAL/KEY/AAD
- Azure Data Lake Gen2 - SAS/MSI/SERVICE PRINCIPAL/KEY/AAD
  
- 使用共享访问签名 (SAS) 进行身份验证
  - *IDENTITY：一个值为“共享访问签名”的常量*
  - *SECRET：[共享访问签名](/azure/storage/common/storage-sas-overview)对存储帐户中的资源提供委托访问*  。
  - 所需的最低权限：READ、LIST、WRITE、CREATE、DELETE
  
- 使用 [*服务主体*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)进行身份验证
  - *IDENTITY：<ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET：AAD 应用程序服务主体密钥*
  - 所需的最小 RBAC 角色：存储 blob 数据参与者或存储 blob 数据所有者
  
> [!NOTE]  
> 使用 OAuth 2.0 令牌终结点 **V1**

- 使用存储帐户密钥进行身份验证
  - *IDENTITY：一个值为“存储帐户密钥”的常量*
  - *SECRET：存储帐户密钥*
  
- 使用[托管标识](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional)（VNet 服务终结点）进行身份验证
  - *IDENTITY：一个值为“托管标识”的常量*
  - 所需的最小 RBAC 角色：已注册 AAD 的 SQL Database 服务器的存储 blob 数据参与者或存储 blob 数据所有者
  
- 使用 AAD 用户进行身份验证
  - *不需要 CREDENTIAL*
  - 所需的最小 RBAC 角色：AAD 用户的存储 blob 数据参与者或存储 blob 数据所有者

> [!NOTE]  
> 如果为 ERRORFILE 使用相同的存储帐户，并指定相对于容器根目录的 ERRORFILE 路径，则无需指定 ERROR_CREDENTIAL。

*MAXERRORS = max_errors*</br>
*MAXERRORS* 指定允许加载的最大拒绝行数，超过该数量后将取消 COPY 操作。 COPY 操作无法导入的每一行都将被忽略并且计为一个错误。 如果未指定 max_errors，则默认值为 0。

COMPRESSION = { 'DefaultCodec '\| ’Snappy’ \| ‘GZIP’ \| ‘NONE’}</br>
*COMPRESSION* 是一个可选参数，用于指定外部数据的数据压缩方法。

- CSV 支持 GZIP
- Parquet 支持 GZIP 和 Snappy
- ORC 支持 DefaultCodec 和 Snappy。
- Zlib 是 ORC 的默认压缩方法

如果未指定此参数，COPY 命令将根据文件扩展名自动检测压缩类型：

- .gz - **GZIP**
- .snappy - **Snappy**
- .deflate - **DefaultCodec**（仅限 Parquet 和 ORC）

 *FIELDQUOTE = 'field_quote'*</br>
*FIELDQUOTE* 适用于 CSV，它指定一个字符，该字符将用作 CSV 文件中的引号字符（字符串分隔符）。 如果未指定，根据 RFC 4180 标准中的定义，引号字符 (") 将用作引号字符。 FIELDQUOTE 的 UTF-8 不支持扩展的 ASCII 和多字节字符。

> [!NOTE]  
> FIELDQUOTE 字符会在有双 FIELDQUOTE（分隔符）的字符串列中进行转义。 

*FIELDTERMINATOR = 'field_terminator’*</br>
*FIELDTERMINATOR* 仅适用于 CSV。 指定将在 CSV 文件中使用的字段终止符。 可使用十六进制表示法指定字段终止符。 字段终止符可以是多字符。 默认的字段终止符为 (,)。 FIELDTERMINATOR 的 UTF-8 不支持扩展的 ASCII 和多字节字符。

ROW TERMINATOR = 'row_terminator'</br>
*ROW TERMINATOR* 仅适用于 CSV。 指定将在 CSV 文件中使用的行终止符。 可使用十六进制表示法指定行终止符。 行终止符可以是多字符。 默认情况下，行终止符为 \r\n。 

当指定 \n（换行符）以生成 \r\n 时，COPY 命令会为 \r 字符加上前缀。 要仅指定 \n 字符，请使用十六进制表示法 (0x0A)。 以十六进制指定多字符行终止符时，请勿在每个字符之间指定 0x。

ROW TERMINATOR 的 UTF-8 不支持扩展的 ASCII 和多字节字符。

*FIRSTROW  = First_row_int*</br>
*FIRSTROW* 适用于 CSV，它为 COPY 命令指定在所有文件中最先读取的行号。 值从 1 开始，1 是默认值。 如果值设置为二，则在加载数据时，会跳过每个文件中的第一行（标头行）。 如果有行终止符，则跳过该行。

DATEFORMAT = { ‘mdy’ \| ‘dmy’ \| ‘ymd’ \| ‘ydm’ \| ‘myd’ \| ‘dym’ }</br>
DATEFORMAT 仅适用于 CSV，它指定映射到 SQL Server 日期格式的日期格式。 有关所有 Transact-SQL 日期和时间数据类型及函数的概述，请参阅[日期和时间数据类型及函数 (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md)。 COPY 命令中的 DATEFORMAT 优先于[在会话级别配置的 DATEFORMAT](set-dateformat-transact-sql.md)。

*ENCODING = ‘UTF8’ | ‘UTF16’*</br>
*ENCODING* 仅适用于 CSV。 默认值为 UTF8。 指定 COPY 命令加载的文件的数据编码标准。 

*IDENTITY_INSERT = ‘ON’ | ‘OFF’*</br>
IDENTITY_INSERT 指定是否将导入数据文件中的标识值用于标识列。 如果 IDENTITY_INSERT 为 OFF（默认值），则验证此列的标识值，但不导入。 Azure Synapse Analytics 会根据创建表时指定的种子和增量值自动分配唯一值。 请注意 COPY 命令的以下行为：

- 如果 IDENTITY_INSERT 为 OFF，并且表有一个标识列
  - 必须指定一个不会将输入字段映射到标识列的列列表。
- 如果 IDENTITY_INSERT 为 ON，并且表有一个标识列
  - 如果传递了列列表，则它必须将输入字段映射到标识列。
- 列列表中的 IDENTITY COLUMN 不支持默认值。
- 一次只能为一个表设置 IDENTITY_INSERT。

### <a name="permissions"></a>权限  

执行 Copy 命令的用户必须具有以下权限： 

- [ADMINISTER DATABASE BULK OPERATIONS](grant-database-permissions-transact-sql.md#remarks)
- [INSERT ](grant-database-permissions-transact-sql.md#remarks)

需要 INSERT 和 ADMINISTER BULK OPERATIONS 权限。 在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中，需要 INSERT 和 ADMINISTER DATABASE BULK OPERATIONS 权限。

## <a name="examples"></a>示例  

### <a name="a-load-from-a-public-storage-account"></a>A. 从公共存储帐户加载数据

下面的示例将从公共存储帐户加载数据，这是 COPY 命令最简单的一种形式。 在此示例中，COPY 语句的默认值与行项 csv 文件的格式匹配。

```sql
COPY INTO dbo.[lineitem] FROM 'https://unsecureaccount.blob.core.windows.net/customerdatasets/folder1/lineitem.csv'
```

COPY 命令的默认值为：

- DATEFORMAT = Session DATEFORMAT 

- MAXERRORS = 0

- COMPRESSION 默认值为未压缩

- FIELDQUOTE = “” 

- FIELDTERMINATOR = “,” 

- ROWTERMINATOR = ‘\n'

> [!IMPORTANT]
> COPY 在内部将 ‘\n’ 视为 ‘\r\n’。 有关详细信息，请参阅 ROWTERMINATOR 部分。

- FIRSTROW = 1

- ENCODING = ‘UTF8’

- FILE_TYPE = ‘CSV’

- IDENTITY_INSERT = ‘OFF’

### <a name="b-load-authenticating-via-share-access-signature-sas"></a>B. 加载时通过共享访问签名 (SAS) 进行身份验证

下面的示例将加载使用换行符作为行终止符的文件（例如 UNIX 输出）。 此示例还使用 SAS 密钥向 Azure blob 存储进行身份验证。

```sql
COPY INTO test_1
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSHFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=';',
    ROWTERMINATOR='0X0A',
    ENCODING = 'UTF8',
    DATEFORMAT = 'ymd',
    MAXERRORS = 10,
    ERRORFILE = '/errorsfolder',--path starting from the storage container
    IDENTITY_INSERT = 'ON'
)
```

### <a name="c-load-with-a-column-list-with-default-values-authenticating-via-storage-account-key"></a>C. 加载具有默认值的列列表时通过存储帐户密钥进行身份验证

此示例将加载指定了具有默认值的列列表的文件。 

```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO test_1 (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0PQW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0A',
    ENCODING = 'UTF8',
    FIRSTROW = 2
)
```

### <a name="d-load-parquet-or-orc-using-existing-file-format-object"></a>D. 使用现有文件格式对象加载 Parquet 或 ORC

 此示例将使用通配符加载某个文件夹下的所有 parquet 文件。 

```sql
COPY INTO test_parquet
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.parquet'
WITH (
    FILE_FORMAT = myFileFormat,
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
)
```

### <a name="e-load-specifying-wild-cards-and-multiple-files"></a>E. 加载时指定通配符和多个文件

```sql
COPY INTO t1
FROM 
'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt', 
    'https://myaccount.blob.core.windows.net/myblobcontainer/folder1'
WITH ( 
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= '<client_id>@<OAuth_2.0_Token_EndPoint>',SECRET='<key>'),
    FIELDTERMINATOR = '|'
)
```

### <a name="f-load-using-msi-credentials"></a>F. 使用 MSI 凭据加载

```sql
COPY INTO dbo.myCOPYDemoTable
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL = (IDENTITY = 'Managed Identity'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=','
)
```

## <a name="faq"></a>常见问题解答

### <a name="what-is-the-performance-of-the-copy-command-compared-to-polybase"></a>与 PolyBase 相比，COPY 命令的性能如何？
COPY 命令将具有更好的性能，具体取决于工作负载。 为了获得最佳加载性能，请考虑在加载 CSV 时将你的输入拆分为多个文件。

### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-csv-files"></a>加载 CSV 文件时，COPY 命令的文件拆分指导是什么？
下表概述了文件数量指导。 一旦达到推荐的文件数量，便能获得更大的文件，性能也就越高。 对于简单的文件拆分体验，请参阅以下[文档](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/how-to-maximize-copy-load-throughput-with-file-splits/ba-p/1314474)。 

| **DWU** | **文件数** |
| :-----: | :--------: |
|   100   |     60     |
|   200   |     60     |
|   300   |     60     |
|   400   |     60     |
|   500   |     60     |
|  1,000  |    120     |
|  1,500  |    180     |
|  2,000  |    240     |
|  2,500  |    300     |
|  3,000  |    360     |
|  5,000  |    600     |
|  6,000  |    720     |
|  7,500  |    900     |
| 10,000  |    1200    |
| 15,000  |    1800    |
| 30,000  |    3600    |


### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-parquet-or-orc-files"></a>加载 Parquet 或 ORC 文件时，COPY 命令的文件拆分指导是什么？
不需要拆分 Parquet 或 ORC 文件，因为 COPY 命令会自动拆分这些文件。 为了获取再最佳性能，Azure 存储帐户中的 Parquet 或 ORC 文件应为 256 MB 或更大。 

### <a name="are-there-any-limitations-on-the-number-or-size-of-files"></a>文件的数量和大小有限制吗？
文件的数量或大小没有限制；但是，为了获得最佳性能，建议文件至少为 4 MB。

### <a name="are-there-any-known-issues-with-the-copy-statement"></a>COPY 语句是否存在已知问题？
如果有在 2020/12/07 之前创建的 Synapse 工作区，则使用托管标识进行身份验证时，可能会遇到类似的错误消息：

*com.microsoft.sqlserver.jdbc.SQLServerException：此服务器尚未启用托管服务标识。请先启用托管服务标识，再重试。*

请按照以下步骤操作，通过重新注册工作区的托管标识来规避此问题：

1. 在 Azure 门户中转到 Synapse 工作区
2. 转到“托管标识”边栏选项卡 
3. 如果已选中“允许管道”选项，则必须取消选中此设置并保存
4. 选中“允许管道”选项并保存


## <a name="see-also"></a>另请参阅  

 [使用 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 加载概述](/azure/sql-data-warehouse/design-elt-data-loading)
