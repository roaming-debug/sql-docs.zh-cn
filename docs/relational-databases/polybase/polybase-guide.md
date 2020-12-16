---
title: 什么是 PolyBase？ | Microsoft Docs
description: 借助 PolyBase，SQL Server 实例可处理从外部数据源（如 Hadoop 和 Azure Blob 存储）中读取数据的 Transact-SQL 查询。
ms.date: 06/10/2019
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: 43d5d214f1720513955c27c45349da74afe3888e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473358"
---
# <a name="what-is-polybase"></a>什么是 PolyBase？

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017 || >= aps-pdw-2016 || = azure-sqldw-latest"

借助 PolyBase，SQL Server 2016 实例可处理从 Hadoop 中读取数据的 Transact-SQL 查询。 同一查询还可以访问 SQL Server 中的关系表。 借助 PolyBase，同一查询还可以联接 Hadoop 和 SQL Server 中的数据。 在 SQL Server 中，[外部表](../../t-sql/statements/create-external-table-transact-sql.md)或[外部数据源](../../t-sql/statements/create-external-data-source-transact-sql.md)提供对 Hadoop 的连接。

![PolyBase 逻辑](../../relational-databases/polybase/media/polybase-logical.png "PolyBase 逻辑")

PolyBase 将一些计算推送到 Hadoop 节点，以优化总体查询。 不过，PolyBase 外部访问不仅限于 Hadoop。 其他未结构化的非关系表也受支持，如带分隔符的文本文件。

> [!TIP]
> SQL Server 2019 为 PolyBase 引入了新的连接器，包括 SQL Server、Oracle、Teradata 和 MongoDB。 有关详细信息，请参阅 [SQL Server 2019 的 PolyBase 文档](polybase-guide.md?view=sql-server-ver15)

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

借助 PolyBase，SQL Server 实例可处理从外部数据源中读取数据的 Transact-SQL 查询。 SQL Server 2016 及更高版本可以访问 Hadoop 和 Azure Blob 存储中的外部数据。 从 SQL Server 2019 开始，现在可以使用 PolyBase 访问 [SQL Server](polybase-configure-sql-server.md)、[Oracle](polybase-configure-oracle.md)、[Teradata](polybase-configure-teradata.md) 和 [MongoDB](polybase-configure-mongodb.md) 中的外部数据。

访问外部数据的相同查询还可以定位 SQL Server 实例中的关系表。 这样可以将外部源中的数据与数据库中的高价值关系数据合并。 在 SQL Server 中，[外部表](../../t-sql/statements/create-external-table-transact-sql.md)或[外部数据源](../../t-sql/statements/create-external-data-source-transact-sql.md)提供对 Hadoop 的连接。

PolyBase 将一些计算推送到 Hadoop 节点，以优化总体查询。 不过，PolyBase 外部访问不仅限于 Hadoop。 其他未结构化的非关系表也受支持，如带分隔符的文本文件。

::: moniker-end

### <a name="supported-sql-products-and-services"></a>支持的 SQL 产品和服务

PolyBase 对以下 Microsoft SQL 产品提供这些相同功能：

- SQL Server 2016 及更高版本（仅限 Windows）
- 分析平台系统（旧称为“并行数据仓库”）
- Azure Synapse Analytics

### <a name="azure-integration"></a>Azure 集成

借助 PolyBase 的基础帮助，T-SQL 查询还可以将数据导入和导出 Azure Blob 存储。 此外，借助 PolyBase，Azure Synapse Analytics 还可以将数据导入和导出 Azure Data Lake Store 和 Azure Blob 存储。

## <a name="why-use-polybase"></a>为什么要用 PolyBase？

过去联接 SQL Server 数据与外部数据的难度更大。 具体有下列两种不方便的方法：

- 传输一半数据，这样所有数据都采用一种格式或其他格式。
- 查询两个数据源，然后编写自定义查询逻辑，以在客户端一级联接和集成数据。

PolyBase 使用 T-SQL 来联接数据，因此可避免使用这两种不方便的方法。

为了简单起见，PolyBase 不要求在 Hadoop 环境中安装其他软件。 查询外部数据所用的 T-SQL 语法也是用于查询数据库表的语法。 PolyBase 实现的所有支持操作全都以透明方式发生。 查询作者无需对 Hadoop 有任何了解。

### <a name="polybase-uses"></a>PolyBase 用法

PolyBase 支持在 SQL Server 中使用以下方案：

- **通过 SQL Server 或 PDW 查询 Hadoop 中存储的数据。** 用户将数据存储在经济高效的分布式、可扩展系统中，例如 Hadoop。 PolyBase 使得使用 T-SQL 查询数据更加容易。

- **查询存储在 Azure Blob 存储中的数据。** Azure blob 存储是一个方便存储供 Azure 服务使用的数据的位置。  PolyBase 使得使用 T-SQL 访问数据变得更加容易。

- **导入 Hadoop、Azure Blob 存储或 Azure Data Lake Store 中的数据。** 通过将 Hadoop、Azure Blob 存储或 Azure Data Lake Store 中的数据导入到关系表中，利用 Microsoft SQL 的列存储技术和分析功能的速度优势。 不需要单独的 ETL 或导入工具。

- **将数据导出到 Hadoop、Azure Blob 存储或 Azure Data Lake Store。** 将数据存档到 Hadoop、Azure Blob 存储或 Azure Data Lake Store，以获得经济高效的存储，并使数据保持联机以便于访问。

- **与 BI 工具集成** 结合使用 PolyBase 和 Microsoft 的商业智能和分析堆栈，或使用任何与 SQL Server 兼容的第三方工具。

## <a name="performance"></a>性能

- **将计算推送到 Hadoop。** 查询优化器制定基于成本的决策，以在执行此操作将提升查询性能时将计算推送到 Hadoop。  查询优化器使用外部表上的统计来制定基于成本的决策。 推送计算会创建 MapReduce 作业并利用 Hadoop 的分布计算资源。

- **缩放计算资源。** 若要提高查询性能，可以使用 SQL Server [PolyBase 横向扩展组](../../relational-databases/polybase/polybase-scale-out-groups.md)。 这使并行数据可以在 SQL Server 实例和 Hadoop 节点之间传输，并为处理外部数据添加计算资源。

<!--SQL Server 2016/2017-->
::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="next-steps"></a>后续步骤

在使用 PolyBase 之前，必须[安装 PolyBase 功能](polybase-installation.md)。 然后，请参阅以下配置指南，具体取决于你的数据源：

- [Hadoop](polybase-configure-hadoop.md)
- [Azure Blob 存储](polybase-configure-azure-blob-storage.md)

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-linux-ver15||>= sql-server-ver15"

## <a name="next-steps"></a>后续步骤

在使用 PolyBase 之前，必须[安装 PolyBase 功能](polybase-installation.md)。 然后，请参阅以下配置指南，具体取决于你的数据源：
- [Hadoop](polybase-configure-hadoop.md)
- [Azure Blob 存储](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [ODBC 泛型类型](polybase-configure-odbc-generic.md)

::: moniker-end
