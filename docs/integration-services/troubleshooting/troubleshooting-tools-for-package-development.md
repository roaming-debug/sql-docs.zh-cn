---
description: 包开发的故障排除工具
title: 包开发的故障排除工具 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Integration Services packages, troubleshooting
- SSIS packages, troubleshooting
- Integration Services, troubleshooting
- errors [Integration Services], troubleshooting
- packages [Integration Services], troubleshooting
ms.assetid: 41dd248c-dab3-4318-b8ba-789a42d5c00c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e4f59f38a15bf7a703bbdf18277b4e384d1f4c94
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495126"
---
# <a name="troubleshooting-tools-for-package-development"></a>包开发的故障排除工具

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中开发包的过程中可用于对包进行故障排除的功能和工具。  
  
## <a name="troubleshooting-design-time-validation-issues"></a>设计时验证问题故障排除  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的当前版本中，当打开包后，系统将在验证所有数据流组件值之前验证所有连接，并设置速度较慢或无法脱机工作的所有连接。 这有助于减少验证包数据流时的延迟时间。  
  
 打开包后，还可以通过右键单击“连接管理器”区域中的连接管理器并单击“脱机工作”来关闭连接。 这可以在 SSIS 设计器中加快执行操作。  
  
 已设置为脱机工作的连接将保持脱机状态，直到您执行下列操作之一：  
  
-   通过右键单击 SSIS 设计器的“连接管理器”区域中的连接管理器并单击“测试连接”来测试连接。  
  
     例如，当打开包后，连接最初设置为脱机工作。 修改连接字符串以解决该问题，并单击 **“测试连接”** 以测试连接。  
  
-   重新打开包或重新打开包含该包的项目。 将重新对包中的所有连接进行验证。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括以下附加功能，可帮助您避免验证错误：  
  
-   **在数据源不可用时，将所有包和所有连接设置为脱机工作**。 您可以从 **“SSIS”** 菜单启用 **“脱机工作”** 。 与 **DelayValidation** 属性不同， **“脱机工作”** 选项即使在包打开之前也可用。 您还可以启用 **“脱机工作”** 以加快设计器中操作的速度，而仅在需要验证包的时候再禁用此选项。  
  
-   **针对在运行时之前无效的包元素配置 DelayValidation 属性**。 对于其配置在设计时无效的包元素，您可以将 **DelayValidation** 设置为 **True** 以防止出现验证错误。 例如，可以使用只有在运行时通过执行 SQL 任务创建才存在的目标表来执行数据流任务。 可以在包级或包中的各个任务和容器级启用 **DelayValidation** 属性。 在部署包时，对于相同的包元素，通常必须将此属性设置为 **True** ，以防止运行时出现相同的验证错误。  
  
     可以对数据流任务设置 **DelayValidation** 属性，但不能对单个数据流组件设置该属性。 可以通过将单个数据流组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> 属性设置为 **false**。 但是，当此属性的值为 **false** 时，该组件不能识别外部数据源的元数据的更改。  
  
 如果进行验证时包使用的数据库对象被锁定，则验证过程可能会停止响应。 在这些情况下， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器也会停止响应。 可以使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 继续验证过程，以结束 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的关联会话。 还可以使用本节中介绍的设置避免此问题。  
  
## <a name="troubleshooting-control-flow"></a>控制流故障排除  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括以下功能和工具，可用于在包开发过程中对包中的控制流进行故障排除：  
  
-   **对任务、容器和包设置断点**。 可以使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供的图形工具设置断点。 可以在包级或包中的各个任务和容器级启用断点。 某些任务和容器提供用于设置断点的其他中断条件。 例如，可以对 For 循环容器启用一个中断条件，该中断条件在循环的每次迭代开始时挂起执行。  
  
-   **使用调试窗口**。 在运行具有断点的包时， [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中的调试窗口提供对变量值和状态消息的访问。  
  
-   **查看“进度”选项卡中的信息**。[!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供有关在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中运行包时控制流的其他信息。 “进度”选项卡按执行顺序列出任务和容器，而且还包括每个任务和容器及包自身的开始时间和结束时间、警告以及错误消息。  
  
 有关这些功能的详细信息，请参阅 [Debugging Control Flow](../../integration-services/troubleshooting/debugging-control-flow.md)。  
  
## <a name="troubleshooting-data-flow"></a>数据流故障排除  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括以下功能和工具，可用于在包开发过程中对包中的数据流进行故障排除：  
  
-   **仅使用数据的子集进行测试**。 如果只想通过使用一个数据集的示例对包中的数据流进行故障排除，您可以包括百分比抽样转换或行抽样转换，以便在运行时创建一个内联的数据示例。 有关详细信息，请参阅 [Percentage Sampling Transformation](../../integration-services/data-flow/transformations/percentage-sampling-transformation.md) 和 [Row Sampling Transformation](../../integration-services/data-flow/transformations/row-sampling-transformation.md)。  
  
-   **使用数据查看器监视在数据流中移动的数据**。 数据查看器随着数据在源、转换和目标之间的移动显示相关数据值。 数据查看器可以在网格中显示数据。 您可以将数据从数据查看器复制到剪贴板中，然后再将数据粘贴到文件或 Excel 电子表格中。 有关详细信息，请参阅 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md) 中开发包的过程中可用于对包进行故障排除的功能和工具。  
  
-   **对支持错误输出的数据流组件配置错误输出**。 许多数据流的源、转换和目标也支持错误输出。 通过配置数据流组件的错误输出，可以将包含错误的数据定向到不同的目标。 例如，您可以将失败或截断的数据捕获到一个单独的文本文件中。 也可以将数据查看器附加到错误输出，而且只检查错误数据。 在设计时，错误输出捕获错误的数据值，以帮助您开发可高效处理实际数据的包。 不过，其他故障排除工具和功能仅在设计时有用，而错误输出在生产环境中仍然非常有用。 有关详细信息，请参阅 [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。  
  
-   **捕获处理的行数**。 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中运行包时，通过某路径传递的行数将显示在数据流设计器中。 随着数据在路径中的移动，该数量会定期更新。 您还可以将行计数转换添加到数据流中，以捕获变量中的最终行计数。 有关详细信息，请参阅 [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md)。  
  
-   **查看“进度”选项卡中的信息**。[!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供有关在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中运行包时数据流的其他信息。 “进度”选项卡按执行顺序列出数据流组件，而且还包括包的每个阶段的进度（显示为完成百分比）以及写入目标的行数。  
  
 有关这些功能的详细信息，请参阅 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
## <a name="troubleshooting-scripts"></a>脚本故障排除  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 是一种可以用来编写脚本任务和脚本组件所使用脚本的开发环境。 VSTA 提供了可用于排除包开发过程中的脚本故障的以下功能和工具：  
  
-   **在脚本任务的脚本中设置断点。** VSTA 仅对脚本任务中的脚本提供调试支持。 通过综合使用在脚本任务中设置的断点与对包以及包中的任务和容器设置的断点，可以对所有包元素进行无缝调试。  
  
    > [!NOTE]  
    >  当调试一个包含多个脚本任务的包时，调试器将只命中其中一个脚本任务中的断点，而将忽略其他脚本任务中的断点。 如果脚本任务是 Foreach 循环容器或 For 循环容器的一部分，则调试器将在循环的第一次迭代之后忽略脚本任务中的断点。  
  
 有关详细信息，请参阅 [Debugging Script](../../integration-services/troubleshooting/debugging-script.md)。 有关如何调试脚本组件的建议，请参阅 [脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。  
  
## <a name="troubleshooting-errors-without-a-description"></a>无说明错误的故障排除  
 如果在包开发过程中遇到了没有附带说明的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 错误号，可以在 [Integration Services 错误和消息引用](../../integration-services/integration-services-error-and-message-reference.md)中查找说明。 目前该列表中不包括故障排除信息。  
  
## <a name="see-also"></a>另请参阅  
 [对包执行进行故障排除的工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)   
 [数据流性能特点](../../integration-services/data-flow/data-flow-performance-features.md)  
  
  
