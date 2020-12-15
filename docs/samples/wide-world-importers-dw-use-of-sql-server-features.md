---
title: DW WideWorldImporters 数据库中的主要功能
description: 了解 WideWorldImportersDW 数据库如何展示适用于数据仓库和分析的 SQL Server 的主要功能。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 07/01/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: dea493c6c46536367d3251579d60c731bde1bb84
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97424535"
---
# <a name="wideworldimportersdw-use-of-sql-server-features-and-capabilities"></a>WideWorldImportersDW 使用 SQL Server 特性和功能
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW 旨在展示适用于数据仓库和分析的 SQL Server 的许多关键功能。 下面是 SQL Server 特性和功能的列表，以及如何在 WideWorldImportersDW 中使用这些特性和功能的说明。

## <a name="polybase"></a>PolyBase

[适用于 SQL Server (2016 及更高版本) ]

PolyBase 用于将 WideWorldImportersDW 中的销售信息与有关统计信息的公共数据集合并，以了解哪些城市可能会对销售更有兴趣。

若要在示例数据库中启用 PolyBase，请确保已安装该数据库，并在数据库中运行以下存储过程：

```sql
EXECUTE [Application].[Configuration_ApplyPolyBase]
```

这将创建一个外部表 `dbo.CityPopulationStatistics` ，该表引用包含在 Azure blob 存储中托管的美国中的城市人口数据的公共数据集。 建议您查看存储过程中的代码以了解配置过程。 如果希望在 Azure blob 存储中托管自己的数据，并将其从通用公共访问中保护，则需要执行其他配置步骤。 以下查询将返回该外部数据集中的数据：

```sql
SELECT
        CityID, StateProvinceCode, CityName,
        YearNumber, LatestRecordedPopulation
    FROM
        dbo.CityPopulationStatistics;
```

若要了解哪些城市可能需要进一步扩展，请查看以下查询，查看城市的增长率，并返回具有巨大增长的排名靠前的100城市，而宽世界导人员没有销售状态。 查询涉及远程表 `dbo.CityPopulationStatistics` 和本地表之间的联接 `Dimension.City` ，以及涉及本地表的筛选器 `Fact.Sales` 。

```sql
WITH PotentialCities
AS
(
    SELECT cps.CityName,
            cps.StateProvinceCode,
            MAX(cps.LatestRecordedPopulation) AS PopulationIn2016,
            (MAX(cps.LatestRecordedPopulation) - MIN(cps.LatestRecordedPopulation)) * 100.0
                / MIN(cps.LatestRecordedPopulation) AS GrowthRate
    FROM dbo.CityPopulationStatistics AS cps
    WHERE cps.LatestRecordedPopulation IS NOT NULL
    AND cps.LatestRecordedPopulation <> 0
    GROUP BY cps.CityName, cps.StateProvinceCode
),
InterestingCities
AS
(
    SELECT DISTINCT pc.CityName,
                    pc.StateProvinceCode,
                    pc.PopulationIn2016,
                    FLOOR(pc.GrowthRate) AS GrowthRate
    FROM PotentialCities AS pc
    INNER JOIN Dimension.City AS c
    ON pc.CityName = c.City
    WHERE GrowthRate > 2.0
    AND NOT EXISTS (SELECT 1 FROM Fact.Sale AS s WHERE s.[City Key] = c.[City Key])
)
SELECT TOP(100) CityName, StateProvinceCode, PopulationIn2016, GrowthRate
FROM InterestingCities
ORDER BY PopulationIn2016 DESC;
```

## <a name="clustered-columnstore-indexes"></a>聚集列存储索引

示例 (完整版本) 

聚集列存储索引 (CCI) 与所有事实数据表一起使用，以减少存储空间占用量和提高查询性能。 使用 CCI 后，事实数据表的基本存储使用列压缩。

非聚集索引是在聚集列存储索引的顶层使用的，用于简化 primary key 和 foreign key 约束。 这些约束已增加，但 ETL 过程从 WideWorldImporters 数据库中查找数据，而这些数据具有强制完整性的约束。 删除主键和外键约束及其支持索引会减少事实数据表的存储空间。

**数据大小**

该示例数据库的数据大小有限，以便轻松下载和安装该示例。 但是，若要查看列存储索引的实际性能优势，需要使用较大的数据集。

您可以运行以下语句以增加表的大小， `Fact.Sales` 方法是插入另一个12000000行的示例数据。 将为2012年插入这些行，以便不会干扰 ETL 进程。

```sql
    EXECUTE [Application].[Configuration_PopulateLargeSaleTable]
```

此语句的运行时间大约为5分钟。 若要插入超过12000000行，请将所需的行数作为参数传递给此存储过程。

若要将具有和不包含列存储的查询性能进行比较，可以删除和/或重新创建聚集列存储索引。

删除索引：

```sql
 DROP INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

若要重新创建：

```sql
CREATE CLUSTERED COLUMNSTORE INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

## <a name="partitioning"></a>分区

示例 (完整版本) 

数据仓库中的数据大小可能会增长得非常大。 因此，最佳做法是使用分区来管理数据库中大表的存储。

所有较大的事实数据表按年份进行分区。 唯一的例外是 `Fact.Stock Holdings` ，它不是基于日期的，并且与其他事实数据表相比，数据大小有限制。

用于所有已分区表的分区函数为 `PF_Date` ，使用的分区方案是 `PS_Date` 。

## <a name="in-memory-oltp"></a>内存中 OLTP

示例 (完整版本) 

WideWorldImportersDW 对临时表使用 SCHEMA_ONLY 内存优化表。 所有 `Integration.` * `_Staging` 表都 SCHEMA_ONLY 内存优化表。

SCHEMA_ONLY 表的优点在于，它们不会被记录，并且不需要任何磁盘访问。 这会提高 ETL 过程的性能。 由于未记录这些表，因此如果出现故障，它们的内容将会丢失。 但是，数据源仍可用，因此，如果发生故障，ETL 过程只需重新启动。
