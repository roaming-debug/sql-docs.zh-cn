---
title: 扩展性体系结构
description: 本文介绍用于在 SQL Server 机器学习服务上运行外部 Python 或 R 脚本的扩展性框架的体系结构。 该脚本在语言运行时环境中作为核心数据库引擎的扩展执行。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: beb02a4741ccef20b4c412c05489d8047b1475e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471308"
---
# <a name="extensibility-architecture-in-sql-server-machine-learning-services"></a>SQL Server 机器学习服务中的扩展性体系结构 
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍用于在 SQL Server 机器学习服务上运行外部 Python 或 R 脚本的扩展性框架的体系结构。 该脚本在语言运行时环境中作为核心数据库引擎的扩展执行。

## <a name="background"></a>背景

在 SQL Server 2016 中引入了扩展性框架，用于支持 [R Services](../r/sql-server-r-services.md) 的 R 运行时。 SQL Server 2017 及更高版本支持[机器学习服务](../sql-server-machine-learning-services.md)的 Python。

扩展性框架用于在 SQL Server 与数据科学语言（例如 R 和 Python）之间提供一个接口。 目的是在将数据科学解决方案迁移到生产环境时减少摩擦，以及保护开发过程中公开的数据。 通过在 SQL Server 管理的安全框架内执行受信任的脚本语言，数据库管理员可以在维持安全性的同时允许数据科学家访问企业数据。

下图直观地说明了可扩展体系结构的机会和优势。

  ![与 SQL Server 集成的目标](../media/ml-service-value-add.png "机器学习服务值添加")

可以通过调用存储过程来运行外部脚本，然后将结果以表格结果的形式直接返回到 SQL Server。 这样就可以在任何可以发送 SQL 查询并处理结果的应用程序中轻松地生成或使用机器学习。

+ 外部脚本执行受 SQL Server 数据安全性的约束。 运行外部脚本的用户只能访问在 SQL 查询中同样可用的数据。 如果由于权限不足而导致查询失败，则同一用户运行的脚本也会因为同一原因而失败。 系统在表、数据库和实例级别实施 SQL Server 安全性。 数据库管理员可以管理用户访问权限、外部脚本使用的资源以及添加到服务器的外部代码库。  

+ 可通过两种方式获得扩展和优化机会：受益于数据库平台（列存储索引、[资源治理](../../machine-learning/administration/resource-governor.md)）；以及特定于扩展插件的受益，例如，将适用于 R 和 Python 的 Microsoft 库用于数据科学模型时。 R 是单线程的，而 RevoScaleR 函数是多线程的，后者能够在多个内核之间分配工作负荷。

+ 部署使用 SQL Server 方法。 这些方法可以是包装了外部脚本的存储过程、嵌入式 SQL 或调用 PREDICT（用于从服务器上保留的预测模型返回结果）等函数的 T-SQL 查询。

+ 熟练使用特定工具和 IDE 的开发人员可以在这些工具中编写代码，然后将代码移植到 SQL Server。

## <a name="architecture-diagram"></a>体系结构关系图

该体系结构的设计理念是让外部脚本在与 SQL Server 不同的进程中运行，但具有在内部管理 SQL Server 上的数据和操作请求链的组件。 根据 SQL Server 版本的不同，支持的语言扩展包括 [R](extension-r.md)、[Python](extension-python.md) 以及第三方语言（例如 Java 和 .NET）。

  Windows 中的组件体系结构：_*
  
  ![Windows 组件体系结构](../media/generic-architecture-windows.png "组件体系结构")
  
  _*_Linux 中的组件体系结构：_*_

  ![Linux 组件体系结构](../media/generic-architecture-linux.png "组件体系结构")
  
组件包括用于调用外部运行时的 _Launchpad* 服务以及用于加载解释器和库的特定于库的逻辑。 启动器用于加载语言运行时和所有专有模块。 例如，如果代码包含 RevoScaleR 函数，则会加载 RevoScaleR 解释器。 **BxlServer** 和 **SQL Satellite** 管理与 SQL Server 的通信和数据传输。 

在 Linux 中，SQL 使用 Launchpad  服务与每个用户的独立 Launchpad 进程进行通信。

<a name="launchpad"></a>

## <a name="launchpad"></a>启动板

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 是用来管理和执行外部脚本的服务，其工作方式类似于全文索引和查询服务启动单独的主机来处理全文查询。 该 Launchpad 服务只能启动 Microsoft 发布的受信任启动器，或者经 Microsoft 认证满足性能和资源管理要求的启动器。

| 受信任的启动器 | 分机 | SQL Server 版本 |
|-------------------|-----------|---------------------|
| 适用于 Windows 的 R 语言的 RLauncher.dll | [R 扩展](extension-r.md) | SQL Server 2016 及更高版本 |
| 适用于 Windows 的 Python 3.5 的 Pythonlauncher.dll | [Python 扩展](extension-python.md) | SQL Server 2017 及更高版本 |
| 适用于 Linux 的 R 语言的 RLauncher.so | [R 扩展](extension-r.md) | SQL Server 2019 及更高版本 |
| 适用于 Linux 的 Python 3.5 的 Pythonlauncher.so | [Python 扩展](extension-python.md) | SQL Server 2019 及更高版本 |

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 服务在其自身的用户帐户下运行。 如果更改运行 Launchpad 的帐户，请务必使用 SQL Server 配置管理器来执行此操作，以确保将更改写入相关文件。

在 Windows 中，系统会为每个已添加 SQL Server 机器学习服务的数据库引擎实例创建一个单独的 [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 服务。 每个数据库引擎实例都有一个 Launchpad 服务，因此如果有多个具有外部脚本支持的实例，则每个实例都有一个 Launchpad 服务。 数据库引擎实例会绑定到为它创建的 Launchpad 服务。 存储过程或 T-SQL 中对外部脚本的所有调用都会导致 SQL Server 服务调用为同一实例创建的 Launchpad 服务。

若要以支持的特定语言执行任务，Launchpad 会从池中获取安全工作线程帐户，并启动附属进程以管理外部运行时。 每个附属进程都会继承 Launchpad 的用户帐户，并在脚本执行期间使用该工作线程帐户。 如果脚本使用并行进程，则会在同一个工作线程帐户下创建这些进程。

在 Linux 中，仅支持一个数据库引擎实例，并且该实例绑定一个 Launchpad 服务。 执行脚本后，该 Launchpad 服务使用低特权用户帐户 **mssql_satellite** 启动一个单独的 Launchpad 进程。 每个附属进程都会继承 Launchpad 的 mssql_satellite 用户帐户，并在脚本执行期间使用该帐户。

## <a name="bxlserver-and-sql-satellite"></a>BxlServer 和 SQL Satellite

**BxlServer** 是由 Microsoft 提供的一个可执行文件，用于管理 SQL Server 和语言运行时之间的通信。 它为 Windows 创建 Windows 作业对象，或为 Linux 创建命名空间，用来包含外部脚本会话。 它还为每个外部脚本作业预配安全的工作文件夹，并使用 SQL Satellite 管理外部运行时与 SQL Server 之间的数据传输。 如果在某个作业运行时运行[进程资源管理器](/sysinternals/downloads/process-explorer)，则可能会看到一个或多个 BxlServer 实例。

实际上，BxlServer 是语言运行时环境的配套项，该运行时环境与 SQL Server 一起传输数据和管理任务。 BXL 是二进制交换语言的缩写，是指在 SQL Server 与外部进程之间有效移动数据时所用的数据格式。 BxlServer 也是 Microsoft R Client 和 Microsoft R Server 等相关产品的重要组成部分。

**SQL Satellite** 是数据库引擎中包含的扩展性 API，它支持使用 C 或 C++ 实现的外部代码或外部运行时。

BxlServer 使用 SQL Satellite 执行以下任务：

+ 读取输入数据
+ 写入输出数据
+ 获取输入参数
+ 写入输出参数
+ 错误处理。
+ 将 STDOUT 和 STDERR 写回客户端

SQL Satellite 使用自定义数据格式，这种格式已针对 SQL Server 与外部脚本语言之间的快速数据传输进行优化。 在 SQL Server 与外部脚本运行时通信期间，SQL Satellite 会执行类型转换并定义输入和输出数据集的架构。

可以使用 Windows 扩展事件 (xEvent) 来监视 SQL Satellite。 有关详细信息，请参阅 [SQL Server 机器学习服务的扩展事件](../../machine-learning/administration/extended-events.md)。

## <a name="communication-channels-between-components"></a>各组件之间的通信通道

此部分介绍组件和数据平台之间的通信协议。

+ **TCP/IP**

  默认情况下，SQL Server 与 SQL Satellite 之间的内部通信使用 TCP/IP。

+ **命名管道**

  BxlServer 与 SQL Server 之间通过 SQL Satellite 进行的内部数据传输使用专有的压缩数据格式来增强性能。 可使用命名管道以 BXL 格式在语言运行时和 BxlServer 之间交换数据。

+ **ODBC**

  外部数据科学客户端与远程 SQL Server 实例之间的通信使用 ODBC。 将脚本作业发送到 SQL Server 的帐户必须有权连接到实例，同时有权运行外部脚本。

  此外，根据任务的不同，该帐户可能需要以下权限：

  + 读取作业使用的数据
  + 将数据写入表：例如，将结果保存到表中时
  + 创建数据库对象：例如，将外部脚本保存为新存储过程的一部分时。

  如果将 SQL Server 用作从远程客户端执行的脚本的计算上下文，并且可执行文件必须从外部源检索数据，则 ODBC 将用于写回。 SQL Server 将发出远程命令的用户标识映射到当前实例上的用户标识，并使用该用户的凭据运行 ODBC 命令。 执行此 ODBC 调用所需的连接字符串从客户端代码中获取。

+ **RODBC（仅限 R）** 

  可以使用 **RODBC** 在脚本内部发出其他 ODBC 调用。 RODBC 是用于访问关系数据库中数据的常用 R 包；但是，与 SQL Server 使用的同类提供程序相比，其性能通常较慢。 许多 R 脚本使用 RODBC 的嵌入式调用，类似于检索用于分析的“辅助”数据集。 例如，用于训练模型的存储过程可以定义一个 SQL 查询来获取用于训练模型的数据，但使用嵌入式 RODBC 调用来获取其他因子，以执行查找，或者从文本文件或 Excel 等外部源中获取新数据。

  以下代码演示了 R 脚本中嵌入的 RODBC 调用：

    ```R
    library(RODBC);
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    dbhandle <- odbcDriverConnect(connStr)
    OutputDataSet <- sqlQuery(dbhandle, "select * from table_name");
    ```

+ **其他协议**

  可能需要在“区块”中工作或将数据传输回远程客户端的进程还可以使用 [XDF 文件格式](/machine-learning-server/r/concept-what-is-xdf)。 实际数据传输通过编码的 blob 进行。

## <a name="see-also"></a>另请参阅

+ [SQL Server 中的 R 扩展](extension-r.md)
+ [SQL Server 中的 Python 扩展](extension-python.md)