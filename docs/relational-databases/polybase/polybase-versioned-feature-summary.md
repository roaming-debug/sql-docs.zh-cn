---
description: PolyBase 功能和限制
title: PolyBase 功能和限制 | Microsoft Docs
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 11/13/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
ms.assetid: 6591994d-6109-4285-9c5b-ecb355f8a111
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce4c3624600741d900d51dc832ab4a685c28107a
ms.sourcegitcommit: 0f484f32709a414f05562bbaafeca9a9fc57c9ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2020
ms.locfileid: "94631743"
---
# <a name="polybase-features-and-limitations"></a>PolyBase 功能和限制

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

本文是可用于 SQL Server 产品和服务的 PolyBase 功能摘要。  
  
## <a name="feature-summary-for-product-releases"></a>产品发布的功能摘要

此表列出了 PolyBase 的主要功能以及提供这些功能的产品。  

|**功能** |**SQL Server 2016** |**Azure SQL 数据库** |**Azure Synapse Analytics** |**并行数据仓库** |
|---------|---------|---------|---------|---------|
|查询 Hadoop 数据和 [!INCLUDE[tsql](../../includes/tsql-md.md)]|是|否|否|是|
|从 Hadoop 导入数据|是|否|否|是|
|导出数据到 Hadoop  |是|否|否| 是|
|查询、导入自、导出至 Azure HDInsight |否|否|否|否
|将查询计算下推到 Hadoop|是|否|否|是|  
|从 Azure Blob 存储导入数据|是|否|是|是|
|导出数据到 Azure Blob 存储|是|否|是|是|  
|从 Azure Data Lake Store 导入数据|否|否|是|否|
|将数据导出到 Azure Data Lake Store|否|否|是|否|
|从 Microsoft BI 工具运行 PolyBase 查询|是|否|是|是|

## <a name="pushdown-computation-supported-by-t-sql-operators"></a>T-SQL 运算符支持的下推计算

在 SQL Server 和 APS 中，不是所有 T-SQL 运算符都可下推到 Hadoop 群集。 此表列出了所有支持的运算符和不支持的运算符的子集。

|**运算符类型** |可推送到 Hadoop |可推送到 Blob 存储 |
|---------|---------|---------|
|列投影|是|否|
|谓词|是|否|
|聚合|部分|否|
|外部表之间的联接|否|否|
|外部表和本地表之间的联接|否|否|
|排序|否|否|

部分聚合意味着必须在数据到达 SQL Server 后进行最终聚合。 但是一部分聚合发生在 Hadoop 中。 这是在大规模并行处理系统中计算聚合的常见方法。  

## <a name="known-limitations"></a>已知的限制

PolyBase 具有以下限制：

- 若要使用 PolyBase，必须对数据库具有 sysadmin 或 CONTROL SERVER 级别的权限。

- 在 SQL Server 中最大行大小（包括可变长度列的全长）不能超过 32 KB，在 Azure Synapse Analytics 中不能超过 1 MB。

- 从 SQL Server 或 Azure Synapse Analytics 中以 ORC 文件格式导出数据时，包含大量问文本的列可能会受到限制。 由于 Java 内存不足的错误消息，可将它们限制为最少 50 列。 若要解决此问题，请仅导出列的一个子集。

- 如果已启用 Knox，则 PolyBase 无法连接到 Hortonworks 实例。

- 如果使用事务为 true 的 Hive 表，PolyBase 将无法访问 Hive 表目录中的数据。

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 || =sqlallproducts-allversions"

- [向 SQL Server 2016 故障转移群集添加节点时，PolyBase 没有安装](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster)。

::: moniker-end

## <a name="next-steps"></a>后续步骤

有关 PolyBase 的详细信息，请参阅[什么是 PolyBase？](polybase-guide.md)。
