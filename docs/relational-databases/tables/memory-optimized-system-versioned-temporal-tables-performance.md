---
description: 系统版本控制的内存优化临时表的性能
title: 系统版本控制的内存优化临时表的性能 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 2e110984-7703-4806-a24b-b41e8c3018c6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a81d2b397418a934e51a7e9e0f8491d10e0cdba
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462588"
---
# <a name="memory-optimized-system-versioned-temporal-tables-performance"></a>系统版本控制的内存优化临时表的性能


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


本主题讨论使用系统版本控制内存优化临时表时的一些特定性能注意事项。

- 在向现有的非临时表添加系统版本控制时，更新和删除操作的性能可能会受到影响，因为历史记录表会自动更新。
- 每个更新和删除操作都将记录到内部内存优化历史记录表中，因此，如果你的工作负荷大规模使用这两项操作，可能会存在意外的内存消耗。 因此，建议你注意以下事项：

  - 不要从当前表执行大规模删除操作，通过清理空间来增加可用的 RAM。 请考虑分批删除数据并通过调用 [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md)在两个批次之间手动调用数据刷新，或者在 **SYSTEM_VERSIONING = OFF** 时删除数据。
  - 不要一次性执行大规模的表更新操作，因为这样做所产生的内存消耗可能是更新非时态内存优化表所需内存量的两倍。 双倍的内存消耗只是暂时的，因为数据刷新任务会定期执行，以使计划边界内内部临时表的内存消耗保持稳定状态（约为当前时态表内存消耗的 10%）。 建议分批或在 SYSTEM_VERSIONING = OFF 时执行大规模的更新操作，如使用更新操作来设置新添加列的默认值。

- 数据刷新任务的激活时间段无法进行配置，但是你可以通过调用 [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md)时删除数据。
- 请考虑将群集列存储用作基于磁盘的历史记录表的存储选项，特别是在你计划对历史数据使用聚合或开窗函数运行分析查询时。 在这种情况下，群集列存储将成为历史记录表的最佳选择，因为它不仅提供良好的数据压缩，还具有“插入友好”的行为，可与历史记录数据的生成方式保持协调。

## <a name="see-also"></a>另请参阅

- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [创建系统版本控制的内存优化临时表](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [使用带有系统版本的内存优化临时表](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)
- [监视系统版本控制型内存优化临时表](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [临时表](../../relational-databases/tables/temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
