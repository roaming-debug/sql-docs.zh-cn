---
title: 大容量导入和导出数据 (SQL Server) | Microsoft Docs
description: SQL Server 支持从 SQL Server 表批量导出数据，以及将批量数据导入 SQL Server 表或未分区的视图。
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- exporting data
- bulk importing [SQL Server], about bulk importing
- data [SQL Server], bulk export and import
- transferring data
- importing data, (See also bulk importing [SQL Server])
- copying data [SQL Server]
- bulk exporting [SQL Server]
- importing data, bulk import
- copying data [SQL Server], bulk export and import
- exporting data,(See also bulk exporting [SQL Server])
- bulk exporting [SQL Server], about bulk exporting
- bulk importing [SQL Server]
- importing data
ms.assetid: 19049021-c048-44a2-b38d-186d9f9e4a65
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8429b376cee1b701d95320ecc202daa66155419d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480128"
---
# <a name="bulk-import-and-export-of-data-sql-server"></a>大容量导入和导出数据 (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表大容量导出数据（“大容量数据”  ）以及将大容量数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或未分区的视图。

- “大容量导出”  是指将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表复制到数据文件。
- “大容量导入”  是指将数据从数据文件加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 例如，您可以将数据从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 应用程序导出到数据文件，然后将这些数据大容量导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。

## <a name="methods-for-bulk-importing-and-exporting-data"></a><a name="MethodsForBuliIE"></a> 批量导入和导出数据的方法

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表大容量导出数据以及将数据大容量导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或未分区的视图。 可以使用下列基本方法：

|方法|说明|导入数据|导出数据|
|------------|-----------------|------------------|------------------|
|[bcp 实用工具](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)|大容量导出数据和大容量导入数据并生成格式化文件的命令行实用工具 (Bcp.exe)。|是|是|
|[BULK INSERT 语句](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)|将数据直接从数据文件导入数据库表或未分区视图的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|是|否|
|[INSERT ...SELECT * FROM OPENROWSET(BULK...) 语句](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)|通过在 INSERT 语句中指定 OPENROWSET(BULK…) 函数来选择数据，从而使用 OPENROWSET 大容量行集提供程序将数据大容量导入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句。|是|否|
|[SQL Server 导入和导出向导](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)|此向导可创建简单包，以便导入和导出介于多种常用数据格式（包括数据库、电子表格和文本文件）的数据。|是|是|

> [!IMPORTANT]
> 有关将逗号分隔值 (CSV) 文件用作将数据批量导入到 SQL Server 的数据文件的规则，请参阅[准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)。

> [!NOTE]
> Azure Synapse Analytics 仅支持使用 bcp 实用工具导入和导出带分隔符的文件。

## <a name="format-files"></a><a name="FFs"></a> 格式化文件

[bcp 实用工具](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 以及 [INSERT ...SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 都支持使用专门的“格式化文件”  来存储数据文件中每个字段的格式信息。 格式化文件还可以包含相应的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的有关信息。 格式化文件可以用于提供从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例大容量导出数据和向其中大容量导入数据时所需的所有格式信息。

> [!IMPORTANT]
> 不能使用 BCP 将数据从 Azure blob 存储导入，也不能将数据从 Azure blob 存储导出到 Azure SQL 数据库。 使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 从 Azure blob 存储导入数据或将数据导出到 Azure blob 存储。

格式化文件提供了一种解释导入期间数据文件中数据的格式以及设置导出期间数据文件中数据格式的灵活方式。 这种灵活性使得解释数据时无需编写专用代码，也无需为满足 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或外部应用程序的特殊需要而重新设置数据的格式。 例如，如果将要加载的数据大容量导出到某个需要逗号分隔值的应用程序，则可以使用格式化文件将逗号作为字段终止符插入导出的数据中。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持两种格式化文件类型：XML 格式化文件和非 XML 格式化文件。

[bcp 实用工具](../../tools/bcp-utility.md) 是唯一能够生成格式化文件的工具。 有关详细信息，请参阅 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)。 有关格式化文件的详细信息，请参阅[导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。

> [!NOTE]
> 如果在大容量导出或导入操作期间未提供格式化文件，您可以在命令行处覆盖默认格式。

|相关主题|
|---|
|[准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)|
|[用于大容量导入或导出的数据格式 (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)<br />&emsp;&#9679;&emsp;[使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)|
|[在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 bcp 指定字段长度 (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)<br />&emsp;&#9679;&emsp;[指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)|
|[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)|
|[大容量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)|
|[用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)<br />&emsp;&#9679;&emsp;[创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)|

## <a name="more-information"></a>详细信息

- [在批量导入中按最小方式记录日志的前提条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)
- [批量导入和导出 XML 文档的示例 (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)
- [将数据库复制到其他服务器](../../relational-databases/databases/copy-databases-to-other-servers.md)
- [对 XML 数据执行大容量加载 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)
- [执行大容量复制操作](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)
