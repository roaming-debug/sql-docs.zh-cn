---
description: CDC 流组件
title: CDC 流组件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 5ae69ddf-27c3-467c-9af1-c89ec383f661
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 19b4d69708405a3c70ffaacd0f9d81e995f4aba8
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384613"
---
# <a name="cdc-flow-components"></a>CDC 流组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  用于 Microsoft [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 的 Change Data Capture 组件（由 Attunity 提供）可帮助 SSIS 开发人员处理 CDC，并降低 CDC 包的复杂性。  
  
 SSIS CDC 组件设计用于处理 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 功能，其中的源表可以是同一 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 数据库，也可以是 Oracle 数据库（在使用 Oracle CDC Service for [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]时）。 支持已分区表。  
  
 这些组件包括可简化 SSIS 包中更改数据的读取和处理体验的控制和数据流组件。 可以将这些组件添加到 Microsoft [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中的组件库，但需要单独安装。  
  
 以下是由 Attunity 提供的 Change Data Capture 获组件：  
  
 **CDC 控制流组件** ：  
  
 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)  
  
 **CDC 数据流组件** ：  
  
 [CDC 源](../../integration-services/data-flow/cdc-source.md)  
  
 [CDC 拆分器](../../integration-services/data-flow/cdc-splitter.md)  
  
## <a name="installation"></a>安装  
 本节介绍用于 Microsoft [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]的 CDC 组件的安装过程。  
  
 用于 SSIS 的 CDC 组件与用于 Microsoft SQL ServerÂ® 的 MicrosoftÂ® Change Data Capture Designer and Service for Oracle by Attunity 打包在一起。 SQL Server 功能包下载内容中包含此类组件。 从 [SQL Server 2016 功能包网页](https://www.microsoft.com/download/details.aspx?id=56833)下载功能包的组件。  
  
### <a name="version-support"></a>版本支持

#### <a name="sql-server-version-support"></a>SQL Server 版本支持

所有支持的 Microsoft SQL Server 版本均支持用于 SSIS 的 CDC 组件。 目前，支持的 SQL Server 版本包括 SQL Server 2012 到 SQL Server 2017 的所有版本。

#### <a name="operating-system-version-support"></a>操作系统版本支持
  
以下操作系统和平台支持用于 SSIS 的 CDC 组件：  
  
-   Windows 8 和 8.1
-   Windows 10  
-   Windows Server 2012 和 2012 R2
-   Windows Server 2016
  
### <a name="running-the-installation-program"></a>运行安装程序  
 在运行安装向导前，请务必关闭 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 。 然后按照安装向导中的说明进行操作。  
  
### <a name="restart-ssis-service"></a>重启 SSIS 服务 
安装 CDC 组件后，必须重新启动 SSIS 服务，以确保在 SQL [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 中开发包时，这些组件能够正常运行。  
  
安装这些组件后将显示一条消息。 在系统提示时单击 **“是”** 。  
  
### <a name="uninstalling-the-microsoft-cdc-components"></a>卸载 Microsoft CDC 组件  
 可以使用卸载向导卸载 CDC 源、CDC 拆分器或 CDC 控制任务。 如果你要使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 进行包开发，请务必在运行卸载向导前关闭 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 。  
  
## <a name="benefits"></a>优点  
 借助用于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件的 CDC 组件，SSIS 开发者可以轻松生成 SSIS 包来处理变更数据。 这些组件增强了 SSIS 开发人员处理 CDC 的能力并降低了 CDC 包的复杂性。  
  
 使用 SSIS CDC 提供的更改数据更易于进一步处理，从而便于复制、加载数据仓库、更新 OLAP 的渐变维度、审核更改，或方便应用于其他可能的用途。 所使用的进一步处理的类型由 SSIS 开发人员确定。  
  
 SSIS CDC 组件设计用于处理 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 功能，其中的更改表位于同一 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 数据库中。  
  
## <a name="getting-started-with-the-change-data-capture-components"></a>Change Data Capture 组件入门  
 典型的 CDC 包处理针对一组表的更改。 下图显示了此类 CDC 包的基本控制流部分。 此包被称为滴送处理包。  
  
 ![滴送处理包控制流](../../integration-services/data-flow/media/tricklefeedprocessing.gif "滴送处理包控制流")  
  
 这一 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 控制流包含两个 CDC 控制任务以及一个数据流任务。 第一个任务称为“获取 CDC 处理范围”，此任务为在称为“处理更改”的数据流任务中处理的更改建立 LSN 范围。 基于上一包运行期间处理的已保存在持久存储区中的更改建立此范围。  
  
 有关使用 CDC 控制任务的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md) 和 [CDC Control Task Editor](../control-flow/cdc-control-task.md)。  
  
 下图显示了 **“处理更改”** 数据流，该数据流在概念上演示了如何处理更改。  
  
 ![处理变化数据流](../../integration-services/data-flow/media/processchangesdataflow.gif "处理变化数据流")  
  
 此图中演示了以下步骤：  
  
-   针对表 X 的更改是一种 CDC 源，用来读取在父控制流中确定的 CDC 处理范围中对表 X 所作的更改。  
  
-   CDC 拆分器 X 用于将更改拆分为插入、删除和更新等操作。 在此情况下，假定 CDC 源被配置为生成净更改，以便并行处理不同的更改类型。  
  
-   然后在下游进一步处理特定的更改。 在此演示中，使用多个 ODBC 目标将更改插入表，但实际情况下处理方式可能有所不同。  
  
 有关 CDC 源的详细信息，请参阅：  
  
 [CDC 源](../../integration-services/data-flow/cdc-source.md)  
  
 [CDC 源编辑器（“连接管理器”页）](./cdc-source.md)  
  
 [CDC 源编辑器（“列”页）](./cdc-source.md)  
  
 [CDC 源编辑器（“错误输出”页）](./cdc-source.md)  
  
 有关 CDC 拆分器的详细信息，请参阅：  
  
 [CDC 拆分器](../../integration-services/data-flow/cdc-splitter.md)  
  
 生成 CDC 包时需要注意的基本事项之一就是：更改处理如何与数据的初始加载（或初始处理）交互。  
  
 CDC 组件支持三种不同的初始加载和更改处理方案：  
  
-   使用数据库快照执行的初始加载。 在这种情况下，更改处理从快照事件的 LSN 开始。  
  
-   从静默数据库执行的初始加载。 在这种情况下，初始加载期间不进行任何更改，以便在初始加载期间的某个时点对当前 LSN 抽样，并自该 LSN 开始更改处理。  
  
-   从活动数据库执行的初始加载。 在这种情况下，由于初始加载正在进行，对数据库不断进行更改，所以不存在可自其准确开始更改处理的单个 LSN。 在这种情况下，初始加载包开发人员可以在初始加载之前和之后对源数据库的当前 LSN 抽样。 然后，在处理更改时，应该在并行处理针对初始加载的更改时小心操作，因为在初始加载中已经可以看到某些已处理的更改（例如，由于在初始加载进程中已经读取插入的行，所以插入更改可能因重复键错误而失败）。  
  
 下图显示了可处理前两种方案的 SSIS 包：  
  
 ![处理前两个方案的 SSIS 包](../../integration-services/data-flow/media/scenarioonetwo.gif "处理前两个方案的 SSIS 包")  
  
 下图显示了可处理第三种方案的 SSIS 包：  
  
 ![处理第三方案的 SSIS 包](../../integration-services/data-flow/media/scenario3.gif "处理第三方案的 SSIS 包")  
  
 在初始加载包之后，会根据计划反复运行滴送更新包，以便一出现可使用的更改就送交处理。  
  
 将 CDC 处理的状态从初始加载包传递到滴送更新包以及在每个包的不同任务之间传递该状态时，需要借助特殊的 SSIS 包字符串变量。 此变量的值称为 CDC 状态，该状态反映初始加载包和滴送包所处理的各组表的 CDC 处理的当前状态。  
  
 CDC 状态变量的值需在永久性存储中留存和维护。 应在启动 CDC 处理前读取该值，并在处理完成后以当前状态保存。 加载和存储 CDC 状态的任务可由 SSIS 开发人员处理，但是 CDC 控制组件可以通过在数据库表中维护 CDC 状态值来自动处理此任务。  
  
## <a name="security-considerations"></a>安全注意事项  
 本节列出了与在 SSIS 中使用 CDC 组件相关的一些安全注意事项。  
  
### <a name="access-authorization-to-change-data"></a>针对更改数据的访问授权  
 滴送更新包需要针对 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 功能的访问权限。 默认情况下向 **db_owner** 固定数据库角色的成员授予此访问权限。 由于 **db_owner** 是一个权限很高的角色，在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中定义捕获实例时，建议为每个捕获实例关联一个安全访问控制角色，以便允许 SSIS CDC 包使用限制性更强的用户身份处理这些更改。  
  
### <a name="access-to-cdc-database-current-lsn"></a>针对 CDC 数据库当前 LSN 的访问权限  
 用于标记更改处理始自的 LSN 的 CDC 控制任务操作必须能够找到 CDC 数据库的当前 LSN。 此组件通过从 master 数据库使用 sp_replincrementlsn 来查找 LSN。 必须为用于连接 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 数据库的登录名授予针对此过程的执行权限。  
  
### <a name="access-to-cdc-states-table"></a>针对 CDC 状态表的访问权限  
 CDC 状态表用于自动持久化 CDC 状态，这些状态需要由用于连接 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 数据库的登录名来更新。 由于此表是 SSIS 开发人员创建的，所以要将 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 系统管理员设置为有权创建 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 数据库并执行管理和维护任务的用户。 此外，使用启用 CDC 的数据库的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 系统管理员必须了解 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CDC 技术及其执行方式。  
  
## <a name="grouping-tables-for-cdc-processing"></a>用于 CDC 处理的分组表  
 数据库项目的大小从几个表到成千上万的表不等。 设计初始加载和 CDC 包时，最好将表分组到较小的组中，以便简化管理和提高效率。 本节列出影响将表归入较小的组中的各种注意事项，表将在各组中进行初始加载，随后作为一个组来更新。  
  
 CDC 组件支持的 CDC 模式假设已经确定此分组。 每个组定义单独的 CDC 上下文，该上下文与其他组分开维护。 对于每个组，创建初始加载包和滴送更新包。 滴送更新根据更改处理约束的速率（例如 CPU 和 IO 占用、对其他系统的影响）和所需的滞后时间定期运行。  
  
 根据下列注意事项对表分组：  
  
1.  根据目标数据库。 写入不同目标数据库或执行不同处理的所有表应分配到不同的 CDC 组。  
  
2.  与引用完整性约束相关的表应分配到同一组中，以避免目标上出现引用完整性问题。  
  
3.  可容忍较高滞后时间的表可以归入一组，这样可以降低这些表的处理频率，从而减轻总体系统负荷。  
  
4.  更改率较高的表应分入较小的组，更改率较低的表应分入较大的组。  
  
 为每个 CDC 组创建了以下两个包：  
  
-   一个初始加载包，该包从源表读取整个数据范围并将其应用到目标表。  
  
-   一个滴送更新包，该包读取对源表所做的更改并将这些更改应用到目标表。 此包应定期执行。  
  
## <a name="cdc-state"></a>CDC 状态  
 每个 CDC 组都关联有某种状态，该状态表示为具有特定格式的字符串。 有关详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。 下表列出了可能的 CDC 状态值。  
  
|状态|描述|  
|-----------|-----------------|  
|0-(INITIAL)|对当前 CDC 组运行任何包之前存在的状态。 这也是 CDC 状态为空时的状态。<br /><br /> 有关 CDC 控制任务操作的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。|  
|1-ILSTART（初始-加载-已启动）|这是在初始加载包启动时存在的状态。 在 **MarkInitialLoadStart** 操作调用 CDC 控制任务后就会出现该状态。<br /><br /> 有关 CDC 控制任务操作的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。|  
|2- ILEND（初始-加载-已结束）|这是在初始加载包成功结束时存在的状态。 在 MarkInitialLoadEnd 操作调用 CDC 控制任务后就会出现该状态。<br /><br /> 有关 CDC 控制任务操作的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。|  
|3-ILUPDATE（初始加载更新）|在初始加载后仍在处理初始处理范围期间，首次运行更新包之后，就会出现该状态。 在 **GetProcessingRange** 操作调用 CDC 控制任务后就会出现该状态。<br /><br /> 如果使用 **_$reprocessing** 列，该列设置为 1，指示该包可能在重新处理目标上已存在的行。<br /><br /> 有关 CDC 控制任务操作的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。|  
|4-TFEND（滴送-更新-已结束）|这是常规 CDC 运行应该出现的状态。 它指示前一次运行已成功完成，可以开始具有新的处理范围的新一轮运行。|  
|5-TFSTART（滴送-更新-已启动）|这是在 **GetProcessingRange** 操作调用 CDC 控制任务后，后续运行的更新包存在的状态。<br /><br /> 这指示已启动常规 CDC 运行，但运行未完成或未完全结束 ( **MarkProcessedRange** )。<br /><br /> 有关 CDC 控制任务操作的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。|  
|6-TFREDO（重新处理-滴送-更新)|这是在 TFSTART 之后执行 **GetProcessingRange** 时出现的状态。 这指示上次运行未成功完成。<br /><br /> 如果使用 _$reprocessing 列，该列设置为 1，指示该包可能在重新处理目标上已存在的行。|  
|7-ERROR|CDC 组处于 ERROR 状态。|  
  
 这里是 CDC 组件的状态示意图。 当出现意外状态时，就会出现 ERROR 状态。 下图对期望的状态进行了阐释。 但是该图不显示错误状态。  
  
 例如，在初始加载包的末尾，当试图将状态设置为 ILEND 时，如果该状态为 TFSTART，则 CDC 组将处于错误状态，滴送更新包不会运行（但初始加载包会运行）。  
  
 ![状态图](../../integration-services/data-flow/media/statediagram.gif "状态图")  
  
 一旦初始加载包成功运行，滴送更新包就会根据预先确定的计划反复运行，以处理针对源表的更改。 滴送更新包的每次运行都是一次 CDC 运行。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [CDC 源](../../integration-services/data-flow/cdc-source.md)  
  
-   [CDC 拆分器](../../integration-services/data-flow/cdc-splitter.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [根据更改的类型定向 CDC 流](../../integration-services/data-flow/direct-the-cdc-stream-according-to-the-type-of-change.md)  
  
-   [定义状态变量](../../integration-services/data-flow/define-a-state-variable.md)  
  
## <a name="related-content"></a>相关内容  
  
-   mattmasson.com 上的博客文章 [SSIS 中针对 SQL Server 2012 的 CDC](https://www.mattmasson.com/2011/12/cdc-in-ssis-for-sql-server-2012-2/)。  
  
-   blogs.msdn.com 上有关设置 CDC 服务的博客文章 [SQL Server 2012 中 Oracle 的 CDC](https://sqlblog.iridule.net/index.php/2013/04/15/sql-server-2012-cdc-for-oracle-a-review-of-one-implementation/)。  
  
-   social.technet.microsoft.com 上的技术文章 [安装 Microsoft SQL Server 2012 Change Data Capture for Oracle by Attunity](https://go.microsoft.com/fwlink/?LinkId=252958)。  
  
-   social.technet.microsoft.com 上的技术文章 [解决 Microsoft Change Data Capture for Oracle by Attunity 中的配置问题](https://go.microsoft.com/fwlink/?LinkId=252960)。  
  
-   social.technet.microsoft.com 上的技术文章 [解决 Microsoft Change Data Capture for Oracle by Attunity 中的 CDC 实例错误问题](https://go.microsoft.com/fwlink/?LinkId=252961)。  
  
-   technet.microsoft.com 上的视频 [使用 SQL Server Integration Services 2012 时 Oracle 数据库的 CDC（SQL Server 视频）](/previous-versions/dn912438(v=msdn.10))。  
  
## <a name="see-also"></a>另请参阅  
 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)  
  
