---
title: 查询 HDFS 数据：存储池
titleSuffix: SQL Server Big Data Clusters
description: 本教程演示如何在 SQL Server 2019 大数据群集中查询 HDFS 数据。 可以为存储池中的数据创建外部表，然后运行查询。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aa376549bf3d6430ab5967e193069d35650be76b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043516"
---
# <a name="tutorial-query-hdfs-in-a-sql-server-big-data-cluster"></a>教程：在 SQL Server 大数据群集中查询 HDFS

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 中查询 HDFS 数据。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 创建指向大数据群集中 HDFS 数据的外部表。
> * 将此数据与主实例中的高值数据联接起来。

> [!TIP]
> 如果需要，可以下载并运行本教程中的命令脚本。 有关说明，请参阅 GitHub 上的[数据虚拟化示例](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization)。

这段 7 分钟的视频将引导你逐步了解在大数据集群中查询 HDFS 数据的步骤：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Query-HDFS-data-inside-SQL-Server-big-data-cluster/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- [大数据工具](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
- [将示例数据加载到大数据群集中](tutorial-load-sample-data.md)

## <a name="create-an-external-table-to-hdfs"></a>为 HDFS 创建外部表

存储池包含存储在 HDFS 中的 CSV 文件中的 Web 点击流数据。 使用以下步骤定义可访问该文件中的数据的外部表。

1. 在 Azure Data Studio 中，连接到大数据群集的 SQL Server 主实例。 有关详细信息，请参阅[连接到 SQL Server 主实例](connect-to-big-data-cluster.md#master)。

1. 双击“服务器”窗口中的连接，以显示 SQL Server 主实例的服务器仪表板。 选择“新建查询”。

   ![SQL Server 主实例查询](./media/tutorial-query-hdfs-storage-pool/sql-server-master-instance-query.png)

1. 运行以下 Transact-SQL 命令，将上下文更改为主实例中的 Sales 数据库。

   ```sql
   USE Sales
   GO
   ```

1. 定义从 HDFS 读取的 CSV 文件格式。 按 F5 运行本语句。

   ```sql
   CREATE EXTERNAL FILE FORMAT csv_file
   WITH (
       FORMAT_TYPE = DELIMITEDTEXT,
       FORMAT_OPTIONS(
           FIELD_TERMINATOR = ',',
           STRING_DELIMITER = '"',
           FIRST_ROW = 2,
           USE_TYPE_DEFAULT = TRUE)
   );
   ```

1. 如果尚未创建存储池的外部数据源，请创建该数据源。

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   BEGIN
     CREATE EXTERNAL DATA SOURCE SqlStoragePool
     WITH (LOCATION = 'sqlhdfs://controller-svc/default');
   END
   ```

1. 创建可以从存储池读取 `/clickstream_data` 的外部表。 可以从大数据群集的主实例访问 SqlStoragePool。

   ```sql
   CREATE EXTERNAL TABLE [web_clickstreams_hdfs]
   ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
   WITH
   (
       DATA_SOURCE = SqlStoragePool,
       LOCATION = '/clickstream_data',
       FILE_FORMAT = csv_file
   );
   GO
   ```

## <a name="query-the-data"></a>查询数据

运行以下查询，将 `web_clickstream_hdfs` 外部表中的 HDFS 数据与本地 `Sales` 数据库中的关系数据联接起来。

```sql
SELECT  
    wcs_user_sk,
    SUM( CASE WHEN i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
    SUM( CASE WHEN i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
    SUM( CASE WHEN i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
    SUM( CASE WHEN i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
    SUM( CASE WHEN i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
    SUM( CASE WHEN i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
    SUM( CASE WHEN i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
    SUM( CASE WHEN i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
    SUM( CASE WHEN i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
    SUM( CASE WHEN i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
  FROM [dbo].[web_clickstreams_hdfs]
  INNER JOIN item it ON (wcs_item_sk = i_item_sk
                        AND wcs_user_sk IS NOT NULL)
GROUP BY  wcs_user_sk;
GO
```

## <a name="clean-up"></a>清除

使用以下命令删除本教程中使用的外部表。

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_hdfs];
GO
```

## <a name="next-steps"></a>后续步骤

请继续学习下一篇文章，了解如何从大数据群集查询 Oracle。
> [!div class="nextstepaction"]
> [查询 Oracle 中的外部数据](tutorial-query-oracle.md)
