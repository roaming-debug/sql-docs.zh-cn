---
title: 内存中 OLTP 的示例数据库 | Microsoft Docs
description: 了解内存中 OLTP 功能及其性能优势。 此示例演示内存优化表和本机编译的存储过程。
ms.custom: ''
ms.date: 12/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: df347f9b-b950-4e3a-85f4-b9f21735eae3
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0d28550dba1a4703bab01324910436b7256597bb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643833"
---
# <a name="sample-database-for-in-memory-oltp"></a>内存中 OLTP 的示例数据库
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
## <a name="overview"></a>概述  
 本示例展示内存中 OLTP 功能。 它演示内存优化表和本机编译存储过程，可用于展示内存中 OLTP 的性能优势。  
  
> [!NOTE]  
>  若要查看关于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]的此主题，请参阅 [演示内存中 OLTP 的 AdventureWorks 扩展](./overview-and-usage-scenarios.md#in-memory-oltp-overview)。  
  
 该示例将 AdventureWorks 数据库中的五个表迁移到内存优化表，并包括销售订单处理演示工作负荷。 可以使用此演示工作负荷了解在服务器上使用内存中 OLTP 的性能优势。  
  
 在示例说明中，我们讨论将表迁移到内存中 OLTP 时进行的权衡，以说明对内存优化表（尚）不支持的功能。  
  
 此示例的文档的结构如下：  
  
-   安装示例和运行演示工作负荷的[先决条件](#Prerequisites)  
  
-   [基于 AdventureWorks 安装内存中 OLTP 示例](#InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks)的说明  
  
-   [示例表和过程的说明](#Descriptionofthesampletablesandprocedures) - 包括通过内存中 OLTP 示例添加到 AdventureWorks 的表和过程的说明，以及将一些原始 AdventureWorks 表迁移到内存优化表的注意事项  
  
-   有关[使用演示工作负荷执行性能衡量](#PerformanceMeasurementsusingtheDemoWorkload)的说明 - 包括有关安装和运行 ostress（一种用于驱动工作负荷的工具）以及运行演示工作负荷本身的说明  
  
-   [示例中的内存和磁盘空间利用率](#MemoryandDiskSpaceUtilizationintheSample)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
-   对于性能测试，服务器的规格类似于生产环境。 对于此特定示例，应至少有 16 GB 内存可供 SQL Server 使用。 有关内存中 OLTP 硬件的一般指南，请参阅下面的博客文章：[SQL Server 2014 中的内存中 OLTP 的硬件注意事项](blog-hardware-in-memory-oltp.md)

##  <a name="installing-the-in-memory-oltp-sample-based-on-adventureworks"></a><a name="InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks"></a>基于 AdventureWorks 安装内存中 OLTP 示例  
 请按照以下步骤安装示例：  
  
1.  从 [https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) 将 AdventureWorks2016CTP3.bak 和 SQLServer2016CTP3Samples.zip 下载到本地文件夹，例如“c:\temp”。  
  
2.  使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]还原数据库备份：  
  
    1.  标识数据文件的目标文件夹和文件名，例如  
  
         “h:\DATA\AdventureWorks2016CTP3_Data.mdf”  
  
    2.  标识日志文件的目标文件夹和文件名，例如  
  
         “i:\DATA\AdventureWorks2016CTP3_log.ldf”  
  
        1.  日志文件应置于与数据文件不同的驱动器上，最好是低延迟驱动器（如 SSD 或 PCIe 存储）以实现最大性能。  
  
     示例 T-SQL 脚本：  
  
    ```sql
    RESTORE DATABASE [AdventureWorks2016CTP3]   
      FROM DISK = N'C:\temp\AdventureWorks2016CTP3.bak'   
        WITH FILE = 1,    
      MOVE N'AdventureWorks2016_Data' TO N'h:\DATA\AdventureWorks2016CTP3_Data.mdf',    
      MOVE N'AdventureWorks2016_Log' TO N'i:\DATA\AdventureWorks2016CTP3_log.ldf',  
      MOVE N'AdventureWorks2016CTP3_mod' TO N'h:\data\AdventureWorks2016CTP3_mod'  
     GO  
    ```  
  
3.  若要查看示例脚本和工作负荷，请将文件 SQLServer2016CTP3Samples.zip 解压到本地文件夹。 有关运行工作负荷的说明，请查阅 In-Memory OLTP\readme.txt。  
  
##  <a name="description-of-the-sample-tables-and-procedures"></a><a name="Descriptionofthesampletablesandprocedures"></a> 示例表和过程的说明  
 示例基于 AdventureWorks 中的现有表为产品和销售订单创建新表。 新表的架构类似于现有表，不过有几个区别，如下所述。  
  
 新的内存优化表带有后缀“_inmem”。 示例还包括带有后缀“_ondisk”的对应表 - 这些表可以用于在系统上进行内存优化表与基于磁盘的表性能之间的一对一比较。  
  
 性能比较工作负荷中使用的内存优化表具有完全持久性并且会完整记录。 这些表不会牺牲持久性或可靠性来获取性能提升。  
  
 此示例的目标工作负荷是销售订单处理，我们还考虑在其中包含产品和折扣信息。 为此，创建了表 SalesOrderHeader、SalesOrderDetail、Product、SpecialOffer 和 SpecialOfferProduct。  
  
 两个新存储过程 Sales.usp_InsertSalesOrder_inmem 和 Sales.usp_UpdateSalesOrderShipInfo_inmem 用于插入销售订单和更新给定销售订单的发货信息。  
  
 新架构“Demo”包含用于执行演示工作负荷的帮助器表和存储过程。  
  
 具体而言，内存中 OLTP 示例向 AdventureWorks 添加以下对象：  
  
### <a name="tables-added-by-the-sample"></a>示例添加的表  
  
#### <a name="the-new-tables"></a>新表  
 Sales.SalesOrderHeader_inmem  
  
-   有关销售订单的表头信息。 每个销售订单都在此表中有对应的一行。  
  
 Sales.SalesOrderDetail_inmem  
  
-   销售订单的详细信息。 销售订单的每个行项都在此表中有对应的一行。  
  
 Sales.SpecialOffer_inmem  
  
-   有关特价商品的信息，包括与每个特价商品关联的折扣百分比。  
  
 Sales.SpecialOfferProduct_inmem  
  
-   特价商品与产品之间的引用表。 每个特价商品可以包含零个或多个产品，每个产品可以包含在零个或多个特价商品中。  
  
 Production.Product_inmem  
  
-   有关产品的信息，包括其标价。  
  
 Demo.DemoSalesOrderDetailSeed  
  
-   在演示工作负荷中用于构造示例销售订单。  
  
 表的基于磁盘的变体：  
  
-   Sales.SalesOrderHeader_ondisk  
  
-   Sales.SalesOrderDetail_ondisk  
  
-   Sales.SpecialOffer_ondisk  
  
-   Sales.SpecialOfferProduct_ondisk  
  
-   Production.Product_ondisk  
  
#### <a name="differences-between-original-disk-based-and-the-and-new-memory-optimized-tables"></a>原始基于磁盘的表与新内存优化表之间的区别  
 在很大程度上，此示例引入的新表使用的列和数据类型与原始表相同。 但二者之间存在一些区别。 下面列出了区别以及更改的理由。  
  
 Sales.SalesOrderHeader_inmem  
  
-   内存优化表支持 *默认约束* ，大多数默认约束按原样迁移。 但是，原始表 Sales.SalesOrderHeader 包含两个为列 OrderDate 和 ModifiedDate 检索当前日期的默认约束。 在具有大量并发的高吞吐量订单处理工作负荷中，任何全局资源都可能成为争用点。 系统时间就是全局资源，我们观察到，它可能在运行插入销售订单的内存中 OLTP 工作负荷时成为瓶颈，特别是如果需要为销售订单表头中的多列以及销售订单详细信息检索系统时间。 此示例中解决问题的方式是仅为插入的每个销售订单检索系统时间一次，然后将该值用于 SalesOrderHeader_inmem 和 SalesOrderDetail_inmem 中的 datetime 列以及存储过程 Sales.usp_InsertSalesOrder_inmem。  
  
-   别名用户定义数据类型 (UDT) - 原始表将两个别名 UDT dbo.OrderNumber 和 dbo.AccountNumber 分别用于列 PurchaseOrderNumber 和 AccountNumber。 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 在内存优化表中不支持别名 UDT，因而新表分别使用系统数据类型 nvarchar(25) 和 nvarchar(15)。  
  
-   *索引键中可以为 Null 的列* - 在原始表中，列 SalesPersonID 可以为 Null，而在新表中，该列不可为 Null，并且默认约束值为 -1。 这种情况是因为内存优化表中的索引不能在索引键中具有可为 null 的列；-1 是这种情况下 NULL 的代理项。  
  
-   计算列 - 省略了计算列 SalesOrderNumber 和 TotalDue，因为 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 在内存优化表中不支持计算列。 新视图 Sales.vSalesOrderHeader_extended_inmem 反映了列 SalesOrderNumber and TotalDue。 因此，如果需要这些列，可以使用此视图。  

    - **适用于：** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]CTP 1.1。  
从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 开始，内存优化表和索引中支持计算列。

  
-   *中的内存优化表支持“外键约束”* [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]，前提是引用的表也是经过内存优化的。 引用也已迁移到内存优化的表的外键将保留在已迁移的表中，而其他外键则被忽略。  此外，SalesOrderHeader_inmem 是示例工作负荷中的热表，并且外键约束需要对所有 DML 操作进行附加处理，因为它需要在这些约束引用的所有其他表中进行查找。 因此，假设应用可确保 Sales.SalesOrderHeader_inmem 表的引用完整性，从而在插入行时不验证引用完整性。  
  
-   *Rowguid* - 省略了 rowguid 列。 虽然内存优化表支持 uniqueidentifier，但是 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]中不支持选项 ROWGUIDCOL。 这种列通常用于合并复制或具有文件流列的表。 此示例不包括其中任何一种。  
  
 Sales.SalesOrderDetail  
  
-   *默认约束* - 类似于 SalesOrderHeader，未迁移需要系统时间/时间的默认约束，而是由插入销售订单的存储过程负责在第一次插入时插入当前系统日期/时间。  
  
-   *计算列* - 在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中，内存优化表不支持未作为计算列迁移的计算列 LineTotal。 要访问此列，请使用视图 Sales.vSalesOrderDetail_extended_inmem。  
  
-   *Rowguid* - 省略了 rowguid 列。 有关详细信息，请参见表 SalesOrderHeader 的说明。  
  
 Production.Product  
  
-   *别名 UDT* - 原始表使用等效于系统数据类型位的用户定义数据类型 dbo.Flag。 迁移的表改用位数据类型。  
  
-   *Rowguid* - 省略了 rowguid 列。 有关详细信息，请参见表 SalesOrderHeader 的说明。  
  
 Sales.SpecialOffer  
  
-   *Rowguid* - 省略了 rowguid 列。 有关详细信息，请参见表 SalesOrderHeader 的说明。  
  
 Sales.SpecialOfferProduct  
  
-   *Rowguid* - 省略了 rowguid 列。 有关详细信息，请参见表 SalesOrderHeader 的说明。  
  
#### <a name="considerations-for-indexes-on-memory-optimized-tables"></a>有关内存优化表的索引的注意事项  
 内存优化表的基线索引是 NONCLUSTERED 索引，该索引支持点查找（采用相等谓词的索引查找）、范围扫描（采用不相等谓词的索引查找）、全文索引扫描和有序扫描。 此外，NONCLUSTERED 索引还支持对索引键的前导列进行搜索。 事实上，内存优化 NONCLUSTERED 索引支持基于磁盘的 NONCLUSTERED 索引支持的所有操作，唯一的例外是向后扫描。 因此，使用 NONCLUSTERED 索引是针对索引的安全选择。  
  
 HASH 索引可用于进一步优化工作负荷。 这些索引特别针对点查找和行插入进行了优化。 但是，必须考虑到这些索引不支持范围扫描、有序扫描或是对前导索引键列进行的搜索。 因此，使用这些索引时需要谨慎。 此外，还需要在创建时指定 bucket_count。 它通常应设置为索引键值的一到二倍之间，不过估计过高通常不是什么问题。  
  
有关详细信息，请参阅联机丛书以详细了解[索引指南](/sql/relational-databases/indexes/guidelines-for-online-index-operations)和[选择正确 bucket_count](https://sqlserver-help.com/tag/bucket_count/) 的指南。  

联机丛书提供有关以下主题的详细信息：
- [索引指南](/sql/relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables) <!-- On MSDN-TechNet was version sql.120 (2014), library/dn133166 -->

 迁移的表中的索引针对演示销售订单处理工作负荷进行了优化。 该工作负荷依赖于表 Sales.SalesOrderHeader_inmem 和 Sales.SalesOrderDetail_inmem 中的插入和点查找，还依赖于对表 Production.Product_inmem 和 Sales.SpecialOffer_inmem 中的主键列进行的点查找。  
  
 Sales.SalesOrderHeader_inmem 有三个索引，因为性能原因，而且工作负荷无需有序或范围扫描，所以这些索引都是 HASH 索引。  
  
-   (SalesOrderID) 上的 HASH 索引：bucket_count 大小为 1000 万（向上舍入为 1600 万），因为预期销售订单数为 1000 万  
  
-   (SalesPersonID) 上的 HASH 索引：bucket_count 为 1000 万。 提供的数据集不包含许多销售人员。 但此大型 bucket_count 考虑了将来的增长。 此外，如果 bucket_count 过大，也不会造成点查找性能下降。  
  
-   (CustomerID) 上的 HASH 索引：bucket_count 为 1 万。 提供的数据集不包含许多客户，但是允许将来增加。  
  
 Sales.SalesOrderDetail_inmem 有三个索引，因为性能原因，而且工作负荷无需有序或范围扫描，所以这些索引都是 HASH 索引。  
  
-   (SalesOrderID, SalesOrderDetailID) 上的 HASH 索引：这是主键索引，即使不会经常对 (SalesOrderID, SalesOrderDetailID) 进行查找，将哈希索引用于该键也可加快行插入速度。 bucket_count 大小为 5000 万（向上舍入为 6700 万）：预期销售订单数为 1000 万，这是针对平均每个订单五个项目确定的大小  
  
-   (SalesOrderID) 上的 HASH 索引：经常按销售订单进行查找：您要查找对应于单个订单的所有行项。  bucket_count 大小为 1000 万（向上舍入为 1600 万），因为预期销售订单数为 1000 万  
  
-   (ProductID) 上的 HASH 索引：bucket_count 为 1 万。 提供的数据集不包含许多产品，但是允许将来增加。  
  
 Production.Product_inmem 有三个索引  
  
-   (ProductID) 上的 HASH 索引：对 ProductID 进行的查找是演示工作负荷的关键因素，因此这是哈希索引  
  
-   (Name) 上的 NONCLUSTERED 索引：这允许进行产品名称的有序扫描  
  
-   (ProductNumber) 上的 NONCLUSTERED 索引：这允许进行产品编号的有序扫描  
  
 Sales.SpecialOffer_inmem 在 (SpecialOfferID) 上有一个 HASH 索引：对特价商品进行的点查找是演示工作负荷的关键因素。 bucket_count 大小为 100 万，允许将来增加。  
  
 Sales.SpecialOfferProduct_inmem 未在演示工作负荷中进行引用，因而表面上无需使用此表中的哈希索引优化工作负荷 - (SpecialOfferID, ProductID) 和 (ProductID) 上的索引是 NONCLUSTERED。  
  
 请注意，以上某些 bucket_count 过大，但是 SalesOrderHeader_inmem 和 SalesOrderDetail_inmem 上的索引的 bucket_count 并非如此：其大小仅用于 1000 万个销售订单。 这样做是为了可以在内存可用性较低的系统上安装示例，不过在这些情况下，演示工作负荷会由于内存不足而失败。 如果的确要扩展为远远超过 1000 万个销售订单，可以随意相应地提高桶计数。  
  
#### <a name="considerations-for-memory-utilization"></a>内存利用率注意事项  
 示例数据库中的内存利用率（运行演示工作负荷之前以及之后）在 [内存优化表的内存利用率](#Memoryutilizationforthememory-optimizedtables)一节中进行了讨论。  
  
### <a name="stored-procedures-added-by-the-sample"></a>示例添加的存储过程  
 用于插入销售订单和更新发货详细信息的两个重要存储过程如下：  
  
-   Sales.usp_InsertSalesOrder_inmem  
  
    -   在数据库中插入新销售订单并输出该销售订单的 SalesOrderID。 它采用销售订单表头的详细信息以及订单中的行项作为输入参数。  
  
    -   输出参数：  
  
        -   @SalesOrderID int - 刚插入的销售订单的 SalesOrderID  
  
    -   输入参数（必需）：  
  
        -   @DueDate datetime2  
  
        -   @CustomerID int  
  
        -   @BillToAddressID [int]  
  
        -   @ShipToAddressID [int]  
  
        -   @ShipMethodID [int]  
  
        -   @SalesOrderDetails Sales.SalesOrderDetailType_inmem - 包含订单行项目的表值参数 (TVP)  
  
    -   输入参数（可选）：  
  
        -   @Status [tinyint]  
  
        -   @OnlineOrderFlag [bit]  
  
        -   @PurchaseOrderNumber [nvarchar](25\)  
  
        -   @AccountNumber [nvarchar](15\)  
  
        -   @SalesPersonID [int]  
  
        -   @TerritoryID [int]  
  
        -   @CreditCardID [int]  
  
        -   @CreditCardApprovalCode [varchar](15\)  
  
        -   @CurrencyRateID [int]  
  
        -   @Comment nvarchar(128)  
  
-   Sales.usp_UpdateSalesOrderShipInfo_inmem  
  
    -   更新给定销售订单的发货信息。 这也会更新销售订单所有行项的发货信息。  
  
    -   这是本机编译的存储过程 Sales.usp_UpdateSalesOrderShipInfo_native 的包装过程，其中的重试逻辑用于处理与更新该订单的并发事务形成的（意外）潜在冲突。 有关重试逻辑的更多信息，请参见 [此处](./transactions-with-memory-optimized-tables.md)的联机丛书主题。  
  
-   Sales.usp_UpdateSalesOrderShipInfo_native  
  
    -   这是实际处理发货信息更新的本机编译的存储过程。 它可以从包装存储过程 Sales.usp_UpdateSalesOrderShipInfo_inmem 进行调用。 如果客户端可以处理失败并实现重试逻辑，则您可以直接调用此过程，而不是使用包装存储过程。  
  
 以下存储过程用于演示工作负荷。  
  
-   Demo.usp_DemoReset  
  
    -   通过清空 SalesOrderHeader 和 SalesOrderDetail 表并对这两个表重设种子来重置演示。  
  
 以下存储过程用于对内存优化表进行插入和删除，同时保证域和引用完整性。  
  
-   Production.usp_InsertProduct_inmem  
  
-   Production.usp_DeleteProduct_inmem  
  
-   Sales.usp_InsertSpecialOffer_inmem  
  
-   Sales.usp_DeleteSpecialOffer_inmem  
  
-   Sales.usp_InsertSpecialOfferProduct_inmem  
  
 最后，以下存储过程用于验证域和引用完整性。  
  
1.  dbo.usp_ValidateIntegrity  
  
    -   可选参数：@object_id - 要验证其完整性的对象的 ID  
  
    -   此过程依赖于用于需要验证的完整性规则的表 dbo.DomainIntegrity、dbo.ReferentialIntegrity 和 dbo.UniqueIntegrity - 示例基于对 AdventureWorks 数据库中的原始表存在的检查、外键和唯一约束来填充这些表。  
  
    -   它依赖于帮助器过程 dbo.usp_GenerateCKCheck、dbo.usp_GenerateFKCheck 和 dbo.GenerateUQCheck 来生成执行完整性检查所需的 T-SQL。  
  
##  <a name="performance-measurements-using-the-demo-workload"></a><a name="PerformanceMeasurementsusingtheDemoWorkload"></a> 使用演示工作负荷执行度量  
 Ostress 是 Microsoft CSS SQL Server 支持团队开发的命令行工具。 此工具可以用于并行执行查询或运行存储过程。 可以配置线程数来并行运行给定 T-SQL 语句，并且可以指定应对此线程执行语句的次数；Ostress 会加快线程，对所有线程并行执行语句。 对所有线程完成执行之后，Ostress 会报告所有线程完成执行所花费的时间。  
  
### <a name="installing-ostress"></a>安装 Ostress  
 ostress 作为报告标记语言 (RML) 实用工具的一部分进行安装；ostress 没有独立安装版本。  
  
 安装步骤：  
  
1.  从以下页面下载并运行 RML 实用工具的 x64 安装包：[下载用于 SQL Server 的 RML](https://www.microsoft.com/download/details.aspx?id=4511)

2.  如果出现指示某些文件正在使用的对话框，请单击“继续”  
  
### <a name="running-ostress"></a>运行 Ostress  
 Ostress 从命令行提示符运行。 从“RML 命令提示符”（作为 RML 实用工具的一部分安装）运行该工具是最方便的。  
  
 若要打开 RML 命令提示符，请按照以下说明执行：  
  
 在 Windows Server 2012 [R2] 以及 Windows 8 和 8.1 中，通过单击 Windows 键打开“开始”菜单，然后键入“rml”。 单击“RML 命令提示符”（处于搜索结果列表中）。  
  
 确保命令提示符位于 RML 实用工具安装文件夹中。  
  
 在不使用任何命令行选项的情况下仅运行 ostress.exe 时，可以查看 Ostress 的命令行选项。 对此示例运行 Ostress 时要考虑的主要选项有：  
  
-   -S：要连接到的 Microsoft SQL Server 实例的名称  
  
-   -E：使用 Windows 身份验证连接（默认）；如果使用 SQL Server 身份验证，请使用选项 -U 和 -P 分别指定用户名和密码  
  
-   -d 数据库的名称，对于此示例为 AdventureWorks2014  
  
-   -Q 要执行的 T-SQL 语句  
  
-   -n 处理每个输入文件/查询的连接数  
  
-   -r 每个连接执行每个输入文件/查询的迭代数  
  
### <a name="demo-workload"></a>演示工作负荷  
 演示工作负荷中使用的主要存储过程是 Sales.usp_InsertSalesOrder_inmem/ondisk。 下面的脚本使用示例数据构造一个表值参数 (TVP)，并调用该过程以插入包含五个行项目的销售订单。  
  
 Ostress 工具用于并行执行存储过程调用，以模拟并发插入销售订单的客户端。  
  
 在每次执行 Demo.usp_DemoReset 的压力运行之后重置演示。 此过程删除内存优化表中的行、截断基于磁盘的表并执行数据库检查点。  
  
 以下脚本并发执行，以模拟销售订单处理工作负荷：  
  
```sql
DECLARE   
      @i int = 0,   
      @od Sales.SalesOrderDetailType_inmem,   
      @SalesOrderID int,   
      @DueDate datetime2 = sysdatetime(),   
      @CustomerID int = rand() * 8000,   
      @BillToAddressID int = rand() * 10000,   
      @ShipToAddressID int = rand() * 10000,   
      @ShipMethodID int = (rand() * 5) + 1;   
  
INSERT INTO @od   
SELECT OrderQty, ProductID, SpecialOfferID   
FROM Demo.DemoSalesOrderDetailSeed   
WHERE OrderID= cast((rand()*106) + 1 as int);   
  
WHILE (@i < 20)   
BEGIN;   
      EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od;   
      SET @i += 1   
END
```  
  
 借助此脚本，构造的每个示例订单会通过在 WHILE 循环中执行的 20 个存储过程插入 20 次。 因为数据库用于构造示例订单，所以使用该循环。 在典型生产环境中，中间层应用程序将构造要插入的销售订单。  
  
 上面的脚本将销售订单插入内存优化表中。 用于将销售订单插入基于磁盘的表中的脚本通过将两个“_inmem”替换为“_ondisk”而派生。  
  
 我们将使用 Ostress 工具通过几个并发连接执行脚本。 我们将使用参数“-n”控制连接数，使用参数“r”控制对每个连接执行脚本的次数。  
  
#### <a name="running-the-workload"></a>运行工作负荷  
 为了进行大规模测试，我们使用 100 个连接插入 1000 万个销售订单。 此测试适合在中等服务器（例如，8 个物理核心、16 个逻辑核心以及用于日志的基本 SSD 存储）上执行。 如果该测试在你的硬件上性能不佳，请查看[解决测试运行缓慢的问题](#Troubleshootingslow-runningtests)一节。如果要降低此测试的压力级别，请通过更改参数“-n”减少连接数。 例如，若要将连接计数减少为 40，请将参数“-n100”更改为“-n40”。  
  
 作为工作负荷的性能度量，我们使用 ostress.exe 在运行工作负荷之后报告的占用时间。  
  
 在以下说明和测量中，使用了一个插入 1000 万个销售订单的工作负荷。 有关如何运行插入 100 万个销售订单的缩减工作负荷的说明，请参阅作为 SQLServer2016CTP3Samples.zip 存档一部分的“In-Memory OLTP\readme.txt”中的说明。  
  
##### <a name="memory-optimized-tables"></a>内存优化表  
 我们首先对内存优化表运行工作负荷。 以下命令打开 100 个线程，每个线程运行 5,000 次迭代。  每次迭代在单独事务中插入 20 个销售订单。 每次迭代进行 20 个插入，对数据库用于生成待插入数据进行补偿。 这会生成总共 20 \* 5,000 \* 100 = 10,000,000 个销售订单插入。  
  
 打开 RML 命令提示符，执行以下命令：  
  
 单击“复制”按钮复制该命令，将其粘贴到 RML 实用工具命令提示符处。  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 在一台共有 8 个物理（16 个逻辑）核心的测试服务器上，需要 2 分 5 秒。 在另一台有 24 个物理（48 个逻辑）核心的测试服务器上，需要 1 分 0 秒。  
  
 在工作负荷运行期间观察 CPU 利用率（例如使用任务管理器）。 可以看到 CPU 利用率接近 100%。 如果不是这样，则存在日志 IO 瓶颈，另请参见 [运行缓慢的测试的故障排除](#Troubleshootingslow-runningtests)。  
  
##### <a name="disk-based-tables"></a>基于磁盘的表  
 以下命令对基于磁盘的表运行工作负荷。 此工作负荷可能需要一些时间来执行，这在很大程度上是由于系统中存在闩锁争用。 内存优化表没有闩锁，因而不会遇到这个问题。  
  
 打开 RML 命令提示符，执行以下命令：  
  
 单击“复制”按钮复制该命令，将其粘贴到 RML 实用工具命令提示符处。  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_ondisk, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_ondisk @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 在一台共有 8 个物理（16 个逻辑）核心的测试服务器上，需要 41 分 25 秒。 在另一台有 24 个物理（48 个逻辑）核心的测试服务器上，需要 52 分 16 秒。  
  
 此测试中内存优化表与基于磁盘的表之间存在性能差异的主要因素在于，使用基于磁盘的表时，SQL Server 无法完全利用 CPU。 原因在于闩锁争用：并发事务尝试写入相同数据页；闩锁用于确保一次只有一个事务才能写入页。 内存中 OLTP 引擎没有闩锁，数据行不是按页组织。 因而，并发事务不阻止相互的插入，使 SQL Server 可以完全利用 CPU。  
  
 在工作负荷运行期间可以观察 CPU 利用率（例如使用任务管理器）。 对于基于磁盘的表，可以看到 CPU 利用率远低于 100%。 在有 16 个逻辑处理器的测试配置中，利用率保持在 24% 左右。  
  
 或者，可以使用性能监视器通过性能计数器“\SQL Server:Latches\Latch Waits/sec”查看每秒的闩锁等待数。  
  
#### <a name="resetting-the-demo"></a>重置演示  
 若要重置演示，请打开 RML 命令提示符，执行以下命令：  
  
```console
ostress.exe -S. -E -dAdventureWorks2016CTP3 -Q"EXEC Demo.usp_DemoReset"  
```  
  
 根据硬件情况，可能需要几分钟来运行。  
  
 我们建议在每次演示运行之后重置。 因为此工作负荷仅涉及插入，所以每次运行将占用更多内存，因而需要重置来防止内存不足。 运行之后占用的内存量在 [运行工作负荷之后的内存利用率](#Memoryutilizationafterrunningtheworkload)一节中进行了讨论。  
  
###  <a name="troubleshooting-slow-running-tests"></a><a name="Troubleshootingslow-runningtests"></a> 解决测试运行缓慢的问题  
 测试结果通常因硬件而异，也因测试运行中使用的并发级别而异。 结果与预期不符时要了解的几个方面有：  
  
-   并发事务数：对单个线程运行工作负荷时，通过内存中 OLTP 获得的性能提升可能小于 2 倍。 如果并发级别较高，则闩锁争用是唯一的大问题。  
  
-   可供 SQL Server 使用的核心数较少：这意味着系统中的并发级别较低，因为并发执行事务的数量不能超过可供 SQL 使用的核心数。  
  
    -   症状：如果对基于磁盘的表运行工作负荷时 CPU 利用率较高，则意味着不存在很多争用，说明缺乏并发性。  
  
-   日志驱动器的速度：如果日志驱动器无法跟上系统中的事务吞吐量级别，则工作负荷会在日志 IO 方面遇到瓶颈。 虽然日志记录对于内存中 OLTP 更加高效，但是如果日志 IO 是瓶颈，则性能提升的可能性有限。  
  
    -   症状：如果对内存优化表运行工作负荷时 CPU 利用率不接近于 100% 或是非常尖峰，则可能存在日志 IO 瓶颈。 这可以通过打开资源监视器查看日志驱动器的队列长度来进行确认。  
  
##  <a name="memory-and-disk-space-utilization-in-the-sample"></a><a name="MemoryandDiskSpaceUtilizationintheSample"></a> 示例中的内存和磁盘空间利用率  
 下面我们讨论示例数据库的内存和磁盘空间利用率方面的情况。 我们还会介绍在有 16 个逻辑核心的测试服务器上看到的结果。  
  
###  <a name="memory-utilization-for-the-memory-optimized-tables"></a><a name="Memoryutilizationforthememory-optimizedtables"></a> 内存优化表的内存利用率  
  
#### <a name="overall-utilization-of-the-database"></a>数据库的总体利用率  
 以下查询可用于获取系统中内存中 OLTP 的总体内存利用率。  
  
```sql
SELECT type  
   , name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
 刚刚创建数据库之后的快照：  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|默认|94|  
|MEMORYCLERK_XTP|DB_ID_5|877|  
|MEMORYCLERK_XTP|默认|0|  
|MEMORYCLERK_XTP|默认|0|  
  
 默认内存分配器包含系统范围内存结构，相对较小。 用户数据库（在此例中是 ID 为 5 的数据库）的内存分配器大约为 900 MB。  
  
#### <a name="memory-utilization-per-table"></a>每个表的内存利用率  
 以下查询可用于深化到各个表及其索引的内存利用率：  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
 下表显示此查询对于全新示例安装的结果：  
  
| 表名称 | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SpecialOfferProduct_inmem|64|3840|  
|DemoSalesOrderHeaderSeed|1984|5504|  
|SalesOrderDetail_inmem|15316|663552|  
|DemoSalesOrderDetailSeed|64|10432|  
|SpecialOffer_inmem|3|8192|  
|SalesOrderHeader_inmem|7168|147456|  
|Product_inmem|124|12352|  

 可以看到，这些表相当小：SalesOrderHeader_inmem 大约为 7 MB，SalesOrderDetail_inmem 大约为 15 MB。  
  
 此处比较显著的是为索引分配的内存大小（与表数据大小相比）。 这是因为示例中的哈希索引针对较大数据大小预设了大小。 请注意，哈希索引有固定大小，因而其大小不会随表中的数据大小而增大。  
  
####  <a name="memory-utilization-after-running-the-workload"></a><a name="Memoryutilizationafterrunningtheworkload"></a> 运行工作负荷之后的内存利用率  
 插入 1000 万个销售订单之后，总体内存利用率类似于下面这样：  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|默认|146|  
|MEMORYCLERK_XTP|DB_ID_5|7374|  
|MEMORYCLERK_XTP|默认|0|  
|MEMORYCLERK_XTP|默认|0|  

 可以看到，SQL Server 将稍小于 8 GB 的大小用于示例数据库中的内存优化表和索引。  
  
 在一次示例运行之后查看每个表的详细内存使用率：  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
| 表名称 | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SalesOrderDetail_inmem|5113761|663552|  
|DemoSalesOrderDetailSeed|64|10368|  
|SpecialOffer_inmem|2|8192|  
|SalesOrderHeader_inmem|1575679|147456|  
|Product_inmem|111|12032|  
|SpecialOfferProduct_inmem|64|3712|  
|DemoSalesOrderHeaderSeed|1984|5504|  

 可以看到共有约 6.5 GB 的数据。 请注意，表 SalesOrderHeader_inmem 和 SalesOrderDetail_inmem 上的索引大小与插入销售订单之前的索引大小相同。 索引大小未更改是因为这两个表都使用哈希索引，而哈希索引是静态的。  
  
#### <a name="after-demo-reset"></a>演示重置之后  
 存储过程 Demo.usp_DemoReset 可以用于重置演示。 它删除表 SalesOrderHeader_inmem 和 SalesOrderDetail_inmem 中的数据，并对原始表 SalesOrderHeader 和 SalesOrderDetail 中的数据重设种子。  
  
 现在，即使表中的行已删除，这也不意味着会立即回收内存。 SQL Server 根据需要在后台从内存优化表中的已删除行回收内存。 可以看到，在演示重置之后（系统上没有事务工作负荷），尚未立即回收已删除行的内存：  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|默认|2261|  
|MEMORYCLERK_XTP|DB_ID_5|7396|  
|MEMORYCLERK_XTP|默认|0|  
|MEMORYCLERK_XTP|默认|0|  

 这是预期行为：事务工作负荷运行时将回收内存。  
  
 如果启动演示工作负荷的第二次运行，可以看到内存利用率在开始时下降，因为以前删除的行会进行清理。 在某一时刻，内存大小会再次增大，直至工作负荷完成。 演示重置之后插入 1000 万行之后，内存利用率非常类似于第一次运行之后的利用率。 例如：  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|默认|1863|  
|MEMORYCLERK_XTP|DB_ID_5|7390|  
|MEMORYCLERK_XTP|默认|0|  
|MEMORYCLERK_XTP|默认|0|  

### <a name="disk-utilization-for-memory-optimized-tables"></a>内存优化表的磁盘利用率  
 可以使用查询获得数据库检查点文件在给定时间的总体磁盘上大小：  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
  
```  
  
#### <a name="initial-state"></a>初始状态  
 最初创建示例文件组和示例内存优化表时，会预先创建一些检查点文件，并且系统开始填充这些文件 - 预先创建的检查点文件数取决于系统中的逻辑处理器数。 因为示例最初非常小，所以预先创建的文件在初始创建之后大部分为空。  
  
 以下代码是示例在有 16 个逻辑处理器的计算机上的初始磁盘上大小：  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| 磁盘上大小（以 MB 为单位） |
| ------------------ |
|2312|  

 可以看到，检查点文件的磁盘上大小 (2.3 GB) 与实际数据大小（接近 30 MB）之间存在巨大差异。  
  
 可以使用以下查询更详细地查看磁盘空间利用率的来源。 此查询返回的磁盘上大小与处于状态 5 (REQUIRED FOR BACKUP/HA)、6 (IN TRANSITION TO TOMBSTONE) 或 7 (TOMBSTONE) 的文件的大小接近。  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
 对于示例的初始状态，结果类似于下面这样（对于有 16 个逻辑处理器的服务器）：  
  
| state_desc | file_type_desc | count | 磁盘上大小 (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 可以看到，大部分空间由预先创建的数据和差异文件使用。 SQL Server 为每个逻辑处理器预先创建一对（数据、差异）文件。 此外，数据文件的预设大小为 128 MB，差异文件的预设大小为 8 MB，以便更高效地向这些文件插入数据。  
  
 内存优化表中的实际数据在单个数据文件中。  
  
#### <a name="after-running-the-workload"></a>运行工作负荷之后  
 进行插入 1000 万个销售订单的单次测试运行之后，总体磁盘上大小类似于下面这样（对于 16 核测试服务器）：  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| 磁盘上大小（以 MB 为单位） |
| ------------------ |
|8828|
  
 磁盘上大小接近 9 GB，这接近于数据的内存中大小。  
  
 更详细地查看各种状态间的检查点文件大小：  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | 磁盘上大小 (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 我们仍有 16 对预先创建的文件，准备好在关闭检查点时执行。  
  
 有一对尚在构造中，这一对会一直用到关闭当前检查点。 这一对与活动的检查点文件一起，对于内存中 6.5 GB 的数据使用了 6.5 GB 的磁盘。 请注意，索引不在磁盘上保存，因而在此例中，磁盘上的总体大小小于内存中的大小。  
  
#### <a name="after-demo-reset"></a>演示重置之后  
 演示重置之后，如果系统上没有事务工作负荷，则不会立即回收磁盘空间，并且没有数据库检查点。 对于要在其各个阶段中移动并最终丢弃的检查点文件，需要发生一些检查点和日志截断事件，以启动检查点文件合并以及启动垃圾收集。 如果在系统中具有事务工作负荷 [并且在使用完整恢复模式时进行定期日志备份]，则这些事件会自动发生，但是在系统处于空闲时不会自动发生（如演示方案所示）。  
  
 在示例中，演示重置之后，可能会出现如下内容  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| 磁盘上大小（以 MB 为单位） |
| ------------------ |
|11839|
  
 差不多 12 GB，这显著大于演示重置之前的 9 GB。 这是因为某些检查点文件合并已启动，但是某些合并目标尚未安装，某些合并源文件尚未清理，如下所示：  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | 磁盘上大小 (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|ACTIVE|DATA|38|5152|  
|ACTIVE|DELTA|38|1331|  
|MERGE TARGET|DATA|7|896|  
|MERGE TARGET|DELTA|7|56|  
|MERGED SOURCE|DATA|13|1772|  
|MERGED SOURCE|DELTA|13|455|  

 事务活动在系统中进行时，合并目标会进行安装，合并源会进行清理。  
  
 第二次运行演示工作负荷（演示重置之后插入 1000 万个销售订单）之后，可以看到在第一次运行工作负荷过程中构造的文件已清理。 如果在工作负荷运行期间多次运行以上查询，则可以查看检查点文件在各个阶段间移动。  
  
 第二次运行工作负荷（插入 1000 万个销售订单）之后，可以看到磁盘利用率非常类似于第一次运行之后的情况（不过不一定相同，因为系统在本质上是动态的）。 例如：  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | 磁盘上大小 (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|2|268|  
|UNDER CONSTRUCTION|DELTA|2|16|  
|ACTIVE|DATA|41|5608|  
|ACTIVE|DELTA|41|328|  

 在此例中，有两个检查点文件对处于“尚在构造”状态，这意味着多个文件对已变为“尚在构造”状态（可能是因为工作负荷的并发级别较高）。 多个并发线程同时需要一个新文件对，因而将一对从“预先创建”变为“尚在构造”。  
  
## <a name="see-also"></a>另请参阅

[内存中 OLTP（内存中优化）](in-memory-oltp-in-memory-optimization.md)