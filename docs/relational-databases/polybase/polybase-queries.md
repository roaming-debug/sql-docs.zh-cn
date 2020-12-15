---
title: PolyBase 查询方案 | Microsoft Docs
description: 查看使用 SQL Server 的 PolyBase 功能的查询示例，包括 SELECT、使用本地表进行外部 JOIN、导入/导出数据和新目录视图。
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 2eec9c64a65ae0fb548944dc88a056c15e5c1e80
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97416716"
---
# <a name="polybase-query-scenarios"></a>PolyBase 查询方案

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文提供的查询示例使用 [PolyBase](../../relational-databases/polybase/polybase-guide.md) 中介绍的 SQL Server（从 2016 开始）功能。 在使用这些示例前，必须先安装和配置 PolyBase。 有关更多信息，请参阅 [PolyBase 概述](polybase-guide.md)。
  
对外部表运行 Transact-SQL 语句或使用 BI 工具来查询外部表。
  
## <a name="select-from-external-table"></a>外部表中的 SELECT  

从已定义的外部表中返回数据的简单查询。  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

包含一个谓词的简单查询。

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>带有本地表的 JOIN 外部表

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>导入数据

从 Hadoop 或 Azure 存储空间将数据导入到 SQL Server 进行永久存储。 使用 SELECT INTO 导入外部表引用的数据，以便永久存储在 SQL Server 中。 动态创建关系表，再在第二步中创建基于此表的列存储索引。

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```

## <a name="export-data"></a>导出数据

将数据从 SQL Server 导出到 Hadoop 或 Azure 存储空间。 

首先，将“允许 PolyBase 导出”的 `sp_configure` 值设置为 1，启用导出功能。 然后，创建一个指向目标目录的外部表。 如果目标目录尚不存在，CREATE EXTERNAL TABLE 语句将创建一个。 然后，使用 INSERT INTO 将数据从本地 SQL Server 表导出到外部数据源。 

SELECT 语句的结果将以指定文件格式导出到指定位置。 外部文件被命名为 *QueryID_date_time_ID.format*，其中 *ID* 是增量标识符， *format* 是导出的数据格式。 例如，其中一个文件名可能是 QID776_20160130_182739_0.orc。

> [!NOTE]
> 通过 PolyBase 将数据导出到 Hadoop 或 Azure Blob 存储时，只会导出数据，而不会导出 CREATE EXTERNAL TABLE 命令中定义的列名称（元数据）。

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>新目录视图

下面的新目录视图显示外部资源。

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 使用 `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>后续步骤  

若要了解有关故障排除的详细信息，请参阅 [PolyBase 故障排除](../../relational-databases/polybase/polybase-troubleshooting.md)。
