---
title: WideWorldImporters OLAP 数据库目录-SQL |Microsoft Docs
description: 了解用于 WideWorldImportersDW 数据库中的数据仓库和分析处理的架构、表和存储过程。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 4481a0c96a8f4a7f2f82e8babc5d8dce5ea97e92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077808"
---
# <a name="wideworldimportersdw-database-catalog"></a>WideWorldImportersDW 数据库目录
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW 数据库中的架构、表和存储过程的说明。 

WideWorldImportersDW 数据库用于数据仓库和分析处理。 有关销售和采购的事务数据在 WideWorldImporters 数据库中生成，并使用 **每日 ETL 过程** 加载到 WideWorldImportersDW 数据库中。

因此 WideWorldImportersDW 中的数据将反映 WideWorldImporters 中的数据，但表的组织方式有所不同。 尽管 WideWorldImporters 具有传统的规范化架构，但 WideWorldImportersDW 使用 [星型架构](https://wikipedia.org/wiki/Star_schema) 方法进行表设计。 除了事实数据表和维度表，数据库还包含一些用于 ETL 进程的临时表。

## <a name="schemas"></a>架构

不同类型的表在三个架构中进行组织。

|架构|说明|
|-----------------------------|---------------------|
|维度|维度表。|
|Fact| 事实数据表。|  
|集成|ETL 需要临时表和其他对象。|  

## <a name="tables"></a>表

下面列出了维度表和事实数据表。 集成架构中的表仅用于 ETL 进程，未列出。

### <a name="dimension-tables"></a>维度表

WideWorldImportersDW 具有以下维度表。 说明包括与 WideWorldImporters 数据库中的源表之间的关系。

|表|源表|
|-----------------------------|---------------------|
|City|`Application.Cities`, `Application.StateProvinces`, `Application.Countries`.|
|客户|`Sales.Customers`, `Sales.BuyingGroups`, `Sales.CustomerCategories`.|
|日期|新表，其中包含有关日期的信息，包括基于财政年度) 的年11月1日开始 (。|
|员工|`Application.People`.|
|StockItem|`Warehouse.StockItems`, `Warehouse.Colors`, `Warehouse.PackageType`.|
|供应商|`Purchasing.Suppliers`, `Purchasing.SupplierCategories`.|
|PaymentMethod|`Application.PaymentMethods`.|
|TransactionType|`Application.TransactionTypes`.|

### <a name="fact-tables"></a>事实数据表

WideWorldImportersDW 具有以下事实数据表。 说明包括与 WideWorldImporters 数据库中的源表的关系，以及每个事实数据表通常用于的分析/报告查询的类。

|表|源表|示例分析|
|-----------------------------|---------------------|---------------------|
|订单|`Sales.Orders` 和 `Sales.OrderLines`|销售人员、选择器/packer 生产力，以及在何时选择订单。 此外，还会出现低库存情况，从而导致订单后退。|
|Sale|`Sales.Invoices` 和 `Sales.InvoiceLines`|销售日期、交付日期、随时间推移而盈利率，按销售人员盈利。|
|购买|`Purchasing.PurchaseOrderLines`|预期与实际提前期|
|事务|`Sales.CustomerTransactions` 和 `Purchasing.SupplierTransactions`|衡量问题日期与终止日期和金额。|
|移动|`Warehouse.StockTransactions`|随着时间的推移而移动。|
|股票控股|`Warehouse.StockItemHoldings`|现有库存水平和值。|

## <a name="stored-procedures"></a>存储过程

存储过程主要用于 ETL 过程和配置目的。

鼓励使用此示例的任何扩展，以便将 `Reports` 架构用于 Reporting Services 报表和 `PowerBI` Power BI 访问的架构。

### <a name="application-schema"></a>应用程序架构

这些过程用于配置示例。 它们用于将企业版功能应用到示例的 standard edition 版本、添加 PolyBase，并为 ETL 添加种子。

|过程|目的|
|-----------------------------|---------------------|
|Configuration_ApplyPartitionedColumnstoreIndexing|对事实数据表应用分区索引和列存储索引。|
|Configuration_ConfigureForEnterpriseEdition|应用分区、列存储索引和内存中的。|
|Configuration_EnableInMemory|将集成临时表替换为 SCHEMA_ONLY 内存优化表，以提高 ETL 性能。|
|Configuration_ApplyPolyBase|配置外部数据源、文件格式和表。|
|Configuration_PopulateLargeSaleTable|应用企业版更改，然后为2012日历年填充更多的数据作为附加历史记录。|
|Configuration_ReseedETL|删除现有数据并重新启动 ETL 种子。 这允许重新填充 OLAP 数据库，以匹配 OLTP 数据库中更新的行。|

### <a name="integration-schema"></a>集成架构

ETL 过程中使用的过程分为以下几类：
- ETL 包的帮助程序过程-所有 Get * 过程。
- ETL 包用于将暂存数据迁移到 DW 表中的过程-所有迁移 * 过程。
- `PopulateDateDimensionForYear` -使用年份，并确保在表中填充该年份的所有日期 `Dimension.Date` 。

### <a name="sequences-schema"></a>序列架构

用于在数据库中配置序列的过程。

|过程|目的|
|-----------------------------|---------------------|
|ReseedAllSequences|`ReseedSequenceBeyondTableValue`为所有序列调用过程。|
|ReseedSequenceBeyondTableValue|用于将下一个序列值重定位到任何使用同一序列的表中的值之外。  (`DBCC CHECKIDENT` ，例如适用于序列但可能跨多个表的标识列。 ) |
