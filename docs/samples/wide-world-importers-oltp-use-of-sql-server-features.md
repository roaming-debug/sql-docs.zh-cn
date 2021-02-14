---
title: 使用 SQL Server 特性和功能 |Microsoft Docs
description: 了解 SQL Server 特性和功能及其在 WideWorldImporters 示例数据库中的用法。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 01/21/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 06f89721-8478-4abc-8ada-e9c73b08bf51
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6e20d90d91badcd0961ab03e219bbf6e9a8ae2c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354063"
---
# <a name="use-of-sql-server-features-and-capabilities"></a>SQL Server 特性和功能的使用

[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

在 OLTP 数据库中 WideWorldImporters 使用 SQL Server 特性和功能。

WideWorldImporters 旨在展示 SQL Server 的许多重要功能，包括 SQL Server 2016 中引入的最新功能。 下表列出了 SQL Server 的特性和功能。 每行还提供了有关如何在 WideWorldImporters 中使用功能的说明。  
&nbsp;

|SQL Server 功能或功能|在 WideWorldImporters 中使用|
|:-------------------------------|:------------------------|
|临时表|存在许多时态表，其中包括所有查找样式引用表和主要实体，如 StockItems、客户和供应商。 使用临时表，可以方便地跟踪这些实体的历史记录。|
|针对 JSON 的 AJAX 调用|应用程序经常使用 AJAX 调用来查询这些表：人员、客户、供应商和 StockItems。 调用返回 JSON 格式的数据。 例如，请参阅存储过程 `Website.SearchForCustomers` 。|
|JSON 属性/值包|许多表的列都包含 JSON 数据，以扩展表中的关系数据。 例如， `Application.SystemParameters` 有一个应用程序设置列，并且 `Application.People` 有一列用于记录用户首选项。 这些表使用 `nvarchar(max)` 列来记录 JSON 数据，并使用内置函数 `ISJSON` 来确保列值是有效的 json。|
|行级别安全性 (RLS)|行级别安全性 (RLS) 用于基于角色成员身份限制对 Customers 表的访问。 每个销售区域都有一个角色和一个用户。 若要查看 RLS 访问限制的操作，请使用 sample-script.zip 中的相应脚本，这是此 [示例](https://go.microsoft.com/fwlink/?LinkID=800630)的一部分。|
|实时运行分析| (完整版数据库) 核心事务表 `Sales.InvoiceLines` 中，并且 `Sales.OrderLines` 两者都具有非聚集列存储索引，以在对操作工作负荷的影响最小时，支持在事务数据库中高效执行分析查询。 在同一数据库中运行事务和分析也称为 [混合事务/分析处理 (HTAP) ](https://wikipedia.org/wiki/Hybrid_Transactional/Analytical_Processing_(HTAP))。 若要查看此操作，请使用 sample-script.zip 中的相应脚本，这是此 [示例版本](https://go.microsoft.com/fwlink/?LinkID=800630)的一部分。|
|PolyBase|若要查看此 PolyBase 操作，请使用外部表以及 Azure 博客存储中托管的公共数据集，使用 sample-script.zip 中的相应脚本，这是此 [示例](https://go.microsoft.com/fwlink/?LinkID=800630)的一部分。|
|内存中 OLTP| (完整版数据库) 表类型都是内存优化的，因此，表值参数 (Tvp) 所有受益于内存优化。<br/><br/>两个监视表 `Warehouse.VehicleTemperatures` 和 `Warehouse.ColdRoomTemperatures` 都是内存优化的。 内存优化允许以比传统的基于磁盘的表更快的速度填充 ColdRoomTemperatures 表。 VehicleTemperatures 表包含 JSON 有效负载，并适合用于 IoT 方案的扩展。 VehicleTemperatures 表进一步适用于涉及 EventHubs、流分析和 Power BI 的方案。<br/><br/>存储过程 `Website.RecordColdRoomTemperatures` 是本机编译的，以进一步提高记录冷房间温度的性能。<br/><br/>若要查看操作中 In-Memory OLTP 的示例，请参阅 workload-drivers.zip 中的车辆位置工作负荷驱动程序，该驱动程序是该 [示例](https://go.microsoft.com/fwlink/?LinkID=800630)的一部分。|
|聚集列存储索引|) 表 `Warehouse.StockItemTransactions` 使用聚集列存储索引 (完整版本的数据库。 此表中的行数预计会增长很大，聚集列存储索引可以显著减少表的磁盘大小，并提高查询性能。 此表的修改是仅插入-此表在联机工作负荷中没有更新/删除，而聚集列存储索引对插入工作负荷执行良好的。|
|动态数据屏蔽|在数据库架构中，数据屏蔽已应用于表中为供应商保留的银行详细信息 `Purchasing.Suppliers` 。 非管理人员将无法访问此信息。|
|Always Encrypted|可下载的 samples.zip 中提供了 Always Encrypted 演示，该示例是此 [示例版本](https://go.microsoft.com/fwlink/?LinkID=800630)的一部分。 此演示将创建加密密钥、对敏感数据使用加密的表，以及将数据插入表中的小型示例应用程序。|
|Stretch Database|`Warehouse.ColdRoomTemperatures`该表已作为时态表实现，在完整版本的示例数据库中是内存优化的。 存档表是基于磁盘的，可以延伸到 Azure。|
|全文索引|全文索引提高了对人员、客户和 StockItems 的搜索。 仅当 SQL Server 实例上安装了全文索引时，才将索引应用于查询。 非永久性计算列用于在 StockItems 表中创建全文索引的数据。<br/><br/>`CONCAT` 用于连接字段以创建全文索引的 SearchData。<br/>若要在示例中启用全文索引，请在数据库中执行以下语句：<br/><br/>`EXECUTE [Application].[Configuration_ConfigureFullTextIndexing]`<br/><br/>此过程将创建一个默认全文目录（如果尚不存在），然后使用这些视图的全文版本替换搜索视图) 。<br/><br/>请注意，在 SQL Server 中使用全文索引需要在安装过程中选择 "Full-Text" 选项。 若要启用全文索引，Azure SQL 数据库不需要和特定的配置。|
|索引的持久化计算列|在 SupplierTransactions 和 CustomerTransactions 中使用的索引持久化计算列。|
|检查约束|中的检查约束相对比较复杂 `Sales.SpecialDeals` 。 这可确保仅配置 DiscountAmount、DiscountPercentage 和单价中的一个。|
|唯一约束|多对多构造 (和) 设置的唯一约束 `Warehouse.StockItemStockGroups` 。|
|表分区|) 表中 (完整的数据库版本 `Sales.CustomerTransactions` ，并 `Purchasing.SupplierTransactions` 使用分区函数和分区方案按年份进行分区 `PF_TransactionDate` `PS_TransactionDate` 。 分区用于提高大型表的可管理性。|
|列出处理|提供了一个示例表类型 `Website.OrderIDList` 。 它由示例过程使用 `Website.InvoiceCustomerOrders` 。 此过程使用公用表表达式 (Cte) 、TRY/CATCH、JSON_MODIFY、XACT_ABORT、NOCOUNT、THROW 和 XACT_STATE，以演示处理订单列表而不只是单个订单的功能，从而最大程度地减少从应用程序到数据库引擎的往返。|
|GZip 压缩|在 `Warehouse.VehicleTemperature` 视图中，其表包含完整的传感器数据。 但当此数据超过几个月后，就会对其进行压缩以节省空间。 Compression 函数使用 GZip 压缩。<br/><br/>视图在 `Website.VehicleTemperatures` 检索以前压缩的数据时使用解压缩函数。|
|查询存储|在数据库上启用查询存储。 运行几个查询后，请执行以下步骤：<br/><br/>1. 打开 Management Studio 中的数据库。<br/>2. 打开位于数据库下的节点查询存储。<br/>3. 打开报告 *资源消耗排名靠前的查询*。 查看查询执行，并查看刚刚运行的查询的计划。|
|STRING_SPLIT|`DeliveryInstructions`表中的列 `Sales.Invoices` 具有以逗号分隔的值，可用于演示 STRING_SPLIT。|
|审核|可以通过在数据库中运行以下语句，为此示例数据库启用 SQL Server 审核：<br/><br/>`EXECUTE [Application].[Configuration_ApplyAuditing]`<br/><br/>在 Azure SQL 数据库中，通过 [Azure 门户](https://portal.azure.com/)启用审核。<br/><br/>涉及登录名、角色和权限的安全操作记录在启用了审核的所有系统 (包括) 标准版系统。 审核会定向到应用程序日志，因为这在所有系统上都可用，无需其他权限。 系统会指出，为了提高安全性，应将它重定向到安全日志或安全文件夹中的文件。 提供了一个链接，用于描述所需的附加配置。<br/><br/>对于评估/开发人员/企业版系统，审核对所有财务事务数据的访问。|
| &nbsp; | &nbsp; |
