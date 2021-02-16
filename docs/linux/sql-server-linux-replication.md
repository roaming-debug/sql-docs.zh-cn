---
title: Linux 上的 SQL Server 复制
description: 了解 SQL Server 2017 (14.x) (CU18) 及更高版本对 Linux 上的 SQL Server 实例的 SQL Server 复制支持。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: bb322f11b866a7593d22e0d722fad84765cb5139
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346448"
---
# <a name="sql-server-replication-on-linux"></a>Linux 上的 SQL Server 复制

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) 及更高版本支持对 Linux 上的 SQL Server 实例进行 SQL Server 复制。

使用 SQL Server Management Studio (SSMS) [复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)配置 Linux 上的复制。

SQL Server 实例可以参与任何复制角色：

* 发布者
* 分发服务器
* 订阅者

复制架构可以混合和匹配操作系统平台。 例如，复制架构可能包括发布服务器和分发服务器的 Linux 上的 SQL Server 实例，并且订阅服务器包含 Windows 和 Linux 上的 SQL Server 实例。

Linux 上的 SQL Server 实例可以参与任何类型的复制。

* 事务性
* 快照

有关复制的详细信息，请参阅 [SQL Server 复制文档](../relational-databases/replication/sql-server-replication.md)。

## <a name="supported-features"></a>支持的功能

支持以下复制功能：

* 快照复制
* 事务复制
* 使用非默认端口的复制 <!--Add link to explanation-->
* 使用 AD 身份验证的复制
* 跨 Windows 和 Linux 的复制配置
* 事务复制的及时更新

## <a name="limitations"></a>限制

不支持以下功能：

* 合并复制
* 对等复制
* Oracle 发布

## <a name="next-steps"></a>后续步骤

[配置 Linux 上的 SQL Server 复制](sql-server-linux-replication-tutorial-tsql.md)

[示例：配置 Linux 上的 SQL Server 复制](sql-server-linux-replication-configure.md)
