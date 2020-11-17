---
title: 可用性组的增强型故障转移
description: 启用增强的数据库故障转移的步骤，如果 Always On 可用性组中的数据库不再能够写入事务，则会触发故障转移。
ms.custom: seodec18
ms.date: 06/03/2020
ms.prod: sql
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], enhanced database failover
- Availability Groups [SQL Server], failover
ms.assetid: ''
author: cawrites
ms.reviewer: mikeray
ms.author: chadam
ms.openlocfilehash: 8a0e98398868676c6b7e5c3cdcaae1172a7a785a
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584269"
---
# <a name="enable-enhanced-database-failover-to-a-database-in-an-always-on-availability-group"></a>向 Always On 可用性组中的数据库启用增强的数据库故障转移
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

在 SQL Server 2012 和 2014 中，如果加入主要副本上的可用性组的数据库无法继续写入事务，即使为进行自动故障转移同步和配置副本，此数据库也无法触发故障转移。

SQL Server 2016 引入了一个新的可选行为，名为“增强的数据库故障转移”，可通过向导或使用 Transact-SQL 进行设置  。 当加入可用性组的数据无法继续写入事务时，如果已启用此选项且配置了自动故障转移，则会触发到同步次要副本的故障转移。

**应用场景 1**

在实例 A 和实例 B 之间配置一个可用性组，其中包含一个名为 DB1 的数据库。 DB1 的数据文件位于驱动器 E，事务日志文件位于驱动器 F。可用性模式设置为随自动故障转移同步提交。 在可用性组上配置增强的数据库故障转移新选项。 两个副本当前处于同步状态。 出现了一个问题，导致驱动器 E 出现故障。 此应用场景不会触发增强的数据库故障转移，因为驱动器 E 不包含事务日志。  

**应用场景 2**

采用与应用场景 1 中相同的可用性组配置。 这次不是驱动器 E 故障，而是事务日志驱动器 - 驱动器 F 出现故障。 此情况将触发故障转移，因为满足增强的数据库故障转移的所需条件：无法访问事务日志，也就意味着数据库无法写入事务。

**应用场景 3**

在实例 A 和实例 B 之间配置一个可用性组，其中包含两个数据库：DB1 和 DB2。 可用性模式设置为随自动故障转移模式同步提交，同时启用增强的数据库故障转移。 无法访问包含 DB2 的数据和事务日志文件的磁盘。 检测到问题时，可用性组将自动故障转移到实例 B。

## <a name="configure-enhanced-failover"></a>配置增强型故障转移

可使用 SQL Server Management Studio 或 Transact-SQL 配置增强的数据库故障转移。 PowerShell cmdlet 当前不具有此功能。 默认情况下，增强的数据库故障转移处于禁用状态。

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

可在创建辅助功能组的过程中使用 SQL Server Management Studio 启用增强的数据库故障转移。 创建可用性组后只能使用 Transact-SQL 将其启用或禁用。

*手动创建可用性组*

利用[使用“新建可用性组”对话框 (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md) 一文中的说明创建可用性组。 若要启用增强的数据库故障转移，请在“数据库级别运行状况检测”旁边选中它的复选框  。

*使用可用性组向导*

利用[使用可用性组向导 (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md) 一文中的说明执行操作。 可在“指定可用性组名称”对话框中找到启用增强的数据故障转移的选项。 若要启用，请勾选“数据库级别运行状况检测”旁边的框  。

### <a name="transact-sql"></a>Transact-SQL

若要在创建可用性组的过程中配置增强的数据库故障转移行为，DB_FAILOVER 必须设置为 ON，如下所示：

```SQL
CREATE AVAILABILITY GROUP [AGNAME]
WITH ( DB_FAILOVER = ON)
...
```
若要在配置可用性组后添加此行为，请使用 ALTER AVAILABILITY GROUP 命令：
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = ON)
```
若要禁用此行为，请发出以下 ALTER AVAILABILITY GROUP 命令：
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = OFF)
```
### <a name="dynamic-management-view"></a>动态管理视图
若要查看可用性组是否已启用增强的数据库故障转移，请查询动态管理视图 `sys.availability_groups`。 如果已禁用，`db_failover` 列将显示 0；如果已启用，则显示 1. 

## <a name="next-steps"></a>后续步骤 

- [配置数据库运行状况检测](sql-server-always-on-database-health-detection-failover-option.md)

- [使用可用性组向导 (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [使用“新建可用性组”对话框 (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [使用 Transact-SQL 创建可用性组](create-an-availability-group-transact-sql.md)

