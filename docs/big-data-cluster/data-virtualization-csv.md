---
title: 从存储池虚拟化 CSV 数据
subtitle: SQL Server 大数据群集
description: 详细介绍如何在大数据群集中为 CSV 文件虚拟化创建外部表的步骤
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/24/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: a524b238e980ee4b8972a4a8f7b976a34ca17c3e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97420119"
---
# <a name="virtualize-csv-data-from-storage-pool-big-data-clusters"></a>从存储池虚拟化 CSV 数据（大数据群集）

SQL Server 大数据群集可以从 HDFS 中的 CSV 文件虚拟化数据。 此过程允许将数据保留在其原始位置，但可以从 SQL Server 实例进行查询（如同任何其他表一样）。 此功能使用 PolyBase 连接器，可最大程度减少对 ETL 过程的需求。 有关数据虚拟化的详细信息，请参阅[什么是 PolyBase？](../relational-databases/polybase/polybase-guide.md)

## <a name="prerequisites"></a>先决条件

- [已部署的大数据群集](deployment-guidance.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)

## <a name="select-or-upload-a-csv-file-for-data-virtualization"></a>选择或上传 CSV 文件以进行数据虚拟化 

在 Azure Data Studio (ADS) 中，[连接到大数据群集的 SQL Server 主实例](connect-to-big-data-cluster.md#master)。 连接后，在对象资源管理器中展开 HDFS 元素，以找到要进行数据虚拟化的 CSV 文件。 

在本教程中，创建一个名为 Data  的新目录。

1. 右键单击 HDFS 根目录上下文菜单。
2. 单击“新建目录”  。
3. 将新目录命名为 Data  。

上传示例数据。 对于简单演练，可以使用示例 csv 数据文件。 本文使用来自[美国运输部](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp?pn=1)的航班延迟原因数据。 下载原始程序，然后将数据提取到计算机。 将文件命名为 airline_delay_causes.csv  。

在提取示例文件之后上传：

1. 在 Azure Data Studio 中，右键单击所创建的新目录  。 
2. 单击“上传文件”  。

![HDFS 中的示例 csv 文件](media/data-virtualization/100-csv-sample-file-hdfs.png)

Azure Data Studio 将文件上传到大数据群集上的 HDFS 中。

## <a name="create-the-storage-pool-external-data-source-in-your-target-database"></a>在目标数据库中创建存储池外部数据源

存储池外部数据源默认不会创建在大数据群集中的数据库里。 先使用以下 Transact-SQL 查询在目标数据库中创建默认 SqlStoragePool 外部数据源  ，然后才能创建外部表。 请确保首先将查询的上下文更改为目标数据库。

```sql
-- Create the default storage pool source for SQL Big Data Cluster
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
    CREATE EXTERNAL DATA SOURCE SqlStoragePool
    WITH (LOCATION = 'sqlhdfs://controller-svc/default');
```

## <a name="create-the-external-table"></a>创建外部表

在 ADS 中，右键单击 CSV 文件，然后从上下文菜单中选择“从 CSV 文件中创建外部表”  。 还可以从 HDFS 中的目录中的 CSV 文件创建外部表（如果目录下的文件遵循相同架构）。 这将允许在目录级别虚拟化数据，而无需处理单个文件，并获得组合数据的联接结果集。Azure Data Studio 会指导你完成创建外部表的步骤。

指定数据库、数据源、表名、架构和表外部文件格式的名称。

单击“下一步”。 

## <a name="preview-data"></a>预览数据

Azure Data Studio 提供导入数据的预览。

![屏幕截图显示了“从 CSV 创建外部表”窗口，其中包含导入数据的预览。](media/data-virtualization/130-csv-preview-data.png)

完成查看预览后，单击“下一步”  继续

## <a name="modify-columns"></a>修改列

在下一个窗口中，你可以修改要创建的外部表的列。 你将能够更改列名称、更改数据类型并允许可为 Null 的行。 

![屏幕截图显示了“从 CSV 创建外部表”窗口，其中显示了“步骤 3 修改列”。](media/data-virtualization/140-csv-modify-columns.png)

验证目标列之后，单击“下一步”  。

## <a name="summary"></a>总结

此步骤提供所选对象的摘要。 它提供 SQL Server 名称、数据库名称、表名、表架构和外部表信息。 在此步骤中，可以选择生成脚本或创建表。 “生成脚本”会在 T-SQL 中创建脚本以创建外部数据源  。 “创建表”会创建外部数据源。 

![“摘要”屏幕](media/data-virtualization/150-csv-virtualize-data-summary.png)

如果单击“创建表”，则 SQL Server 会在目标数据库中创建外部表。 

如果单击“生成脚本”  ，则 Azure Data Studio 会创建用于创建外部表的 T-SQL 查询。

创建后，现在可以直接使用 T-SQL 从 SQL Server 实例查询表。

## <a name="next-steps"></a>后续步骤

有关 SQL Server 大数据群集和相关方案的详细信息，请参阅[什么是 SQL Server 大数据群集？](big-data-cluster-overview.md)。
