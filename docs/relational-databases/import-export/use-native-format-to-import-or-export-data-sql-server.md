---
title: 使用本机格式导入和导出数据
description: 在 SQL Server 导入或导出中，本机格式维护数据库的本机数据类型，以便在 SQL Server 表之间进行高速数据传输。
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 30b9d8f332c1dbb8147b836309724f2216cb9de1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750397"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>使用本机格式导入或导出数据 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
当使用不包含任何扩展/双字节字符集 (DBCS) 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间大容量传输数据时，建议使用本机格式。  

> [!NOTE]
>  若要使用包含扩展字符或 DBCS 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间大容量传输数据，应使用 Unicode 本机格式。 有关详细信息信息，请参阅 [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)。

本机格式保留数据库的本机数据类型。 本机格式适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表之间的高速数据传输。 如果使用格式化文件，则源表和目标表不必相同。 数据传输分为两个步骤：  
  
1.  将源表中的数据批量导出到数据文件中  
  
2.  将数据文件中的数据批量导入到目标表中  

在相同的表之间使用本机格式避免了在各数据类型与字符格式之间进行不必要的转换，从而节省了时间和空间。 但是，若要获得最佳的传输速率，应执行几个有关数据格式的检查。 为了防止加载的数据出现问题，请参阅以下限制列表。  


## <a name="restrictions"></a>限制
若要成功导入本机格式的数据，请确保：  
  
-   数据文件是本机格式的文件。  
  
-   目标表必须与数据文件（含有正确的列数、数据类型、长度及 NULL 状态等）兼容，或者您必须使用格式化文件将每一个字段映射到其相应列。  
  
    > [!NOTE]
    >  如果从与目标表不匹配的文件中导入数据，那么导入操作可能会成功，但插入到目标表中的数据值很可能是错误的。 这是由于文件中的数据是通过使用目标表的格式来解释的。 因此，任何不匹配都会导致插入错误值。 但是，这样的不匹配决不会导致数据库中出现逻辑或物理不一致。  
  
     有关使用格式化文件的信息，请参阅[用于导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。  
  
 成功的导入操作不会损坏目标表。  
  
## <a name="how-bcp-handles-data-in-native-format"></a>bcp 如何处理本机格式的数据
 本节论述了 **bcp** 实用工具如何导出和导入本机格式数据的特殊注意事项。  
  
-   非字符数据  
  
     [bcp 实用工具](../../tools/bcp-utility.md) 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部二进制数据格式将表中的非字符数据写入数据文件中。  
  
-   [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 数据或 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) 数据  
  
     在每个 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 字段或 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) 字段的开头， [bcp](../../tools/bcp-utility.md) 都添加前缀长度。  
  
    > [!IMPORTANT]
    >  使用本机模式时，默认情况下， [bcp 实用工具](../../tools/bcp-utility.md) 先将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的字符转换为 OEM 字符，然后将这些字符复制到数据文件中。 [bcp 实用工具](../../tools/bcp-utility.md) 先将数据文件中的字符转换为 ANSI 字符，然后将这些字符批量导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。 在执行这些转换过程中，可能丢失扩展字符数据。 对于扩展字符，请使用 Unicode 本机格式或指定代码页。
  
-   [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 数据  
  
     如果 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 数据以 SQLVARIANT 存储在本机格式数据文件中，则数据会保留其所有特征。 记录每个数据值的数据类型的元数据与数据值一起存储。 此元数据用于在目标 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 列中重新创建具有相同数据类型的数据值。  
  
     如果目标列的数据类型不是 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)，则每个数据值将按照隐式数据转换的一般规则转换为目标列的数据类型。 如果在数据转换过程中出现错误，则回滚当前批。 在 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 列之间传输的任何 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 和 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) 值都可能存在代码页转换问题。  
  
     有关数据转换的详细信息，请参阅[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)。  
  
## <a name="command-options-for-native-format"></a>本机格式的命令选项
可以使用 [bcp](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [INSERT ... 将本机格式数据导入表中SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)。对于 [bcp](../../tools/bcp-utility.md) 命令或 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句，你可以在语句中指定数据格式。  对于 [INSERT ...SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句，必须在格式化文件中指定数据格式。  

下列命令选项支持本机格式：  

|Command|选项|说明|  
|-------------|------------|-----------------|  
|bcp|**-n**|使 bcp 实用工具使用本机数据类型的数据。*|  
|BULK INSERT|DATAFILETYPE **='native'**|使用本机数据类型或宽本机数据类型的数据。 注意，如果格式化文件指定了数据类型，则不需要 DATAFILETYPE。|  
|OPENROWSET|空值|必须使用格式化文件|

  
 \*若要将本机 ( **-n**) 数据加载到与早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端兼容的格式，请使用 **-V** 开关。 有关详细信息，请参阅 [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)。  
  
> [!NOTE]
>  或者，您可以在格式化文件中为每个字段指定格式设置。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。
  

## <a name="example-test-conditions"></a>示例测试条件
本主题中的示例基于下面定义的表和格式化文件。

### <a name="sample-table"></a>示例表
以下脚本将创建测试数据库、名为 `myNative` 的表并用一些初始值填充表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>示例非 XML 格式化文件
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myNative.fmt`生成非 XML 格式化文件 `myNative`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>示例
下面的示例使用上面创建的数据库和格式化文件。

### <a name="using-bcp-and-native-format-to-export-data"></a>使用 bcp 和本机格式导出数据
**-n** 切换和 **OUT** 命令。  请注意：此示例中创建的数据文件将用于所有后续示例中。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>使用 bcp 和本机格式导入数据，而不使用格式化文件
**-n** 切换和 **IN** 命令。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>在使用非 XML 格式化文件的情况下，使用 bcp 和本机格式导入数据
**-n** 和 **-f** 切换以及 **IN** 命令。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>使用 BULK INSERT 和本机格式，而不使用格式化文件
**DATAFILETYPE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>在使用非 XML 格式化文件的情况下，使用 BULK INSERT 和本机格式
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>在使用非 XML 格式化文件的情况下，使用 OPENROWSET 和本机格式
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>相关任务
使用数据格式进行批量导入或批量导出 
  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
