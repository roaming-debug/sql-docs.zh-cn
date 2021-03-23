---
title: 比较 SQL 数据迁移工具
titleSuffix: SQL Server
description: '比较 SQL 数据迁移工具来确定哪种工具最适合你的业务需求，这些工具包括数据迁移助手 (DMA)、Azure Migrate、Azure 数据库迁移服务、SQL Server 迁移助手 (SSMA)、数据库实验助手 (DEA)。 '
ms.date: 03/15/2021
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajpo
ms.author: rajpo
ms.openlocfilehash: fc406a39dc0b5d1a80e4f357a28b6945da0bf453
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603287"
---
# <a name="compare-sql-data-migration-tools"></a>比较 SQL 数据迁移工具

Microsoft 提供了一套工具和服务，它们旨在帮助用户将各种类型的源数据库迁移到各种目标环境中。 

本文简要介绍了可用于迁移到 SQL Server 和 Azure SQL 的工具。 

## <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 提供一个中心用于发现和评估本地服务器、基础结构、应用程序和数据并将其大规模地迁移到 Azure。  Azure Migrate 跨服务器、数据库和应用程序提供统一的迁移。 

使用 Azure Migrate 可跨整个数据中心发现所有 SQL Server、评估应用程序依赖关系、了解这些 SQL Server 就迁移到 Azure SQL 的准备情况，还可获取 Microsoft 建议、最优的 Azure SQL 部署选项，以及满足工作负载性能需求的适当 SKU。  你还可根据你的需求权益，获取在 Azure SQL 上运行这些数据库的每月费用估计。 

可在以下方案中使用 Azure Migrate： 
- 评估和发现你的 SQL Server 数据资产。 
- 获取 Azure SQL 部署建议、目标大小调整和每月费用估计。
- 将整个数据资产直接迁移到 Azure VM 上的 SQL Server。 

若要了解详细信息，请查看 [Azure Migrate 文档](/azure/migrate/)。 

## <a name="data-migration-assistant-dma"></a>数据迁移助手 (DMA)

数据迁移助手 (DMA) 通过检测可能影响新版 SQL Server 或 Azure SQL 数据库中数据库功能的兼容性问题，帮助你升级到新式数据平台。 DMA 为目标环境提供了性能和可靠性改进建议，并可便于将架构、数据和非包含对象从源服务器迁移到目标服务器。

可在以下方案中使用 DMA： 
- 在 Windows、Linux 和 Azure VM 上的 SQL Server 上，将 SQL Server 2005 及更高版本升级到 SQL Server 2012、SQL Server 2014、SQL Server 2016、SQL Server 2017 和 SQL Server 2019。 
- 检测更新目标版本的 SQL Server 或 Azure SQL 上可能会影响数据库功能的兼容性问题，并提供缓解步骤。 
- 将架构、数据和未包含的对象从源服务器移动到 SQL Server 或 Azure SQL。 

若要了解详细信息，请查看[数据迁移助手文档](../../dma/dma-overview.md)。 

## <a name="sql-server-migration-assistant-ssma"></a>SQL Server 迁移助手 (SSMA)

SQL Server 迁移助手 (SSMA) 是一种工具，它旨在将数据库从备用数据库引擎自动迁移到 SQL Server 和 Azure SQL。 

可在以下方案中使用 SSMA：
- 将 Microsoft Access、DB2、MySQL、Oracle 和 SAP ASE 数据库迁移到 SQL Server。
- 将 Microsoft Access、DB2、MySQL、Oracle 和 SAP ASE 数据库迁移到 Azure SQL。

若要了解详细信息，请查看 [SQL Server 迁移助手文档](../../ssma/sql-server-migration-assistant.md)

## <a name="azure-database-migration-service-dms"></a>Azure 数据库迁移服务 (DMS)

使用 Azure 数据库迁移服务可从多个数据库源无缝迁移到 Azure 数据平台，且会尽量缩短停机时间。  数据库迁移服务提供了一个可复原且可靠的迁移管道，它可尽量减少用户在整个迁移过程中的参与。 

可在以下方案中使用数据库迁移服务：
- 将这两种数据库迁移到 Azure SQL，尤其是大规模迁移且适合大型迁移（这里是“大型”是指数据库的数量和大小）。 
- 将数据库迁移到 Azure 数据库。

若要了解详细信息，请查看 [Azure 数据库迁移服务文档](/azure/dms/)。 

## <a name="database-experimentation-assistant-dea"></a>数据库实验助手 (DEA)

数据库实验助手 (DEA) 是一种试验解决方案，它用于 SQL Server 升级。 DEA 可帮助你针对特定工作负载评估 SQL Server 的目标版本。 如果客户要从较早版本的 SQL Server（不低于 2005 版）升级到 SQL Server 最新版本，那么他们可使用该工具提供的分析指标。

可在以下方案中使用数据库实验助手：
- 捕获源 SQL Server 环境的工作负载，并评估源 SQL Server 上的工作负载来为迁移做准备。 
- 确定 SQL Server 迁移方案的兼容性错误和可能的降级查询。 

若要了解详细信息，请查看[数据库实验助手文档](../../dea/database-experimentation-assistant-overview.md)。


## <a name="quick-comparison"></a>快速比较

使用以下图表来比较 SQL 迁移工具的功能：


| 功能 |Azure Migrate  |数据迁移助手 (DMA)  |SQL Server 迁移助手 (SSMA)  | Azure 数据库迁移服务 (DMS) | 数据库实验助手 (DEA)|
|---------|---------|---------|---------|---|---|
|发现和评估 SQL 数据资产| 大规模 | 是 |否 |否|否|
|将 SQL Server 对象迁移到 SQL 数据库或 SQL 托管实例| 否| 是 | 否  | 是|否|
|将 SQL Server 直接迁移到 Azure VM 上的 SQL Server | 是 | 否 | 否 | 否| 否 |
|将 SQL Server 迁移（和/或升级）到 Azure VM 上的 SQL Server | 否 | 是| 否 | 否 | 否| 
|迁移非 SQL 对象 </br> （Oracle、Access、DB2 等） | 否 |否|是|否|否|
|迁移开源数据库 </br> （MySQL、PostgreSQL、MariaDB 等）| 否 | 否 | 否 | 是 | 否|
|比较源 SQL Server 和目标 SQL Server 之间的工作负载 |否|否|否|否|是|
|||||||

## <a name="next-steps"></a>后续步骤

使用 [Azure Migrate](/azure/migrate/how-to-create-azure-sql-assessment) 开始从其他数据库引擎迁移到 [SQL Server](../../ssma/sql-server-migration-assistant.md)、迁移到 [Azure SQL](/azure/azure-sql/migration-guides/)，或者评估你的 SQL 数据资产。 

