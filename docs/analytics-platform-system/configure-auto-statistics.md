---
title: 自动统计信息
description: 介绍分析平台系统 AU7 中引入的自动统计信息功能。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 4311195ce4b748ffeb73c4a001086b3f199b2c5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052778"
---
# <a name="configure-auto-statistics"></a>配置自动统计信息

了解如何将并行数据仓库配置为使用自动统计信息自动创建和更新统计信息。  使用此功能可改进查询计划，从而提高查询性能。

**适用于：** 从 2016 AU7 开始 (的 AP) 

## <a name="what-are-statistics"></a>什么是统计信息？
查询优化的统计信息是包含有关表的一列或多列中的值分布的统计信息的对象。 查询优化器使用这些统计信息来估计查询结果中的基数或行数。 通过这些基数估计，查询优化器可以创建高质量的查询计划。 例如，在 AP 中，MPP 查询优化器使用基数估计来选择无序或复制联接子句中使用的两个表中较小的一个，从而提高查询性能。  有关详细信息，请参阅 [统计信息](../relational-databases/statistics/statistics.md) 和 [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)

## <a name="what-are-auto-statistics"></a>什么是自动统计信息？
自动统计信息是查询优化器创建和自动更新以改进查询计划的统计信息。 加载、插入、更新和删除操作后，统计信息可能会过期。 如果没有自动统计信息，则需要进行自己的分析，以了解哪些列需要统计信息，以及何时需要更新统计信息。

自动统计信息包含以下三种设置： 

### <a name="auto_create_statistics"></a>AUTO_CREATE_STATISTICS
在自动创建统计信息选项 AUTO_CREATE_STATISTICS 为 ON 时，查询优化器将根据需要在查询谓词中的单独列上创建统计信息，以便改进查询计划的基数估计。 这些单列统计信息在现有统计信息对象中尚未具有直方图的列上创建。

### <a name="auto_update_statistics"></a>AUTO_UPDATE_STATISTICS 
在自动更新统计信息选项 AUTO_UPDATE_STATISTICS 为 ON 时，查询优化器将确定统计信息何时可能过期，然后在查询使用这些统计信息时更新它们。 统计信息将在插入、更新、删除或合并操作更改表或索引视图中的数据分布后过期。 查询优化器通过计算自最后统计信息更新后数据修改的次数并且将这一修改次数与某一阈值进行比较，确定统计信息何时可能过期。 该阈值基于表中或索引视图中的行数。

### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC
异步统计信息更新选项 AUTO_UPDATE_STATISTICS_ASYNC 将确定查询优化器是使用同步统计信息更新还是异步统计信息更新。 对于 AP，默认情况下会启用 "异步统计信息更新" 选项，查询优化器会以异步方式更新统计信息。 AUTO_UPDATE_STATISTICS_ASYNC 选项适用于为索引创建的统计信息对象、查询谓词中的单列以及使用 CREATE STATISTICS 语句创建的统计信息。

## <a name="configuration-settings-for-system-administrators"></a>系统管理员的配置设置
升级到 AP AU7 后，默认情况下会启用 "自动统计"。 系统管理员可以使用设备 Configuration Manager 中的 [功能开关](appliance-feature-switch.md) 选项启用或禁用自动统计信息。  启用后，用户可以更改每个数据库的统计信息设置。
更改任何功能开关值都需要在 AP 上重新启动服务。

## <a name="change-auto-statistics-settings-on-a-database"></a>更改数据库的自动统计信息设置
如果系统管理员启用了自动统计信息，则可以使用 [ALTER DATABASE (Parallel 数据仓库) ](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) 更改数据库的统计信息设置。 如果系统管理员启用了自动统计信息功能切换，则升级到 AU7 后创建的任何新数据库都将启用 "自动统计信息"。 升级到 AU7 之前已存在的所有数据库已禁用自动统计信息。 以下示例对现有数据库 myPDW 启用自动统计信息。

```sql
ALTER DATABASE myPDW SET AUTO_CREATE_STATISTICS ON
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS ON 
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS_ASYNC ON
```
 
仅当 AUTO_UPDATE_STATISTICS 为 ON 时，AUTO_UPDATE STATISTICS_ASYNC 选项才起作用。  因此，AUTO_UPDATE_STATISTICS 关闭并且 AUTO_UPDATE_STATISTICS_ASYNC 处于打开状态时，不会更新统计信息。 

### <a name="error-messages"></a>错误消息
你可能会收到错误消息 "PDW 中不支持此选项"。  当系统管理员尚未启用自动统计信息，并且您尝试在 ALTER DATABASE 中设置任一自动统计信息选项时，会出现此错误。 

### <a name="limitations-and-restrictions"></a>限制和局限
自动统计信息不适用于外部表。 

### <a name="check-the-current-values"></a>检查当前值
下面的查询将返回所有数据库的 "自动统计信息" 设置的当前值。

```sql
SELECT NAME
    , IS_AUTO_CREATE_STATS_ON 
    , IS_AUTO_UPDATE_STATS_ON
    , IS_AUTO_UPDATE_STATS_ASYNC_ON
FROM
    sys.databases;
```

返回值为1表示设置为 on，0表示设置为关闭。 

## <a name="next-steps"></a>后续步骤
若要查看查询的执行情况，请参阅 [监视活动查询](monitoring-active-queries.md)
