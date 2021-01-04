---
title: SQL Server 语言扩展中的扩展性体系结构
titleSuffix: ''
description: 了解用于 SQL Server 语言扩展的扩展性体系结构，该体系结构允许在 SQL Server 中运行外部代码。 在 SQL Server 2019 中，支持 Java、Python 和 R。 代码在语言运行时环境中作为核心数据库引擎的扩展执行。
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 344e5f35e35202b6a44a4932db8a27424fdb8753
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471798"
---
# <a name="extensibility-architecture-in-sql-server-language-extensions"></a>SQL Server 语言扩展中的扩展性体系结构

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

了解用于 SQL Server 语言扩展的扩展性体系结构，该体系结构允许在 SQL Server 中运行外部代码。 在 SQL Server 2019 中，支持 Java、Python 和 R。 代码在语言运行时环境中作为核心数据库引擎的扩展执行。

## <a name="background"></a>背景

扩展性框架的用途是在 SQL Server 与外部语言之间提供接口。 通过在 SQL Server 管理的安全框架中执行受信任的语言，数据库管理员可以维护安全性，同时允许数据科学家访问企业数据。

<!-- We need to get a diagram like the one below.
The following diagram visually describes opportunities and benefits of the extensible architecture.

  ![Goals of integration with SQL Server](../media/ml-service-value-add.png "Machine Learning Services Value Add")
-->

任何支持的外部语言都可以通过调用存储过程来运行，并且结果会作为表格结果直接返回到 SQL Server。 这样可以轻松地从任何可以发送 SQL 查询并处理结果的应用程序使用外部语言。

## <a name="architecture-diagrams"></a>体系结构关系图

体系结构设计为使外部代码可在与 SQL Server 不同的进程中运行，但是具有在内部为 SQL Server 上的数据和操作管理请求链的组件。 
  
  Windows 中的组件体系结构：_*

  ![Windows 上的组件体系结构](../media/generic-architecture-windows.png "Windows 上的组件体系结构")
  
  _*_Linux 中的组件体系结构：_*_
  
  ![Linux 上的组件体系结构](../media/generic-architecture-linux.png "WindowsLinux 上的组件体系结构")
  
组件包括 Launchpad 服务，该服务用于调用外部运行时（例如 Java）和特定于库的逻辑，以便加载解释器和库。

<a name="launchpad"></a>

## <a name="launchpad"></a>启动板

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 是一种服务，用于管理负责脚本执行的外部进程的生存期、资源和安全边界。 这类似于全文索引和查询服务启动单独主机来处理全文查询的方式。 该 Launchpad 服务只能启动 Microsoft 发布的受信任启动器，或者经 Microsoft 认证满足性能和资源管理要求的启动器。

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 服务在 SQLRUserGroup 下运行，后者将 [AppContainers](/windows/desktop/secauthz/appcontainer-isolation) 用于执行隔离。

为每个已添加了 SQL Server 机器语言扩展的数据库引擎实例创建单独的 [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 服务。 每个数据库引擎实例都有一个 Launchpad 服务，因此如果有多个具有外部脚本支持的实例，则每个实例都有一个 Launchpad 服务。 数据库引擎实例会绑定到为它创建的 Launchpad 服务。 存储过程或 T-SQL 中对外部脚本的所有调用都会导致 SQL Server 服务调用为同一实例创建的 Launchpad 服务。

若要以支持的特定语言执行任务，Launchpad 会从池中获取安全工作线程帐户，并启动附属进程以管理外部运行时。 每个附属进程都会继承 Launchpad 的用户帐户，并在脚本执行期间使用该工作线程帐户。 如果脚本使用并行进程，则会在同一个工作线程帐户下创建这些进程。

## <a name="communication-channels-between-components"></a>各组件之间的通信通道

此部分介绍组件和数据平台之间的通信协议。

+ **TCP/IP**

  默认情况下，SQL Server 与 SQL Satellite 之间的内部通信使用 TCP/IP。

+ **ODBC**

  外部数据科学客户端与远程 SQL Server 实例之间的通信使用 ODBC。 将脚本作业发送到 SQL Server 的帐户必须有权连接到实例，同时有权运行外部脚本。

  此外，根据任务的不同，该帐户可能需要以下权限：

  + 读取作业使用的数据
  + 将数据写入表：例如，将结果保存到表中时
  + 创建数据库对象：例如，将外部脚本保存为新存储过程的一部分时。

  如果将 SQL Server 用作从远程客户端执行的脚本的计算上下文，并且可执行文件必须从外部源检索数据，则 ODBC 将用于写回。 SQL Server 将发出远程命令的用户标识映射到当前实例上的用户标识，并使用该用户的凭据运行 ODBC 命令。 执行此 ODBC 调用所需的连接字符串从客户端代码中获取。

+ **其他协议**

  可能需要在“区块”中工作或将数据传输回远程客户端的进程还可以使用 [XDF 文件格式](/machine-learning-server/r/concept-what-is-xdf)。 实际数据传输通过编码的 blob 进行。

## <a name="next-steps"></a>后续步骤

+ [什么是语言扩展？](../language-extensions-overview.md)