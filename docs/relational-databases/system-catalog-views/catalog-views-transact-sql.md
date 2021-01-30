---
description: " (Transact-sql) 的系统目录视图"
title: 目录视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql13.SysViewExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- catalog metadata [SQL Server]
- system views [SQL Server], catalog
- metadata [SQL Server], views
- catalogs [SQL Server], metadata
- catalog views [SQL Server]
- Database Engine [SQL Server], metadata
- catalog views [SQL Server], about catalog views
ms.assetid: 13bccc2f-ed3c-4b58-abd0-ca8bf34a66b8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d209068156b63111114b1c81111d8f1ebb1cc3f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193063"
---
# <a name="system-catalog-views-transact-sql"></a> (Transact-sql) 的系统目录视图

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

目录视图返回 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]使用的信息。 建议您使用目录视图这一最常用的目录元数据界面，它可为您提供最有效的方法来获取、转换并显示此信息的自定义形式。 所有用户可用的目录元数据都通过目录视图来显示。

> [!NOTE]
> 目录视图不包含有关复制、备份、数据库维护计划或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理目录数据的信息。

 某些目录视图从其他目录视图继承行。 例如， [sys.databases](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) 目录视图继承自 [sys.databases](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) 目录视图。 sys.objects 目录视图称为基本视图，而 sys.tables 视图称为派生视图。 sys.tables 目录视图返回专用于表的列，同时还返回 sys.objects 目录视图返回的所有列。 sys.objects 目录视图返回表之外的对象（例如，存储过程和视图）的行。 创建表之后，表的元数据将在两个视图中返回。 尽管两个目录视图返回有关表的不同级别的信息，但在此表的元数据中只有一个具有一个名称和一个 object_id 的项。 这可以总结如下：

- 基本视图包含列的子集和行的超集。
- 派生视图包含列的超集和行的子集。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 可能会通过在列列表的末尾添加列来扩充任何系统目录视图的定义。 建议不要使用 \* 在生产代码中 *sys.catalog_view_name* 选择的语法，因为返回的列数可能会更改和中断您的应用程序。

 中的目录视图具有如下类别：

:::row:::
    :::column:::
        [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)
        
        [Azure SQL Database 目录视图](../../relational-databases/system-catalog-views/azure-sql-database-catalog-views.md)
        
        [更改跟踪目录视图 &#40;Transact-sql&#41;](../system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)
        
        [&#40;Transact-sql&#41;的 CLR 程序集目录视图 ](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)
        
        [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)
        
        [数据空间 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)
        
        [&#40;Transact-sql 的数据库邮件视图&#41;](../../relational-databases/system-catalog-views/database-mail-views-transact-sql.md)
        
        [&#40;Transact-sql&#41;的数据库镜像见证服务器目录视图 ](../system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
        
        [数据库和文件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)
        
        [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)
        
        [扩展事件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)
        
        [扩展属性目录视图 (Transact-SQL)](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)
        
        [外部操作目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/external-operations-catalog-views-transact-sql.md)
        
        [Filestream 和 FileTable 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
        
        [&#40;Transact-sql&#41;的全文搜索和语义搜索目录视图 ](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)
        
        [链接服务器目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)
    :::column-end:::
    :::column:::
        [消息 &#40;&#41; 目录视图的错误 &#40;Transact-sql&#41;](../system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)
        
        [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)
        
        [Transact-sql&#41;&#40;分区函数目录视图 ](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)
        
        [基于策略的管理视图 (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)
        
        [Resource Governor 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)
        
        [查询存储目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
        
        [&#40;Transact-sql&#41;的标量类型目录视图 ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)
        
        [&#40;Transact-sql&#41;的架构目录视图 ](../system-catalog-views/schemas-catalog-views-sys-schemas.md)
        
        [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
        
        [Service Broker 目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)
        
        [服务器范围内的配置目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)
        
        [空间数据目录视图](../../relational-databases/system-catalog-views/spatial-data-catalog-views.md)
        
        [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)
        
        [Stretch Database 目录视图 &#40;Transact-sql&#41;](../system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md)
        
        [XML 架构 &#40;XML 类型系统&#41; 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅

- [&#40;Transact-sql&#41;的信息架构视图 ](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)
- [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)
