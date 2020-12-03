---
description: Azure SQL DW 上传任务
title: Azure SQL DW 上传任务 | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPDWUPTASK.F1
- sql14.dts.designer.afpdwuptask.f1
ms.assetid: eef82c89-228a-4dc7-9bd0-ea00f57692f5
author: Lingxi-Li
ms.author: lingxl
ms.openlocfilehash: 5510fb2a0a4760b5465dad7f44eed8c85ef36251
ms.sourcegitcommit: ece151df14dc2610d96cd0d40b370a4653796d74
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96297954"
---
# <a name="azure-sql-dw-upload-task"></a>Azure SQL DW 上传任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Azure SQL DW 上传任务启用 SSIS 包，将表格数据从文件系统或 Azure Blob 存储复制到 Azure Synapse Analytics (DW)。
该任务利用 PolyBase 来改进性能，如 [Azure Synapse Analytics 加载模式和策略](/archive/blogs/sqlcat/azure-sql-data-warehouse-loading-patterns-and-strategies)一文中所述。
当前支持的源数据文件格式是采用 UTF8 编码的带分隔符的文本。
当从文件系统复制时，数据首先将上传到 Azure Blob 存储进行暂存，然后上传到 Azure SQL DW。 因此，需要 Azure Blob 存储帐户。

> [!NOTE]
> 不支持将 Azure 存储连接管理器用于 Data Lake Gen2 服务类型。
>
> 若要为暂存或源使用 Azure Data Lake Gen2，可通过 Azure 存储连接管理器与 Blob 存储服务类型进行连接。

“Azure SQL DW 上传任务”  是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组件。

若要添加“Azure SQL DW 上传任务”  ，请将其从 SSIS 工具栏拖放到设计器画布中，双击或右键单击它，然后单击“编辑”  ，查看任务编辑器对话框。

在“常规”  页上配置以下属性。

SourceType  指定源数据存储的类型。 选择下列类型之一：

* FileSystem：源数据位于本地文件系统中。
* BlobStorage：源数据位于 Azure Blob 存储中。

以下是每个源类型的属性。

### <a name="filesystem"></a>FileSystem

字段|说明
-----|-----------
LocalDirectory|指定包含待上传数据文件的本地目录。
Recursively|指定是否以递归方式搜索子目录。
FileName|指定用于选择具有特定名称模式的文件的名称筛选器。 例如 MySheet*.xls\* 将包含如 MySheet001.xsl 和 MySheetABC.xslx 等文件。
RowDelimiter|指定标记每一行末尾的字符。
ColumnDelimiter|指定标记每一列末尾的一个或多个字符。 例如 &#124;（管道）、\t（制表符）、'（单引号），"（双引号）以及 0x5c（反斜杠）。
IsFirstRowHeader|指定每个数据文件的第一行是否包含列名称，而非实际数据。
AzureStorageConnection|指定 Azure 存储连接管理器。
BlobContainer|指定通过 PolyBase 将本地数据上传并转送到 Azure DW 的目标 blob 容器名称。 如果此容器不存在，则将创建新容器。
BlobDirectory|指定通过 PolyBase 将本地数据上传并转送到 Azure DW 的目标 blob 目录（虚拟层次结构）。
RetainFiles|指定是否保留已上传到 Azure 存储的文件。
CompressionType|指定将文件上传到 Azure 存储时使用的压缩格式。 本地源不受影响。
CompressionLevel|指定用于压缩格式的压缩级别。
AzureDwConnection|指定 Azure SQL DW 的 ADO.NET 连接管理器。
TableName|指定目标表的名称。 可选择现有的表名称，或选择“\<New Table ...>”创建一个新表。
TableDistribution|指定新表的分发方法。 已为 **TableName** 指定新的表名称时适用。
HashColumnName|指定用于哈希表分发的列。 已为 **TableDistribution** 指定 **HASH** 时适用。

### <a name="blobstorage"></a>BlobStorage

字段|说明
-----|-----------
AzureStorageConnection|指定 Azure 存储连接管理器。
BlobContainer|指定源数据所在的 blob 容器的名称。
BlobDirectory|指定源数据所在的 blob 目录（虚拟层次结构）。
RowDelimiter|指定标记每一行末尾的字符。
ColumnDelimiter|指定标记每一列末尾的一个或多个字符。 例如 &#124;（管道）、\t（制表符）、'（单引号），"（双引号）以及 0x5c（反斜杠）。
CompressionType|指定用于源数据的压缩格式。
AzureDwConnection|指定 Azure SQL DW 的 ADO.NET 连接管理器。
TableName|指定目标表的名称。 可选择现有的表名称，或选择“\<New Table ...>”创建一个新表。
TableDistribution|指定新表的分发方法。 已为 **TableName** 指定新的表名称时适用。
HashColumnName|指定用于哈希表分发的列。 已为 **TableDistribution** 指定 **HASH** 时适用。

根据是复制到新表还是现有表，看到的“映射”  页面会有所不同。
如果是前者，请在待创建目标表中配置要映射到的源列及其对应名称。
如果是后者，请配置源和目标列之间的映射关系。

在“列”  页上，配置每个源列的数据类型属性。

“T-SQL”  页显示用于将数据从 Azure Blob 存储加载到 Azure SQL DW 的 T-SQL。
T-SQL 从其他页面的配置中自动生成，并在任务执行的过程中执行。
若要满足特定需求，可通过单击“编辑”  按钮选择手动编辑已生成的 T-SQL。
之后可单击“重置”  按钮还原为自动生成的 T-SQL。