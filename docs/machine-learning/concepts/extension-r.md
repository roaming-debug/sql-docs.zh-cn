---
title: R 语言扩展
description: 了解使用 SQL Server 机器学习服务和 SQL Server R Services 运行外部 R 脚本时使用的 R 扩展。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 41c0eb01dbcd2838a1c6f388e8b4304ef1eb3c7f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471278"
---
# <a name="r-language-extension-in-sql-server-machine-learning-services"></a>SQL Server 机器学习服务中的 R 语言扩展
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍使用 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)和 [SQL Server 2016 R Services](../r/sql-server-r-services.md) 运行外部 Python 脚本时使用的 R 扩展。 该扩展添加：

- R 执行环境
- 带有标准库和工具的基本 R 发行版
- Microsoft R 库：
  - [RevoScaleR](../r/ref-r-revoscaler.md)，用于大规模分析
  - [MicrosoftML](../r/ref-r-microsoftml.md)，用于机器学习算法
  - 用于访问 SQL Server 中的数据或 R 代码的其他库

## <a name="r-components"></a>R 组件

SQL Server 包含开源包和专有包。 基本 R 库通过 Microsoft 的开源 R 发行版安装：Microsoft R Open (MRO)。 R 的当前用户应该能够移植其 R 代码并在 SQL Server 上将其作为外部进程执行，且修改很少或无需修改。 MRO 独立于 SQL 工具安装，并在扩展性框架中的核心引擎进程外部执行。 在安装期间，必须同意开源许可证的条款。 此后，无需进一步的修改即可运行标准 R 包，就像运行 R 的其他任何开源发行版一样。 

SQL Server 不会修改基本 R 可执行文件，但必须使用安装程序安装的 R 版本，因为该版本是用于生成和测试专有包的版本。 若要详细了解 MRO 与可能从 CRAN 获得的 R 基本发行版有何差异，请参阅[与 R 语言以及 Microsoft R 产品和功能的互操作性](/r-server/what-is-r-server-interoperability)。

安装程序安装的 R 基本包发行版可以在与实例关联的文件夹中找到。 例如，如果在 SQL Server 默认实例上安装了 R Services，则 R 库默认位于以下文件夹中：`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`。 同样，与默认实例关联的 R 工具默认位于以下文件夹中：`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin`。

Microsoft 为并行和分布式工作负荷添加的 R 包包含以下库。

| 库 | 描述 |
|---------|-------------|
| [**RevoScaleR**](/machine-learning-server/r-reference/revoscaler/revoscaler) | 支持数据源对象以及数据浏览、操作、转换和可视化。 它支持创建远程计算上下文以及各种可缩放的机器学习模型，例如 **rxLinMod**。 这些 API 已经过优化，可以分析由于过大而无法装入内存的数据集，以及执行分布在多个核心或处理器之间的计算。 RevoScaleR 包还支持使用 XDF 文件格式来加速移动和存储用于分析的数据。 XDF 格式使用纵栏表存储且可移植，可用于加载并处理来自各种来源（包括文本、SPSS 或 ODBC 连接）的数据。 |
| [**MicrosoftML**](/r-server/r/concept-what-is-the-microsoftml-package) | 包含针对速度和准确性进行了优化的机器学习算法，以及用于处理文本和图像的内联转换。 有关详细信息，请参阅 [SQL Server 中的 MicrosoftML](../r/ref-r-microsoftml.md)。 | 

## <a name="using-r-in-sql-server"></a>在 SQL Server 中使用 R

可使用基本函数编写 R 脚本，但要从多处理中受益，则必须将 **RevoScaleR** 和 **MicrosoftML** 模块导入 R 代码，然后调用其函数来创建并行执行的模型。 
 
支持的数据源包括 ODBC 数据库、SQL Server 和用于与其他源或通过 R 解决方案交换数据的 XDF 文件格式。 输入数据必须为表格格式。 所有 R 结果都必须以数据帧的形式返回。

支持的计算上下文包括本地或远程 SQL Server 计算上下文。 远程计算上下文指在一台计算机（例如工作站）上开始，但随后将脚本执行切换到远程计算机的代码执行。 切换计算上下文要求两个系统具有同一 RevoScaleR 库。

正如用户所预期的一样，本地计算上下文包括在与数据库引擎实例相同的服务器上执行 R 代码，以及 T-SQL 内部或嵌入存储过程的代码。 还可以通过定义远程计算上下文，从本地 R IDE 运行代码并使脚本在 SQL Server 计算机上执行。

## <a name="execution-architecture"></a>执行体系结构

以下关系图说明 SQL Server 组件与 R 运行时在每种支持的方案中的交互：在数据库内运行脚本，以及使用 SQL Server 计算上下文从 R 命令行远程执行。

### <a name="r-scripts-executed-from-sql-server-in-database"></a>从 SQL Server（数据库内）执行 R 脚本

从 SQL Server“内部”运行的 R 代码通过调用某个存储过程来执行。 因此，可以发出存储过程调用的任何应用程序都可以启动 R 代码的执行。  然后，SQL Server 按下图中概括的方式管理 R 代码的执行。

![rsql_indb780-01](../r/media/script_in-db-r.png)

1. 对 R 运行时发出的请求由传递给存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 的参数 _@language='R'_ 指示。 SQL Server 将此请求发送到 Launchpad 服务。
在 Linux 中，SQL 使用 Launchpad 服务与每个用户的独立 Launchpad 进程进行通信。 有关详细信息，请参阅[扩展性体系结构关系图](extensibility-framework.md#architecture-diagram)。
2. Launchpad 服务启动相应的启动器（在本例中为 RLauncher）。
3. RLauncher 启动外部 R 进程。
4. BxlServer 与 R 运行时协同工作，管理与 SQL Server 之间的数据交换并存储工作结果。
5. SQL Satellite 管理相关任务和进程与 SQL Server 之间的通信。
6. BxlServer 使用 SQL Satellite 向 SQL Server 传递状态和结果。
7. SQL Server 获取结果并关闭相关任务和进程。

### <a name="r-scripts-executed-from-a-remote-client"></a>从远程客户端执行 R 脚本

从支持 Microsoft R 的远程数据科学客户端进行连接时，可以使用 RevoScaleR 函数在 SQL Server 的上下文中运行 R 函数。 此工作流与前面的工作流不同，下图对此做了汇总。

![rsql_fromR2db-01](../r/media/remote-sqlcc-from-r2.png)

1. 对于 RevoScaleR 函数，R 运行时调用链接函数，后者接着调用 BxlServer。
2. BxlServer 已随附在 Microsoft R 中，在独立于 R 运行时的进程中运行。
3. BxlServer 确定连接目标并使用 ODBC 发起连接，传递 R 数据源对象的连接字符串中提供的凭据。
4. BxlServer 打开与 SQL Server 实例的连接。
5. 对于 R 调用，将调用 Launchpad 服务，该服务接着启动相应的启动器 RLauncher。 然后，R 代码的处理过程类似于从 T-SQL 运行 R 代码。
6. RLauncher 针对 SQL Server 计算机上安装的 R 运行时实例发出调用。
7. 将结果返回到 BxlServer。
8. SQL Satellite 管理与 SQL Server 之间的通信并清理相关作业对象。
9. SQL Server 将结果传回客户端。

## <a name="see-also"></a>另请参阅

+ [SQL Server 中的扩展性框架](extensibility-framework.md)
+ [SQL Server 中的 Python 和机器学习扩展](extension-python.md)