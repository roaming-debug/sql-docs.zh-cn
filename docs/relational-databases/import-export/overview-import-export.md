---
title: 在 SQL Server 和 Azure SQL 数据库中导入和导出数据
description: 你可以使用 Transact-SQL、命令行工具和向导，使用各种数据格式在 SQL Server 和 Azure SQL 数据库中导入和导出数据。
ms.date: 10/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: d934e6f1c5457297e385fbe8d42c2709ed2c0f88
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438778"
---
# <a name="import-and-export-data-from-sql-server-and-azure-sql-database"></a>在 SQL Server 和 Azure SQL 数据库中导入和导出数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
可通过多种方法将数据导入 SQL Server 和 Azure SQL 数据库，以及从 SQL Server 和 Azure SQL 数据库中导出数据。 这些方法包括 Transact-SQL 语句、命令行工具和向导。

也可以各种数据格式导入和导出数据。 这些格式包括平面文件、Excel、主要关系数据库和各种云服务。

## <a name="methods-for-importing-and-exporting-data"></a>导入和导出数据的方法

### <a name="use-transact-sql-statements"></a>使用 Transact-SQL 语句
可以使用 `BULK INSERT` 或 `OPENROWSET(BULK...)` 命令导入数据。 通常在 SQL Server Management Studio (SSMS) 中运行这些命令。 有关详细信息，请参阅[使用 BULK INSERT 或 OPENROWSET(BULK...) 导入批量数据](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)。

### <a name="use-bcp-from-the-command-prompt"></a>从命令提示符使用 BCP
可使用 BCP 命令行实用工具导入和导出数据。 有关详细信息，请参阅[使用 BCP 实用工具导入和导出批量数据](import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)。

### <a name="use-the-import-flat-file-wizard"></a>使用导入平面文件向导
如果并不需要导入和导出向导和其他工具中提供的所有配置选项，则可以使用 SQL Server Management Studio (SSMS) 中的“导入平面文件向导”  将文本文件导入 SQL Server。 有关详细信息，请参阅以下文章：
- [将平面文件导入 SQL 向导](import-flat-file-wizard.md)
- [SQL Server Management Studio 17.3 中的新增功能](https://blogs.technet.microsoft.com/dataplatforminsider/2017/10/10/whats-new-in-sql-server-management-studio-17-3/)
- [SSMS 17.3 中的新导入平面文件向导简介](https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173)

### <a name="use-the-sql-server-import-and-export-wizard"></a>使用 SQL Server 导入和导出向导
可使用 SQL Server 导入和导出向导将数据导入各种源和目标，或从其中导出数据。 要使用向导，必须安装 SQL Server Integration Services (SSIS) 或 SQL Server Data Tools (SSDT)。 有关详细信息，请参阅[使用 SQL Server 导入和导出向导导入和导出数据](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。

### <a name="design-your-own-import-or-export"></a>设计自己的导入或导出
如果想要设计自定义数据导入，可使用以下功能或服务中的一项：
-   SQL Server Integration Services。 有关详细信息，请参阅 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)。
-   Azure 数据工厂。 有关详细信息，请参阅 [Azure 数据工厂简介](/azure/data-factory/data-factory-introduction)。

## <a name="data-formats-for-import-and-export"></a>导入和导出的数据格式

### <a name="supported-formats"></a>支持的格式

可使用多种格式导入和导出数据，包括平面文件或多种其他文件格式、关系数据库和云服务。 要详细了解特定工具的这些选项，请参阅以下主题
-   对于 SQL Server 导入和导出向导，请参阅[使用 SQL Server 导入和导出向导连接到数据源](../../integration-services/import-export-data/connect-to-data-sources-with-the-sql-server-import-and-export-wizard.md)。
-   对于 SQL Server Integration Services，请参阅 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)。
-   对于 Azure 数据工厂，请参阅 [Azure 数据工厂连接器](/azure/data-factory/data-factory-amazon-redshift-connector)。

### <a name="commonly-used-data-formats"></a>常用的数据格式

某些常用的数据格式具有特殊的注意事项和示例。 要了解有关这些数据格式的详细信息，请参阅以下主题：
-   对于 Excel，请参阅[从 Excel 导入](import-data-from-excel-to-sql.md)。
-   对于 JSON，请参阅[导入 JSON 文档](../json/import-json-documents-into-sql-server.md)。
-   对于 XML，请参阅[导入和导出 XML 文档](examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)。
-   对于 Azure Blob 存储，请参阅[从 Azure Blob 存储导入和导出](examples-of-bulk-access-to-data-in-azure-blob-storage.md)。

## <a name="next-steps"></a>后续步骤
如果不确定从何处开始进行导入或导出任务，请考虑使用 SQL Server 导入和导出向导。 有关快速说明，请参阅[导入和导出向导的简单示例入门](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)。