---
title: 访问外部数据：Azure blob 存储 - PolyBase
description: 本文使用 Azure Blob 存储的 SQL Server 实例上的 PolyBase。 PolyBase 适用于外部表的临时查询和数据导入/导出。
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016 || =sqlallproducts-allversions'
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 6621d01c9cb52d528f2d3578a128f0abada22db0
ms.sourcegitcommit: 7a3fdd3f282f634f7382790841d2c2a06c917011
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96563103"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>配置 PolyBase 以访问 Azure Blob 存储中的外部数据

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 Azure Blob 存储中的外部数据。

## <a name="prerequisites"></a>先决条件

如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。 这篇安装文章介绍了安装的先决条件。

### <a name="configure-azure-blob-storage-connectivity"></a>配置 Azure Blob 存储连接

首先，配置 SQL Server PolyBase 以使用 Azure Blob 存储。

1. 运行 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)，将“hadoop 连接”设置为 Azure Blob 存储提供程序。 若要查找提供程序的值，请参阅 [PolyBase 连接配置](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md)。 默认情况下，Hadoop 连接设置为 7。

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. 使用 services.msc 重启 SQL Server。 重启 SQL Server 会重启这些服务：  

   - SQL Server PolyBase 数据移动服务  
   - SQL Server PolyBase 引擎  
  
   ![在 services.msc 中停止和启动 PolyBase 服务](../../relational-databases/polybase/media/polybase-stop-start.png "在 services.msc 中停止和启动 PolyBase 服务")  
  
## <a name="configure-an-external-table"></a>配置外部表

若要查询 Hadoop 数据源中的数据，必须定义外部表以在 Transact-SQL 查询中使用。 以下步骤介绍如何配置外部表。

1. 在数据库上创建主密钥。 需要主密钥来加密凭据机密。

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. 为 Azure Blob 存储创建数据库范围的凭据。

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. 使用 [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md) 创建外部文件格式。

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))  
   ```

1. 使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 创建指向 Azure 存储中存储的数据的外部表。 在此示例中，外部数据包含汽车传感器数据。

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = AzureStorage,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

1. 在外部表上创建统计信息。

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>PolyBase Queries

PolyBase 适用于三个函数：  
  
- 对外部表的即席查询。  
- 导入数据。  
- 导出数据。  

下面的查询提供了虚构汽车传感器数据示例。

### <a name="ad-hoc-queries"></a>即席查询  

下面的即席查询联接与 Hadoop 数据的关系。 它选择驾驶速度超过 35 mph 的客户，将 SQL Server 中存储的结构化客户数据与 Hadoop 中存储的汽车传感器数据相联接。  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>导入数据  

下面的查询将外部数据导入 SQL Server。 此示例将快速驾驶员数据导入 SQL Server 以进一步深入分析。 为提高性能，它利用列存储技术。  

```sql
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

### <a name="exporting-data"></a>导出数据  

以下查询将数据从 SQL Server 导出到 Azure Blob 存储。 首先启用 PolyBase 导出。 然后，在向目标导出数据之前，为目标创建一个外部表。

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
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
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

使用此方法执行 PolyBase 导出时，可能会创建多个文件。

## <a name="view-polybase-objects-in-ssms"></a>查看 SSMS 中的 PolyBase 对象  

在 SSMS 中，外部表在单独的文件夹“外部表” 中显示。 外部数据源和外部文件格式位于“外部资源” 下的子文件夹中。  
  
![SSMS 中的 PolyBase 对象](media/polybase-management.png)  

## <a name="next-steps"></a>后续步骤

在以下文章中了解更多使用和监视 PolyBase 的方式：

[PolyBase 横向扩展组](../../relational-databases/polybase/polybase-scale-out-groups.md)。  
[PolyBase 故障排除](polybase-troubleshooting.md)。  
