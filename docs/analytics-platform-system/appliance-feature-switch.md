---
title: 功能切换
description: 显示有关分析平台系统 AU7 中引入的两个功能开关的信息。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 656b1592dd50567f59744e39c24be7d93b5b220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052828"
---
# <a name="appliance-feature-switches"></a>设备功能切换

**功能切换** 页面显示有关分析平台系统 AU7 和更高版本中引入的功能开关的信息。 使用此配置页可以更新或启用/禁用分析平台系统中的功能和设置。

> [!NOTE]
> 更改功能开关值需要重启服务。

![DWConfig 设备功能开关](media/feature-switch/SQL_Server_PDW_DWConfig_feature_switch.png "DWConfig 设备功能开关")

## <a name="autostatsenabled"></a>AutoStatsEnabled

控制自动统计信息功能。 升级到 AU7 后，此功能开关默认设置为 true。 升级后创建的任何数据库将继承统计信息的自动创建和异步更新。 对于现有数据库，数据库管理员可以使用 [ALTER database (并行数据仓库) ](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw)启用自动统计信息。 有关统计信息的详细信息，请参阅 [统计](../relational-databases/statistics/statistics.md)信息。

## <a name="maxdopforinsertqueries"></a>MaxDOPForInsertQueries

允许你为插入/选择操作选取大于1的 maxdop 设置。 此设置的选项有0、1、2和4，默认值为1。

## <a name="optimizecommonsubexpressions"></a>OptimizeCommonSubExpressions

通过消除 SQL 查询优化器中常见子表达式的数据移动，提高查询性能。 可在 [此处](common-sub-expression-elimination.md)找到有关此功能的详细说明。

## <a name="usecatalogqueries"></a>UseCatalogQueries

对某些元数据调用使用目录对象，而不是使用 SMO 会提高性能。 默认情况下，在 CU 7.1 中设置为 true，此开关控制该行为。

## <a name="dmsprocessstopmessagetimeoutinseconds"></a>DmsProcessStopMessageTimeoutInSeconds

控制在取消涉及数据移动的查询时，数据移动服务 (DM) 等待同步到繁忙系统。 更新到 AU7 会将此值 (15 分钟) 默认设置为900秒。 有效范围为 0-3600 秒。
