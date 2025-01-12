---
description: 项目设置（迁移）(SybaseToSQL)
title: " (迁移的项目设置)  (SybaseToSQL) |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 82f8857f-7ab1-4738-ab6e-b1e95ea94924
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 96b5550449e11cd314e736a36e3845ba92f542c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069915"
---
# <a name="project-settings-migration-sybasetosql"></a>项目设置（迁移）(SybaseToSQL)
" **项目设置** " 对话框的 "迁移" 页包含的设置可用于自定义 SSMA 将数据从 Sybase 自适应服务器企业 (ASE) 迁移到的方式 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "迁移" 窗格。  
  
-   若要指定所有 SSMA 项目的设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，从 " **迁移目标版本** " 下拉菜单中选择需要查看其设置的迁移项目类型，然后单击左窗格底部的 " **常规** "，然后单击 " **迁移**"。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后单击 " **迁移**"。  
  
## <a name="date-correction-options"></a>日期更正选项  
  
|术语|定义|  
|--------|--------------|  
|**替换不受支持的日期**|指定 SSMA 是否应更正早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 1753 年1月1日 (01 月之前 **的日期日期** 。<br /><br />若要保留当前日期值，请选择 " **不执行任何操作**"。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会接受日期时间列中01年1月 1753 1 日之前的日期。 如果使用旧日期，则必须将日期时间值转换为字符值。<br /><br />若要将1753年1月之前的日期转换为 NULL，请选择 " **替换为 null**"。<br /><br />若要将1753年1月之前的日期替换为支持的日期，请选择 " **替换为支持的最早日期**"。<br /><br />**默认模式**：不执行任何操作<br /><br />**乐观模式**：不执行任何操作<br /><br />**完全模式**：替换为支持的最接近日期|  
  
## <a name="migration-engine"></a>迁移引擎  
  
|术语|定义|  
|--------|--------------|  
|**迁移引擎**|指定数据迁移期间使用的数据库引擎。 客户端数据迁移指的是 SSMA 客户端从源检索数据并将数据大容量插入到 SQL Server 中。 服务器端数据迁移指的是 SSMA 数据迁移引擎， (大容量复制程序) 在 SQL Server 框上运行的 SQL 代理作业从数据源中检索数据，并直接插入 SQL Server，从而避免了额外的客户端跃点 (更好的性能) 。<br /><br />**默认模式**：客户端数据迁移引擎<br /><br />**乐观模式**：客户端数据迁移引擎<br /><br />**完整模式**：客户端数据迁移引擎|  
  
> [!IMPORTANT]  
> 当 " **迁移引擎** " 选项设置为 " **服务器端数据迁移引擎**" 时，将显示一个新的项目设置选项 " **使用32位服务器端数据迁移引擎** "。 它指定是否使用32位或64位大容量复制程序 (BCP) 实用工具来迁移数据。  
  
## <a name="miscellaneous-options"></a>“杂项”选项  
  
|术语|定义|  
|--------|--------------|  
|**批大小**|指定数据迁移期间使用的批大小。<br /><br />**默认模式**：10000<br /><br />**乐观模式**：10000<br /><br />**完整模式**：10000|  
|**检查约束**|指定 SSMA 在将数据插入 SQL Server 表时是否应检查约束。<br /><br />**默认模式**： False<br /><br />**乐观模式**： False<br /><br />**完整模式**： False|  
|**数据迁移超时**|指定数据迁移期间使用的超时<br /><br />**默认模式**：15<br /><br />**乐观模式**：15<br /><br />**完整模式**：15|  
|**扩展数据迁移选项**|在单独的 "详细信息" 选项卡中为每个表显示额外的数据迁移选项。<br /><br />**默认模式**：隐藏<br /><br />**乐观模式**：隐藏<br /><br />**完全模式**：隐藏|  
|**激发触发器**|指定 SSMA 在将数据添加到 SQL Server 表时是否应激发插入触发器。<br /><br />**默认模式**： False<br /><br />**乐观模式**： False<br /><br />**完整模式**： False|  
|**保留标识**|指定 SSMA 在将数据添加到 SQL Server 时是否保留 Sybase 标识值。 如果值为 False，则将由目标分配标识值。<br /><br />**默认模式**： True<br /><br />**乐观模式**： True<br /><br />**完整模式**： True|  
|**保留 Null**|指定 SSMA 在将数据添加到 SQL Server 时是否保留源数据中的空值，而不管 SQL Server 中指定的默认值。<br /><br />**默认模式**： True<br /><br />**乐观模式**： True<br /><br />**完整模式**： True|  
|**出错时**|发生错误时停止数据迁移。 它有三个选项：<br /><br />**停止迁移：** 停止数据迁移操作<br /><br />**转到下一个表：** 停止到当前表的数据迁移并继续下一个表<br /><br />**继续下一批：** 停止数据迁移到当前批处理，并继续执行下一个批处理<br /><br />**默认模式**：前进到下一批<br /><br />**乐观模式**：前进到下一批<br /><br />**完整模式**：前进到下一批|  
|**数字的舍入分数部分**|指定在迁移到整数类型时是否修整小数和数值数据的小数部分，或在小数部分不重要时显示错误消息<br /><br />**默认模式**：否<br /><br />**乐观模式**：否<br /><br />**完整模式**：否|  
|**Sybase Unicode 字节序**|指定 Sybase Unicode 字符串的 endian 类型。 可以为此特定设置设置以下选项：<br /><br />小字节序<br /><br />大字节序<br /><br />**默认模式**：小字节序<br /><br />**乐观模式**：小字节序<br /><br />**完整模式**：小字节序|  
|**表锁**|指定 SSMA 在数据迁移过程中将数据添加到表时是否锁定表。 获取批量复制操作持续时间的批量更新锁定。 如果该值为 False，则在行级别设置锁。<br /><br />**默认模式**： True<br /><br />**乐观模式**： True<br /><br />**完整模式**： True|  
|**使用游标**|如果设置了此选项，则使用游标从源数据库中检索数据。<br /><br />**默认模式**： False<br /><br />**乐观模式**： False<br /><br />**完整模式**： False|  
  
## <a name="parallel-data-migration"></a>并行数据迁移  
  
|术语|定义|  
|--------|--------------|  
|**并行数据迁移模式**|指定用于分叉线程以启用并行数据迁移的模式。 在 Auto 模式下，SSMA 会选择默认情况下 (10) 线程数，以迁移数据。 在自定义模式下，用户可以指定分叉于迁移数据的线程数 (最小值为1，最大值为 100) 。 目前，只有客户端数据迁移引擎支持并行数据迁移。<br /><br />**默认模式**：自动<br /><br />**乐观模式**：自动<br /><br />**完全模式**：自动|  
  
> [!IMPORTANT]  
> 如果将 " **并行数据迁移模式** " 选项设置为 " **自定义**"，则会显示新的项目设置选项 " **线程计数** "。 它指定用于数据迁移的线程数。  
  
