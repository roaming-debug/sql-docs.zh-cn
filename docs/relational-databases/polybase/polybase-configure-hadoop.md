---
title: 访问外部数据：Hadoop - PolyBase
description: 本文使用 Hardoop 的 SQL Server 实例上的 PolyBase。 PolyBase 适用于外部表的临时查询和数据导入/导出。
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: c42f763957fd7245bff60293b790ab11a833c3ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480008"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>配置 PolyBase 以访问 Hadoop 中的外部数据

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 Hadoop 中的外部数据。

## <a name="prerequisites"></a>先决条件

- 如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。 这篇安装文章介绍了安装的先决条件。

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- 从 SQL Server 2019 开始，还必须[启用 PolyBase 功能](polybase-installation.md#enable)。

::: moniker-end

- PolyBase 支持两个 Hadoop 提供程序：Hortonworks 数据平台 (HDP) 和 Cloudera 分布式 Hadoop (CDH)。 Hadoop 遵循其新版本的“Major.Minor.Version”模式，且主要和次要版本中支持的所有版本均受支持。 支持以下 Hadoop 提供程序：

  - Linux 上的 Hortonworks HDP 1.3、2.1-2.6、3.0
  - Windows Server 上的 Hortonworks HDP 1.3、2.1-2.3
  - Linux 上的 Cloudera CDH 4.3、5.1 – 5.5、5.9 - 5.13

> [!NOTE]
> 从 SQL Server 2016 SP1 CU7 和 SQL Server 2017 CU3 开始，PolyBase 支持 Hadoop 加密区域。 如果使用 [PolyBase 横向扩展组](polybase-scale-out-groups.md)，所有计算节点还必须位于包含对 Hadoop 加密区域的支持的内部版本上。

### <a name="configure-hadoop-connectivity"></a>配置 Hadoop 连接

首先，配置 SQL Server PolyBase 以使用特定的 Hadoop 提供程序。

1. 使用“hadoop connectivity”运行 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 并为提供程序设置适当的值。 若要为提供程序查找值，请参阅 [PolyBase 连接配置](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md)。 默认情况下，Hadoop 连接设置为 7。

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. 必须使用  services.msc 重启 SQL Server。 重启 SQL Server 会重启这些服务：  

   - SQL Server PolyBase 数据移动服务  
   - SQL Server PolyBase 引擎  
  
   ![在 services.msc 中停止和启动 PolyBase 服务](../../relational-databases/polybase/media/polybase-stop-start.png "在 services.msc 中停止和启动 PolyBase 服务")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> 启用下推计算  

若要提高查询性能，请对 Hadoop 群集启用下推计算：  
  
1. 在 SQL Server 的安装路径中查找文件 **yarn-site.xml** 。 通常情况下，该路径为：  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. 对于 Hadoop 计算机，在 Hadoop 配置目录中查找类似文件。 在文件中，查找并复制配置密钥 yarn.application.classpath 的值。  
  
1. 对于 SQL Server 计算机，在 yarn-site.xml 文件中，查找 yarn.application.classpath 属性   。 将 Hadoop 计算机的值粘贴到值元素中。  
  
1. 对于所有 CDH 5.X 版本，都需要将 mapreduce.application.classpath 配置参数添加到 yarn-site.xml 文件的末尾或添加到 mapred-site.xml 文件中。 HortonWorks 在 yarn.application.classpath 配置中包括了这些配置。 有关示例，请参阅 [PolyBase 配置](../../relational-databases/polybase/polybase-configuration.md)。

## <a name="configure-an-external-table"></a>配置外部表

若要查询 Hadoop 数据源中的数据，必须定义外部表以在 Transact-SQL 查询中使用。 以下步骤介绍如何配置外部表。

1. 创建数据库主密钥（如果尚不存在）。 这是加密凭据密钥所必需的。

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>参数
    PASSWORD ='password'

    用于加密数据库主密钥的密码。 密码必须符合托管 SQL Server 实例的计算机的 Windows 密码策略要求。
1. 为受 Kerberos 保护的 Hadoop 群集创建数据库范围凭据。

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

3. 使用 [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md) 创建外部文件格式。

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. 使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 创建指向存储在 Hadoop 中的数据的外部表。 在此示例中，外部数据包含汽车传感器数据。

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
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

5. 在外部表上创建统计信息。

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

以下查询将数据从 SQL Server 导出到 Hadoop。 若要执行此操作，首先需要启用 PolyBase 导出。 在向目标导出数据之前，为目标创建一个外部表。

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

## <a name="view-polybase-objects-in-ssms"></a>查看 SSMS 中的 PolyBase 对象  

在 SSMS 中，外部表在单独的文件夹“外部表”  中显示。 外部数据源和外部文件格式位于“外部资源”  下的子文件夹中。  
  
![SSMS 中的 PolyBase 对象](media/polybase-management.png)  

## <a name="next-steps"></a>后续步骤

在以下文章中了解更多使用和监视 PolyBase 的方式：

[PolyBase 横向扩展组](../../relational-databases/polybase/polybase-scale-out-groups.md)。  
[PolyBase 故障排除](polybase-troubleshooting.md)。  
