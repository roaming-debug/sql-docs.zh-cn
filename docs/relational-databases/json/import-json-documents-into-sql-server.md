---
description: 将 JSON 文档导入 SQL Server
title: 导入 JSON 文档
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: 0e908ec0-7173-4cd2-8f48-2700757b53a5
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 34f8655a24a84d50c5f677d819330537fa030cc6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075038"
---
# <a name="import-json-documents-into-sql-server"></a>将 JSON 文档导入 SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

本文介绍如何将 JSON 文件导入 SQL Server。 目前有许多 JSON 文档存储在文件中。 应用程序在 JSON 文件中记录信息，传感器生成存储在 JSON 文件中的信息等。 必须能够读取文件中存储的 JSON 数据、将数据载入 SQL Server 和分析数据。

## <a name="import-a-json-document-into-a-single-column"></a>将 JSON 文档导入单个列

**OPENROWSET(BULK)** 是一个表值函数，如果 SQL Server 对本地驱动器或网络中的任一文件拥有读取访问权限，则该函数可从该文件中读取数据。 该函数返回包含单个列的表，该列包含文件的内容。 可以结合 OPENROWSET(BULK) 函数使用各种选项，例如分隔符。 但是，最简单的用法是将整个文件内容作为文本值加载。 （这单个大型值称为单字符大型对象或 SINGLE_CLOB。） 

下面的示例 **OPENROWSET(BULK)** 函数读取 JSON 文件的内容，然后以单个值的形式将该内容返回给用户：

```sql
SELECT BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j;
```

OPENJSON(BULK) 读取文件的内容并在 `BulkColumn` 中返回该内容。

也可以将文件内容载入局部变量或表中，如下面的示例所示：

```sql
-- Load file contents into a variable
SELECT @json = BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j

-- Load file contents into a table 
SELECT BulkColumn
 INTO #temp 
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j
```

加载 JSON 文件的内容后便可在表中保存 JSON 文本。


## <a name="import-json-documents-from-azure-file-storage"></a>从 Azure 文件存储导入 JSON 文档

可以使用上述 OPENROWSET(BULK) 从 SQL Server 可访问的其他文件位置读取 JSON 文件。 例如，Azure 文件存储支持 SMB 协议。 因此，你可以使用以下过程将本地虚拟驱动器映射到 Azure 文件存储共享：

1.  使用 Azure 门户或 Azure PowerShell 在 Azure 文件存储中创建一个文件存储帐户（例如 `mystorage`）、一个文件共享（例如 `sharejson`）和一个文件夹。
2.  将一些 JSON 文件上载到文件存储共享。
3.  在计算机上的 Windows 防火墙中创建一个允许端口 445 的出站防火墙规则。 请注意，Internet 服务提供商可能会阻止此端口。 如果以下步骤出现 DNS 错误（错误 53），则表示尚未打开端口 445，或者 ISP 阻止了该端口。
4. 将 Azure 文件存储共享装载为本地驱动器（例如 `T:`）。

    下面是命令语法：

    ```dos
    net use [drive letter] \\[storage name].file.core.windows.net\[share name] /u:[storage account name] [storage account access key]
    ```

    下面是向 Azure 文件存储共享分配本地驱动器号 `T:` 的示例：

    ```dos
    net use t: \\mystorage.file.core.windows.net\sharejson /u:myaccount hb5qy6eXLqIdBj0LvGMHdrTiygkjhHDvWjUZg3Gu7bubKLg==
    ```

    可以在 Azure 门户上“设置”对话框的“密钥”部分中找到存储帐户密钥以及主要或辅助存储帐户访问密钥。

5.  现在，可使用映射驱动器通过 Azure 文件存储共享访问 JSON 文件，如下面的示例中所示：

    ```sql
    SELECT book.* FROM
     OPENROWSET(BULK N't:\books\books.json', SINGLE_CLOB) AS json
     CROSS APPLY OPENJSON(BulkColumn)
     WITH( id nvarchar(100), name nvarchar(100), price float,
        pages_i int, author nvarchar(100)) AS book
    ```

有关 Azure 文件存储的详细信息，请参阅[文件存储](https://azure.microsoft.com/services/storage/files/)。

## <a name="import-json-documents-from-azure-blob-storage"></a>从 Azure Blob 存储导入 JSON 文档

可以使用 T-SQL BULK INSERT 命令或 OPENROWSET 函数，将文件从 Azure Blob 存储直接载入 Azure SQL 数据库。

> [!NOTE]
> 此功能已添加到 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 Azure SQL 中。

首先，按下面的示例所示创建外部数据源。

```sql
CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
 WITH ( TYPE = BLOB_STORAGE,
        LOCATION = 'https://myazureblobstorage.blob.core.windows.net',
        CREDENTIAL= MyAzureBlobStorageCredential);
```

接下来，结合 DATA_SOURCE 选项运行 BULK INSERT 命令。

```sql
BULK INSERT Product
FROM 'data/product.dat'
WITH ( DATA_SOURCE = 'MyAzureBlobStorage');
```

## <a name="parse-json-documents-into-rows-and-columns"></a>在行和列中分析 JSON 文档

你可能不希望以单个值的形式读取整个 JSON 文件，而是希望分析该文件，并将文件中的书籍及其属性以行和列的形式返回。 以下示例使用[此站点](https://github.com/tamingtext/book/blob/master/apache-solr/example/exampledocs/books.json)中的一个 JSON 文件，该文件包含书籍列表。

### <a name="example-1"></a>示例 1

在最简单的示例中，可以只从文件中加载整个列表。 

```sql
SELECT value
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
```

上述 OPENROWSET 从文件中读取单个文本值。 OPENROWSET 将值作为 BulkColumn 返回，并将 BulkColumn 传递给 OPENJSON 函数。 OPENJSON 循环访问 BulkColumn 数组中 JSON 对象的数组，并在每行中返回一本书籍。 每行格式为 JSON，如下所示。

```json
{"id":"978-0641723445", "cat":["book","hardcover"], "name":"The Lightning Thief", ... }
{"id":"978-1423103349", "cat":["book","paperback"], "name":"The Sea of Monsters", ... }
{"id":"978-1857995879", "cat":["book","paperback"], "name":"Sophie's World : The Greek", ... } 
{"id":"978-1933988177", "cat":["book","paperback"], "name":"Lucene in Action, Second", ... }
```

### <a name="example-2"></a>示例 2

OPENJSON 函数可分析 JSON 内容，并将其转换为表或结果集。 下面的示例加载内容，分析加载的 JSON，然后以列的形式返回五个字段：

```sql
SELECT book.*
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
 WITH( id nvarchar(100), name nvarchar(100), price float,
 pages_i int, author nvarchar(100)) AS book
```

在此示例中，OPENROWSET(BULK) 读取文件的内容，并使用定义的架构将该内容传递给 OPENJSON 函数，以便输出内容。 OPENJSON 使用列名匹配 JSON 对象中的属性。 例如，`price` 属性将以 `price` 列的形式返回，并转换为浮点数据类型。 结果如下：

|ID|名称|price|pages_i|作者|
|---|---|---|---|---|
|978-0641723445|The Lightning Thief|12.5|384|Rick Riordan| 
|978-1423103349|The Sea of Monsters|6.49|304|Rick Riordan| 
|978-1857995879|Sophie’s World : The Greek Philosophers|3.07|64|Jostein Gaarder| 
|978-1933988177|Lucene in Action, Second Edition|30.5|475|Michael McCandless|
||||||

现在，可将此表返回给用户，或者将数据载入另一个表。

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅

[使用 OPENJSON 将 JSON 数据转换为行和列](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)

