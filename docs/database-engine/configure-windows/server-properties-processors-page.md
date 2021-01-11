---
title: 服务器属性（“处理器”页）
description: 熟悉 SQL Server 中的处理器设置。 了解哪些选项控制工作线程数、处理器分配和其他属性。
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684209"
---
# <a name="server-properties-processors-page"></a>服务器属性（“处理器”页）

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用此页可以查看或修改处理器选项。 只有在安装了多个处理器时，才可以启用处理器关联设置。  

## <a name="options"></a>选项

### <a name="processor-affinity"></a>处理器关联
将处理器分配给特定的线程，以消除处理器重新加载和减少处理器之间的线程迁移。 有关详细信息，请参阅 [关联掩码服务器配置选项](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)。

### <a name="io-affinity"></a>I/O 关联
将 [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server 磁盘 I/O 绑定到 CPU 的指定子集。 有关详细信息，请参阅 [关联 I/O 掩码服务器配置选项](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md)。

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>自动设置所有处理器的处理器关联掩码
允许 SQL Server 设置处理器关联。

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>自动设置所有处理器的 I/O 关联掩码
允许 SQL Server 设置 I/O 关联。

### <a name="maximum-worker-threads"></a>最大工作线程数
如果为 0，则允许 SQL Server 动态设置工作线程数。 对于大多数系统而言，此为最佳设置。 但是，根据您的系统配置，将此选项设置为特定值有时可以提高性能。 有关详细信息，请参阅 [配置 max worker threads 服务器配置选项](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)。  

### <a name="boost-sql-server-priority"></a>提升 SQL Server 的优先级
指定 SQL Server 是否应当以比同一计算机上的其他进程更高的 Microsoft Windows 计划优先级运行。 有关详细信息，请参阅 [配置 priority boost 服务器配置选项](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)。  

> [!Note]
> 此选项不适用于 SSMS 18.x 和更高版本。

### <a name="use-windows-fibers-lightweight-pooling"></a>使用 Windows 纤程(轻型池)
使用 Windows 纤程代替 SQL Server 服务的线程。 此选项仅适用于 Windows 2003 Server Edition。 有关详细信息，请参阅 [lightweight pooling 服务器配置选项](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)。

> [!Note]
> 此选项不适用于 SSMS 18.x 和更高版本。

### <a name="configured-values"></a>配置值
显示此窗格上选项的配置值。 如果更改了这些值，请选择“运行值”以查看更改是否已生效。 如果尚未生效，则必须首先重新启动 SQL Server 实例。

### <a name="running-values"></a>“运行值”
查看此窗格上选项的当前运行值。 这些值是只读的。

## <a name="see-also"></a>另请参阅
[服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


