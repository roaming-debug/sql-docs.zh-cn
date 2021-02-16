---
title: 将 Excel 数据导入 SQL | Microsoft Docs
description: 本文介绍将 Excel 数据导入 SQL Server 或 Azure SQL 数据库的方法。 有些只使用一个步骤，而其他则需要一个中间文本文件。
ms.custom: sqlfreshmay19
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 84fce410f5ee03301b5ad8e5fe0aa61698b52e0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351342"
---
# <a name="import-data-from-excel-to-sql-server-or-azure-sql-database"></a>将 Excel 数据导入 SQL Server 或 Azure SQL 数据库

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

将 Excel 文件中的数据导入 SQL Server 或 Azure SQL 数据库的方法有多种。 某些方法允许你在单个步骤中从 Excel 文件直接导入数据，其他方法要求在导入数据前，必须将 Excel 数据先导出为文本 (CSV 文件)。 本文总结了常用的方法，并提供有关更为详细的信息的链接。

## <a name="list-of-methods"></a>方法列表

可使用以下工具从 Excel 导入数据：

| 首先导出到文本（SQL Server 和 SQL 数据库） | 直接从 Excel（仅本地 SQL Server）进行 |
| :------------------------------------------------- |:------------------------------------------------- |
| [导入平面文件向导](#import-wiz)             |[SQL Server 导入和导出向导](#wiz)        |
| [BULK INSERT](#bulk-insert) 语句              |[SQL Server Integration Services (SSIS)](#ssis)    |
| [BCP](#bcp)                                        |[OPENROWSET](#openrowset) 函数 <br>            |
| [复制向导（Azure 数据工厂）](#adf-wiz)       |                                                   |
| [Azure 数据工厂](#adf)                         |                                                   |
| &nbsp; | &nbsp; |

如果要从 Excel 工作簿导入多个工作表，通常必须为每个工作表运行一次其中任何工具。

SSIS 或 Azure 数据工厂等复杂工具和服务的完整描述不属于本表的范围。 要详细了解感兴趣的解决方案，请单击所提供的链接。

> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../../integration-services/load-data-to-from-excel-with-ssis.md)。

如果尚未安装 SQL Server，或已具有 SQL Server 但未安装 SQL Server Management Studio，请参阅 [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="sql-server-import-and-export-wizard"></a><a name="wiz"></a>SQL Server 导入和导出向导

通过单步执行 SQL Server 导入和导出向导各页面，直接从 Excel 文件导入数据。 （可选）将设置保存为可以稍后自定义和重用的 SQL Server Integration Services (SSIS) 包。

1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例。

2. 展开 **“数据库”** 。
3. 右键单击某个数据库。
4. 指向“任务”  。
5. 单击以下选项之一。

  - **导入数据**
  - **导出数据**

    ![启动向导 SSMS](../../integration-services/import-export-data/media/start-wizard-ssms.jpg)

![连接到 Excel 数据源](media/excel-connection.png)

有关使用向导将 Excel 导数据入 SQL Server 的示例，请参阅[从导入和导出向导的这个简单示例入手](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)。

要了解“导入和导出”向导的其他启动方法，请参阅[启动 SQL Server 导入和导出向导](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md)。

## <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)

如果熟悉 SSIS，并且不想运行 SQL Server 导入和导出向导，请创建在数据流中使用 Excel 源和 SQL Server 目标的 SSIS 包。

有关这些 SSIS 组件的详细信息，请参阅以下主题：

- [Excel 源](../../integration-services/data-flow/excel-source.md)
- [SQL Server 目标](../../integration-services/data-flow/sql-server-destination.md)

若要开始学习如何生成 SSIS 包，请参阅教程[如何创建 ETL 包](../../integration-services/ssis-how-to-create-an-etl-package.md)。

![数据流中的组件](media/excel-to-sql-data-flow.png)

## <a name="openrowset-and-linked-servers"></a><a name="openrowset"></a> OPENROWSET 和链接服务器

> [!IMPORTANT]
> 在 Azure SQL 数据库中，无法直接从 Excel 导入。 必须首先将数据导出到文本 (CSV) 文件。 有关示例，请参阅[示例](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

> [!NOTE]
> 连接到 Excel 数据源的 ACE 提供程序（前身为 Jet 提供程序）旨在用于交互式客户端用途。 如果在 SQL Server 上使用 ACE 提供程序，尤其是在自动进程或并行运行的进程中，可能会发生意外结果。

### <a name="distributed-queries"></a>分布式查询

使用 Transact-SQL `OPENROWSET` 或 `OPENDATASOURCE` 函数直接从 Excel 文件导入 SQL Server。 这种用法称为“分布式查询”  。

> [!IMPORTANT]
> 在 Azure SQL 数据库中，无法直接从 Excel 导入。 必须首先将数据导出到文本 (CSV) 文件。 有关示例，请参阅[示例](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

必须先启用 `ad hoc distributed queries` 服务器配置选项（如以下示例所示），然后才能运行分布式查询。 有关详细信息，请参阅[即席分布式查询服务器配置选项](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md)。

```sql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO
sp_configure 'Ad Hoc Distributed Queries', 1;
RECONFIGURE;
GO
```

下面的代码示例使用 `OPENROWSET`，将 Excel `Sheet1` 工作表中的数据导入新的数据库表。

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENROWSET('Microsoft.ACE.OLEDB.12.0',
    'Excel 12.0; Database=C:\Temp\Data.xlsx', [Sheet1$]);
GO
```

下面的示例用途相同，区别在于使用的是 `OPENDATASOURCE`。

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0',
    'Data Source=C:\Temp\Data.xlsx;Extended Properties=Excel 12.0')...[Sheet1$];
GO
```

若要将导入的数据追加  到现有  表，而不是新建表，请使用 `INSERT INTO ... SELECT ... FROM ...` 语法，而不是上面示例中使用的 `SELECT ... INTO ... FROM ...` 语法。

若要查询（而不是导入）Excel 数据，只需使用标准 `SELECT ... FROM ...` 语法。

有关分布式查询的详细信息，请参阅以下主题：

- [分布式查询](/previous-versions/sql/sql-server-2008-r2/ms188721(v=sql.105))（SQL Server 2016 仍支持分布式查询，但此功能的相关文档尚未更新。）
- [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)
- [OPENDATASOURCE](../../t-sql/functions/openquery-transact-sql.md)

### <a name="linked-servers"></a>链接服务器

还可以将从 SQL Server 到 Excel 文件的永久性连接配置为链接服务器  。 下面的示例将现有 Excel 链接服务器 `EXCELLINK` 上的 `Data` 工作表数据导入名为 `Data_ls` 的新 SQL Server 数据库表。

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_ls FROM EXCELLINK...[Data$];
GO
```

可以通过 SQL Server Management Studio 或运行系统存储过程 `sp_addlinkedserver`（如以下示例所示）创建链接服务器。

```sql
DECLARE @RC int

DECLARE @server     nvarchar(128)
DECLARE @srvproduct nvarchar(128)
DECLARE @provider   nvarchar(128)
DECLARE @datasrc    nvarchar(4000)
DECLARE @location   nvarchar(4000)
DECLARE @provstr    nvarchar(4000)
DECLARE @catalog    nvarchar(128)

-- Set parameter values
SET @server =     'EXCELLINK'
SET @srvproduct = 'Excel'
SET @provider =   'Microsoft.ACE.OLEDB.12.0'
SET @datasrc =    'C:\Temp\Data.xlsx'
SET @provstr =    'Excel 12.0'

EXEC @RC = [master].[dbo].[sp_addlinkedserver] @server, @srvproduct, @provider,
@datasrc, @location, @provstr, @catalog
```

有关链接服务器的详细信息，请参阅以下主题：

- [创建链接服务器](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)
- [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)

有关链接服务器和分布式查询的更多示例和详细信息，请参阅以下主题：

- [如何通过 SQL Server 链接服务器和分布式查询使用 Excel](https://support.microsoft.com/help/306397/how-to-use-excel-with-sql-server-linked-servers-and-distributed-queries)
- [如何将 Excel 数据导入 SQL Server](https://support.microsoft.com/help/321686/how-to-import-data-from-excel-to-sql-server)

## <a name="prerequisite---save-excel-data-as-text"></a><a name="prereq"></a>先决条件 - 将 Excel 数据保存为文本

若要使用本页上的其他方法（BULK INSERT 语句、BCP 工具或 Azure 数据工厂），必须先将 Excel 数据导出到文本文件中。

在 Excel 中，依次选择“文件”|“另存为”，再选择“文本文件(制表符分隔)(.txt)”或“CSV (逗号分隔)(.csv)”作为目标文件类型  **\*** **\*** 。

如果要从工作簿中导出多个工作表，请选择每个工作表，然后重复此过程。 “另存为”命令仅导出活动工作表  。

> [!TIP]
> 为在使用数据导入工具时获得最佳结果，保存仅包含列标题和数据行的工作表。 如果保存的数据包含页标题、空白行、注释等，稍后可能会在导入数据时发生意外结果。

## <a name="the-import-flat-file-wizard"></a><a name="import-wiz"></a> 导入平面文件向导

通过单步执行导入平面文件向导各页面，导入保存为文本文件的数据。

如前面[先决条件](#prereq)部分中所述，必须先将 Excel 数据导出为文本，然后才能使用导入平面文件向导导入它。

有关导入平面文件向导的详细信息，请参阅[将平面文件导入到 SQL 向导](import-flat-file-wizard.md)。

## <a name="bulk-insert-command"></a><a name="bulk-insert"></a> BULK INSERT 命令

`BULK INSERT` 是可以通过 SQL Server Management Studio 运行的 Transact-SQL 命令。 下面的示例将 `Data.csv` 逗号分隔文件中的数据加载到现有数据库表中。

如前面[先决条件](#prereq)部分中所述，必须先将 Excel 数据导出为文本，然后才能使用 BULK INSERT 导入它。 BULK INSERT 无法直接读取 Excel 文件。 使用 BULK INSERT 命令，可以导入存储在本地或在 Azure Blob 存储中的 CSV 文件。

```sql
USE ImportFromExcel;
GO
BULK INSERT Data_bi FROM 'C:\Temp\data.csv'
   WITH (
      FIELDTERMINATOR = ',',
      ROWTERMINATOR = '\n'
);
GO
```

有关 SQL Server 和 SQL 数据库的详细信息和示例，请参阅以下主题：

- [使用 BULK INSERT 或 OPENROWSET(BULK...) 导入批量数据](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)

## <a name="bcp-tool"></a><a name="bcp"></a> BCP 工具

BCP 是通过命令提示符运行的程序。 下面的示例将 `Data.csv` 逗号分隔文件中的数据加载到现有 `Data_bcp` 数据库表中。

如前面[先决条件](#prereq)部分中所述，必须先将 Excel 数据导出为文本，然后才能使用 BCP 导入它。 BCP 无法直接读取 Excel 文件。 用于从保存在本地存储的测试 (CSV) 文件中导入 SQL Server 或 SQL 数据库。

> [!IMPORTANT]
> 对于存储在 Azure Blob 存储中的文本 (CSV) 文件，请使用 BULK INSERT 或 OPENROWSET。 有关示例，请参阅[示例](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

```console
bcp.exe ImportFromExcel..Data_bcp in "C:\Temp\data.csv" -T -c -t ,
```

有关 BCP 的详细信息，请参阅以下主题：

- [使用 bcp 实用工具导入和导出批量数据](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [bcp 实用工具](../../tools/bcp-utility.md)
- [准备用于批量导出或导入的数据](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)

## <a name="copy-wizard-azure-data-factory"></a><a name="adf-wiz"></a> 复制向导（Azure 数据工厂）

通过单步执行 Azure 数据工厂复制向导各页面，导入保存为文本文件的数据。

如前面[先决条件](#prereq)部分中所述，必须先将 Excel 数据导出为文本，然后才能使用 Azure 数据工厂导入它。 数据工厂无法直接读取 Excel 文件。

有关复制向导的详细信息，请参阅以下主题：

- [数据工厂复制向导](/azure/data-factory/data-factory-azure-copy-wizard)
- [教程：使用数据工厂复制向导创建带有复制活动的管道](/azure/data-factory/data-factory-copy-data-wizard-tutorial)。

## <a name="azure-data-factory"></a><a name="adf"></a> Azure 数据工厂

如果熟悉 Azure 数据工厂，并且不想运行复制向导，请创建带有复制活动的管道，用于将文本文件复制到 SQL Server 或 Azure SQL 数据库。

如前面[先决条件](#prereq)部分中所述，必须先将 Excel 数据导出为文本，然后才能使用 Azure 数据工厂导入它。 数据工厂无法直接读取 Excel 文件。

若要详细了解如何使用这些数据工厂源和接收器，请参阅以下主题：

- [文件系统](/azure/data-factory/data-factory-onprem-file-system-connector)
- [SQL Server](/azure/data-factory/data-factory-sqlserver-connector)
- [Azure SQL 数据库](/azure/data-factory/data-factory-azure-sql-connector)

若要开始学习如何使用 Azure 数据工厂复制数据，请参阅以下主题：

- [使用复制活动移动数据](/azure/data-factory/data-factory-data-movement-activities)
- [教程：使用 Azure 门户创建带有复制活动的管道](/azure/data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database)

## <a name="common-errors"></a>常见错误

### <a name="microsoftaceoledb120-has-not-been-registered"></a>Microsoft.ACE.OLEDB.12.0 尚未注册

发生此错误的原因是未安装 OLEDB 提供程序。 请通过 [Microsoft Access 数据库引擎 2010 可再发行组件](https://www.microsoft.com/download/details.aspx?id=13255)进行安装。 如果 Windows 和 SQL Server 都是 64 位，请务必安装 64 位版本。

完整错误为：

```
Msg 7403, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" has not been registered.
```

### <a name="cannot-create-an-instance-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>无法为链接服务器 "(null)" 创建 OLE DB 提供程序 "Microsoft.ACE.OLEDB.12.0" 的实例

这表示 Microsoft OLEDB 配置错误。 运行以下 Transact-SQL 代码可解决此问题：

```sql
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'AllowInProcess', 1
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'DynamicParameters', 1
```

完整错误为：

```
Msg 7302, Level 16, State 1, Line 3
Cannot create an instance of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

### <a name="the-32-bit-ole-db-provider-microsoftaceoledb120-cannot-be-loaded-in-process-on-a-64-bit-sql-server"></a>32 位 OLE DB 提供程序 "Microsoft.ACE.OLEDB.12.0" 无法在 64 位 SQL Server 上的进程内加载

32 位版本的 OLD DB 提供程序与 64 位 SQL Server 一起安装时，会发生此情况。 要解决此问题，请卸载 32 位版本，改为安装 64 位版本的 OLE DB 提供程序。

完整错误为：

```
Msg 7438, Level 16, State 1, Line 3
The 32-bit OLE DB provider "Microsoft.ACE.OLEDB.12.0" cannot be loaded in-process on a 64-bit SQL Server.
```

### <a name="the-ole-db-provider-microsoftaceoledb120-for-linked-server-null-reported-an-error-the-provider-did-not-give-any-information-about-the-error"></a>链接服务器 "(null)" 的 OLE DB 提供程序 "Microsoft.ACE.OLEDB.12.0" 报告了错误。 提供程序未给出有关错误的任何信息

### <a name="cannot-initialize-the-data-source-object-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>无法初始化链接服务器 "(null)" 的 OLE DB 提供程序 "Microsoft.ACE.OLEDB.12.0" 的数据源对象

这两个错误通常表示 SQL Server 进程和文件之间存在权限问题。 请确保运行 SQL Server 服务的帐户对文件具有完全访问权限。 建议不要尝试从桌面导入文件。

完整错误为：

```
Msg 7399, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)" reported an error. The provider did not give any information about the error.
```

```
Msg 7303, Level 16, State 1, Line 3
Cannot initialize the data source object of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

## <a name="see-also"></a>另请参阅

[使用 SQL Server Integration Services (SSIS) 将数据导入 Excel 或从 Excel 导出数据](../../integration-services/load-data-to-from-excel-with-ssis.md)