---
description: Integration Services (SSIS) 日志记录
title: Integration Services (SSIS) 日志 | Microsoft Docs
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configuredtslogs.containers.f1
- sql13.dts.designer.configuredtslogs.loggingdetails.f1
- sql13.dts.designer.configuredtslogs.providersandlogs.f1
- SQL13.SSIS.SSMS.ISMANAGECUSTOMIZEDLOGGINGLEVEL.F1
helpviewer_keywords:
- containers [Integration Services], logs
- Windows Event log provider [Integration Services]
- XML File log provider [Integration Services]
- SQL Server Profiler, log provider
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- tasks [Integration Services], logs
- logs [Integration Services], log providers
- Integration Services packages, logs
- log providers [Integration Services]
- packages [Integration Services], logs
- Text File log provider
- SQL Server log provider
ms.assetid: 65e17889-371f-4951-9a7e-9932b2d0dcde
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ac0f913188b23b85548f87a4f458d1c2b754d15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347450"
---
# <a name="integration-services-ssis-logging"></a>Integration Services (SSIS) 日志记录

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含可用来在包、容器和任务中执行日志记录的日志提供程序。 通过日志记录可以捕获有关包的运行时信息，从而帮助您在每次运行包时对其进行审核和故障排除。 例如，日志可以捕获运行包的操作员的姓名以及包开始和完成的时间。  
  
 您可以配置在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上执行包的过程中出现的日志记录范围。 有关详细信息，请参阅 [在 SSIS 服务器上启用包执行的日志记录](#server_logging)。  
  
 在使用 **dtexec** 命令提示实用工具运行包时，还可以包括日志记录。 有关支持日志记录的命令提示参数的详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。  
  
## <a name="configure-logging-in-sql-server-data-tools"></a>在 SQL Server Data Tools 中配置日志记录  
 日志与包关联，而且可以在包级进行配置。 包中的每项任务或容器都可以将信息记录到任何包日志中。 即使不对包本身启用日志记录，也可以对包中的任务和容器启用日志记录。 例如，您可以对执行 SQL 任务启用日志记录，而不对父包启用日志记录。 包、容器或任务都可以将信息写入多个日志中。 可以只在包上启用日志记录，也可以选择在包所包括的任何单个任务或容器上启用日志记录。  
  
 将日志添加到包时，请选择日志提供程序和日志的位置。 日志提供程序指定日志数据的格式：例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库或文本文件。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含下列日志提供程序：  
  
-   文本文件日志提供程序，将日志项以逗号分隔值 (CSV) 格式写到 ASCII 文本文件。 这种提供程序的默认文件扩展名是 .log。  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 日志提供程序，写入可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件探查器查看的跟踪。 这种提供程序的默认文件扩展名是 .trc。  
  
    > [!NOTE]  
    >  无法在以 64 位模式运行的包中使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 日志提供程序。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志提供程序，将日志项写入 **数据库中的** sysssislog [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
-   Windows 事件日志提供程序，将日志项写入本地计算机上 Windows 事件日志中的应用程序日志。  
  
-   XML 文件日志提供程序，将日志文件写入 XML 文件。 这种提供程序的默认文件扩展名是 .xml。  
  
 如果要将日志提供程序添加到包或者以编程方式配置日志记录，可以使用 ProgID 或 ClassID 标识日志提供程序，而不必使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器在 **“配置 SSIS 日志”** 对话框中显示的名称。  
  
 下表列出了用于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 所包含日志提供程序的 ProgID 和 ClassID，以及日志提供程序写入的日志的位置。  
  
|日志提供程序|ProgID|ClassID|位置|  
|------------------|------------|-------------|--------------|  
|文本文件|DTS.LogProviderTextFile|{0A039101-ACC1-4E06-943F-279948323883}|日志提供程序使用的文件连接管理器指定此文本文件的路径。|  
|SQL Server Profiler|DTS.LogProviderSQLProfiler|{E93F6300-AE0C-4916-A7BF-A8D0CE12C77A}|日志提供程序使用的文件连接管理器指定 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]所使用的文件的路径。|  
|SQL Server|DTS.LogProviderSQLServer|{94150B25-6AEB-4C0D-996D-D37D1C4FDEDA}|日志提供程序使用的 OLE DB 连接管理器指定包含存储日志项的 sysssislog 表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|Windows 事件日志|DTS.LogProviderEventLog|{071CC8EB-C343-4CFF-8D58-564B92FCA3CF}|Windows 事件查看器中的应用程序日志包含 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 日志信息。|  
|XML 文件|DTS.LogProviderXMLFile|{440945A4-2A22-4F19-B577-EAF5FDDC5F7A}|日志提供程序使用的文件连接管理器指定 XML 文件的路径。|  
  
 也可以创建自定义日志提供程序。 有关详细信息，请参阅 [Creating a Custom Log Provider](../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)。  
  
 包中的日志提供程序是该包的日志提供程序集合的成员。 在使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器创建包并实现日志记录时，您可以在 **设计器的** “包资源管理器” **选项卡上的** “日志提供程序” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 文件夹中看到集合成员列表。  
  
 请提供日志提供程序的名称和说明，并指定日志提供程序使用的连接管理器，以配置日志提供程序。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志提供程序使用 OLE DB 连接管理器。 文本文件、 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]和 XML 文件日志提供程序全都使用文件连接管理器。 Windows 事件日志提供程序不使用连接管理器，因为它直接写入 Windows 事件日志。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) 和 [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)。  
  
### <a name="logging-customization"></a>自定义日志记录  
 为了自定义事件或自定义消息的日志记录， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了要包括在日志项中的常用记录信息的架构。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 日志架构定义您可以记录的信息。 您可以从日志架构中为每个日志项选择元素。  
  
 包及其容器和任务不必记录相同的信息，同一包或容器内的任务可以记录不同的信息。 例如，包可以在包启动时记录操作员信息，一个任务可以记录任务失败的原因，而另一个任务可以在出现错误时记录信息。 如果包及其容器和任务使用多个日志，则相同信息会写入所有日志中。  
  
 通过指定要记录的事件以及要为每个事件记录的信息，您可以选择满足自己需求的日志记录级别。 您可能会发现一些事件提供的信息更为有用。 例如，对于 **PreExecute** 事件，您希望仅记录计算机名和操作员姓名，而对于 **Error** 事件，您希望记录所有可用的信息。  
  
 若要防止日志文件占用大量的磁盘空间或者避免过多的日志记录（这可能会降低性能），可以通过选择记录特定的事件和信息项来限制日志记录。 例如，您可以将日志配置为仅捕获每个错误的日期和计算机名。  
  
 在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，您可以使用 **“配置 SSIS 日志”** 对话框来定义日志记录选项。  
  
#### <a name="log-schema"></a>日志架构  
 下表介绍了日志架构中的元素：  
  
|元素|说明|  
|-------------|-----------------|  
|Computer|发生日志事件的计算机的名称。|  
|操作员|启动包的用户的标识。|  
|SourceName|发生日志事件的容器或任务的名称。|  
|SourceID|发生日志事件的包、 For 循环容器、Foreach 循环容器、序列容器或任务的唯一标识符。|  
|ExecutionID|包执行实例的 GUID。<br /><br /> 注意：运行单个包可能会创建具有不同的 ExecutionID 元素的值的日志条目。 例如，当在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中运行包时，验证阶段可能会创建 ExecutionID 元素与 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]对应的日志项。 但是，执行阶段可能会创建 ExecutionID 元素与 dtshost.exe 对应的日志项。 再比如，当运行包含“执行包”任务的包时，这些任务中的每个任务都会运行子包。 这些子包创建的日志项所具有的 ExecutionID 元素可能不同于父包创建的日志项。|  
|MessageText|与日志项关联的消息。|  
|DataBytes|日志项特定的字节数组。 此字段的意义因日志项的不同而不同。|  
  
 下表介绍日志架构中三个附加元素，这些元素在 **“配置 SSIS 日志”** 对话框的 **“详细信息”** 选项卡中不可用。  
  
|元素|说明|  
|-------------|-----------------|  
|StartTime|容器或任务开始运行的时间。|  
|EndTime|容器或任务停止运行的时间。|  
|DataCode|可选整数值通常包含 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 枚举值，该枚举值指示运行该容器或任务的结果：<br /><br /> 0 – 成功<br /><br /> 1 - 失败<br /><br /> 2 – 已完成<br /><br /> 3 – 已取消|  
  
#### <a name="log-entries"></a>日志项  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 支持预定义事件的日志项，并提供了可用于很多 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象的自定义日志项。 **设计器中的** “配置 SSIS 日志” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 对话框列出了这些事件和自定义日志项。  
  
 下表描述了可以在发生运行时事件时启用日志项写入功能的预定义事件。 这些日志项将应用到可执行文件、包以及包中的任务和容器。 日志项的名称与引发并导致写入日志项的运行时事件的名称相同。  
  
|事件|说明|  
|------------|-----------------|  
|**OnError**|出现错误时写入日志项。|  
|**OnExecStatusChanged**|在提示期间挂起或恢复任务（而非容器时），写入日志项。|  
|**OnInformation**|在验证和执行可执行文件的过程中写入报告信息的日志项。|  
|**OnPostExecute**|在可执行文件运行完成后立即写入日志项。|  
|**OnPostValidate**|在可执行文件的验证完成时写入日志项。|  
|**OnPreExecute**|在可执行文件即将运行前写入日志项。|  
|**OnPreValidate**|可执行文件的验证开始时写入日志项。|  
|**OnProgress**|在可执行文件的进度可度量时写入日志项。|  
|**OnQueryCancel**|在任务处理过程中可以取消执行的任何时刻写入日志项。|  
|**OnTaskFailed**|在任务失败时写入日志项。|  
|**OnVariableValueChanged**|在变量的值更改时写入日志项。|  
|**OnWarning**|在出现警告时写入日志项。|  
|**PipelineComponentTime**|对于每个数据流组件，为验证和执行的每个阶段写入日志项。 该日志条目为每个阶段指定处理时间。|  
|**诊断**<br /><br /> **DiagnosticEx**|写入提供诊断信息的日志项。<br /><br /> 例如，您可以在每次调用外部数据访问接口之前和之后记录消息。 有关详细信息，请参阅 [包执行的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)。<br /><br /> 当你想查找数据流中存在错误的列的列名称时，请记录 **DiagnosticEx** 事件。 此事件将数据流沿袭映射写入到日志中。 然后就可以使用由错误输出捕获的列标识符来查找此沿袭映射中的列名称。 有关详细信息，请参阅[数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。<br /><br /> 请注意，为了缩减日志大小， **DiagnosticEx** 事件不在其 XML 输出中保留空白。 若要提高可读性，请将日志复制到支持 XML 格式和语法突出显示的 XML 编辑器中 - 例如 Visual Studio 中的 XML 编辑器。<br /><br /> 注意：如果你使用 SQL Server 日志提供程序记录 **DiagnosticEx** ，输出可能被截断。 SQL Server 日志提供程序的 **消息** 字段属于 nvarchar(2048) 类型。 若要避免截断，请在记录 **DiagnosticEx** 事件时使用其他日志提供程序。|  
  
 包和很多任务都有可以启用日志记录功能的自定义日志项。 例如，发送邮件任务提供了 **SendMailTaskBegin** 自定义日志项，该日志项在发送邮件任务开始运行时（但在发送电子邮件消息之前）记录信息。 有关详细信息，请参阅 [Custom Messages for Logging](#custom_messages)。  
  
### <a name="differentiating-package-copies"></a>区分包副本  
 日志数据包括日志项所属的包的名称和 GUID。 如果通过复制现有包而创建了新的包，则现有包的名称和 GUID 也会被复制。 结果，可能有两个包具有相同的 GUID 和名称，这将使您难以区分日志数据中的包。  
  
 若要消除这种不明确性，应当更新新包的名称和 GUID。 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，可以在 **ID** 属性中重新生成 GUID，并在“属性”窗口中更新 **Name** 属性的值。 还可以通过编程或使用 **dtutil** 命令提示符工具来更改 GUID 和名称。 有关详细信息，请参阅 [设置包属性](../../integration-services/set-package-properties.md) 和 [dtutil 实用工具](../../integration-services/dtutil-utility.md)。  
  
### <a name="parent-logging-options"></a>父日志记录选项  
 通常，任务以及 For 循环、Foreach 循环和序列容器的日志记录选项与包或父容器的日志记录选项匹配。 在这种情况下，您可以将它们配置为继承其父容器的日志记录选项。 例如，在包括执行 SQL 任务的 For 循环容器中，执行 SQL 任务可以使用已对 For 循环容器设置的日志记录选项。 若要使用父日志记录选项，可以将容器的 LoggingMode 属性设置为 **UseParentSetting**。 可以在 **的** “属性” [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 窗口中设置此属性，也可以通过 **设计器中的** “配置 SSIS 日志” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 对话框设置此属性。  
  
### <a name="logging-templates"></a>日志记录模板  
 在 **“配置 SSIS 日志”** 对话框中，还可以创建经常使用的日志记录配置并将其保存为模板，然后在多个包中使用这些模板。 这样便于对多个包应用一致的日志记录策略，以及通过更新并应用模板来修改包的日志设置。 这些模板存储为 XML 文件。  
  
 **使用“配置 SSIS 日志”对话框配置日志记录**  
  
1.  为包及其任务启用日志记录。 可以在包级、容器级以及任务级进行日志记录。 可以为包、容器和任务指定不同的日志。  
  
2.  选择日志提供程序并为包添加日志。 可以仅在包级创建日志，任务或容器必须使用为包创建的日志之一。 每个日志都与下列某个日志提供程序关联：文本文件、 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、Windows 事件日志或 XML 文件。 有关详细信息，请参阅 [在 SQL Server Data Tools 中启用包日志记录](#ssdt)。  
  
3.  选择要在日志中捕获的事件以及每个事件的日志架构信息。 有关详细信息，请参阅 [使用保存的配置文件配置日志记录](#saved_config)。  
  
### <a name="configuration-of-log-provider"></a>日志提供程序的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 作为在包中实现日志记录的一个步骤来创建和配置日志提供程序。  
  
 创建日志提供程序后，可以在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的“属性”窗口中查看和修改其属性。  
  
 有关如何以编程方式设置这些属性的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> 类的文档。  
  
### <a name="logging-for-data-flow-tasks"></a>数据流任务的日志记录  
 数据流任务提供了许多可用于监视和调整性能的自定义日志项。 例如，您可以监视可能会导致内存泄漏的组件，或者跟踪特定组件运行所用的时间。 有关这些自定义日志项的列表和日志记录输出示例，请参阅 [Data Flow Task](../../integration-services/control-flow/data-flow-task.md)。  
  
#### <a name="capture-the-names-of-columns-in-which-errors-occur"></a>捕获在其中发生错误的列的名称  
 当在数据流中配置错误输出时，默认情况下错误输出仅提供在其中发生错误的列的数字标识符。 有关详细信息，请参阅[数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。  
  
 可以通过启用日志记录并选择 **DiagnosticEx** 事件来查找列名称。 此事件将数据流沿袭映射写入到日志中。 然后可以在此沿袭映射中从其标识符查找列名称。 请注意，为了缩减日志大小， **DiagnosticEx** 事件不在其 XML 输出中保留空白。 若要提高可读性，请将日志复制到支持 XML 格式和语法突出显示的 XML 编辑器中 - 例如 Visual Studio 中的 XML 编辑器。  
  
#### <a name="use-the-pipelinecomponenttime-event"></a>使用 PipelineComponentTime 事件  
 最有用的自定义日志项可能是 PipelineComponentTime 事件。 该日志项报告数据流中的每个组件执行五个主要处理步骤中的每个步骤所用的毫秒数。 下表说明了这些处理步骤。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 开发人员会将这些步骤标识为 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>。  
  
|步骤|说明|  
|----------|-----------------|  
|验证|该组件查看有效的属性值和配置设置。|  
|PreExecute|该组件在开始处理数据行之前执行一次性处理。|  
|PostExecute|该组件在处理所有数据行之后执行一次性处理。|  
|ProcessInput|转换或目标组件处理由上游源或转换传递的传入数据行。|  
|PrimeOutput|源或转换组件填充数据缓冲区，以传递给下游转换或目标组件。|  
  
 启用 PipelineComponentTime 事件时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将针对每个组件执行的各处理步骤记录一则消息。 以下日志项显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] CalculatedColumns 包示例记录的消息的子集。  
  
 `The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`  
  
 `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`  
  
 `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`  
  
 `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`  
  
 `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`  
  
 `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`  
  
 下列日志项显示数据流任务在下列步骤中消耗了大多数时间，如下所示（按降序排序）：  
  
-   名为“Extract Data”的 OLE DB 源在加载数据期间耗时 688 毫秒 加载数据。  
  
-   名为“Calculate LineItemTotalCost”的派生列转换 在对传入行执行计算期间耗时 356 毫秒。  
  
-   名为“Sum Quantity and LineItemTotalCost”的聚合转换在执行计算和将数据传递到下一转换期间共耗时 220 毫秒，其中有 141 毫秒用于 PrimeOutput，有 79 毫秒用于 ProcessInput。  

## <a name="enable-package-logging-in-sql-server-data-tools"></a><a name="ssdt"></a> 在 SQL Server Data Tools 中启用包日志记录
  本过程介绍如何将日志添加到包中，如何配置包级日志记录，以及如何将日志记录配置保存为 XML 文件。 您只能在包级添加日志，但包不必执行日志记录，就可以在包所包括的容器中启用日志记录。  
  
> [!IMPORTANT]  
>  如果您将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器，则为包执行设置的日志记录级别优先于使用 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]配置的包日志记录。  
  
 默认情况下，包中的容器与其父容器使用相同的日志记录配置。 有关为各个容器设置日志记录选项的信息，请参阅 [使用保存的配置文件配置日志记录](#saved_config)。  
  
### <a name="to-enable-logging-in-a-package"></a>在包中启用日志记录  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在 **SSIS** 菜单上，单击 **“日志记录”** 。  
  
3.  在 **“提供程序类型”** 列表中，选择一个日志提供程序，然后单击 **“添加”** 。  
  
4.  在“配置”列中，选择连接管理器或单击“\<New connection>”为日志提供程序新建一个适当类型的连接管理器 。 根据所选提供程序的不同，可以使用下列某个连接管理器：  
  
    -   对于文本文件，请使用文件连接管理器。 有关详细信息，请参阅 [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
    -   对于 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]，请使用文件连接管理器。  
  
    -   对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，请使用 OLE DB 连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
    -   对于 Windows 事件日志，无需执行任何操作。 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 会自动创建日志。  
  
    -   对于 XML 文件，请使用文件连接管理器。  
  
5.  对于要在包中使用的每个日志，请重复步骤 3 和步骤 4。  
  
    > [!NOTE]  
    >  一个包可以使用多个同一类型的日志。  
  
6.  还可以选中包级复选框，选择要用于包级日志记录的日志，然后单击“详细信息”  选项卡。  
  
7.  在 **“详细信息”** 选项卡上，选择 **“事件”** 将记录所有日志项，清除 **“事件”** 可以选择单个事件。  
  
8.  还可以单击 **“高级”** 指定要记录的信息。  
  
    > [!NOTE]  
    >  默认情况下会记录所有信息。  
  
9. 在“详细信息”  选项卡上，单击“保存”  。 将显示 **“另存为”** 对话框。 找到要将日志记录配置保存到的文件夹，为新的日志配置键入文件名，然后单击 **“保存”** 。  
  
10. 单击“确定”。  
  
11. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="configure-ssis-logs-dialog-box"></a><a name="configure_logs"></a> “配置 SSIS 日志”对话框
  使用 **“配置 SSIS 日志”** 对话框可以定义包的日志记录选项。  
  
 **您希望做什么？**  
  
1.  [打开“配置 SSIS 日志”对话框](#open_dialog)  
  
2.  [配置“容器”窗格中的选项](#container)  
  
3.  [配置“提供程序和日志”选项卡上的选项](#provider)  
  
4.  [配置“详细信息”选项卡上的选项](#detail)  
  
###  <a name="open-the-configure-ssis-logs-dialog-box"></a><a name="open_dialog"></a> 打开“配置 SSIS 日志”对话框  
 **打开“配置 SSIS 日志”对话框**  
  
-   在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的 **SSIS** 菜单上，单击 **“日志记录”** 。  
  
###  <a name="configure-the-options-in-the-containers-pane"></a><a name="container"></a> 配置“容器”窗格中的选项  
 可以使用 **“配置 SSIS 日志”** 对话框的 **“容器”** 窗格，为包及其容器启用日志记录。  
  
#### <a name="options"></a>选项  
 **容器**  
 选中层次结构视图中的该复选框可以为日志记录启用包和包容器：  
  
-   如果清除此复选框，则不会为日志记录启用容器。 选中此复选框将启用日志记录。  
  
-   如果此复选框为灰色，则容器将使用其父容器的日志记录选项。 此选项对包不可用。  
  
-   如果选中此复选框，那么容器将定义自己的日志记录选项。  
  
 如果容器为灰色，而您想要设置该容器的日志记录选项，请单击对应复选框两次。 第一次单击将清除该复选框，第二次单击将选中该复选框，这样您就可以选择要使用的日志提供程序以及要记录的信息。  
  
###  <a name="configure-the-options-on-the-providers-and-logs-tab"></a><a name="provider"></a> 配置“提供程序和日志”选项卡上的选项  
 可以使用“配置 SSIS 日志”对话框的“提供程序和日志”选项卡，创建和配置用于捕获运行时事件的日志。  
  
#### <a name="options"></a>选项  
 **提供程序类型**  
 从列表中选择日志提供程序的类型。  
  
 **添加**  
 将指定类型的日志添加到包的日志提供程序集合中。  
  
 **名称**  
 通过使用复选框，可以为在“配置 SSIS 日志”对话框的“容器”窗格中选择的容器或任务启用或禁用日志。 名称字段是可编辑的。 可以使用提供程序的默认名称，也可以键入唯一的描述性名称。  
  
 **说明**  
 说明字段是可编辑的。 可以单击该字段，然后修改日志的默认说明。  
  
 **配置**  
 从列表中选择现有的连接管理器，或单击“\<**New connection...**>”创建新的连接管理器。 根据日志提供程序的类型，您可以配置 OLE DB 连接管理器或文件连接管理器。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 事件日志的日志提供程序不需要任何连接。  
  
 相关主题： [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) 、 [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **删除**  
 选择一个日志提供程序，然后单击“删除”  。  
  
###  <a name="configure-the-options-on-the-details-tab"></a><a name="detail"></a> 配置“详细信息”选项卡上的选项  
 可以使用 **“配置 SSIS 日志”** 对话框的 **“详细信息”** 选项卡，指定要启用日志记录的事件以及要记录的详细信息。 所选的信息适用于包中的所有日志提供程序。 例如，无法将一些信息写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，而在文本文件中写入另外一些信息。  
  
#### <a name="options"></a>选项  
 **事件**  
 为事件启用或禁用日志记录功能。  
  
 **说明**  
 查看事件的说明。  
  
 **高级**  
 选中或清除要记录的事件，以及选中或清除要为每个事件记录的信息。 单击 **“基本”** 可以隐藏除事件列表之外的所有日志记录详细信息。 日志记录可以包含以下信息：  
  
|值|说明|  
|-----------|-----------------|  
|**计算机**|发生所记录事件的计算机的名称。|  
|**“运算符”**|启动包的人员的用户名。|  
|**SourceName**|发生所记录事件的包、容器或任务的名称。|  
|**SourceID**|发生所记录事件的包、容器或任务的全局唯一标识符 (GUID)。|  
|**ExecutionID**|包执行实例的全局唯一标识符。|  
|**MessageText**|与日志项关联的消息。|  
|**DataBytes**|保留供将来使用。|  
  
 **基本**  
 选中或清除要记录的事件。 选择此选项将隐藏除事件列表之外的日志记录详细信息。 默认情况下，如果选择某事件，则会为该事件选择所有日志记录详细信息。 单击 **“高级”** 将显示所有日志记录详细信息。  
  
 **加载**  
 指定要用作设置日志记录选项的模板的现有 XML 文件。  
  
 **保存**  
 将配置详细信息以模板形式保存到 XML 文件。  

## <a name="configure-logging-by-using-a-saved-configuration-file"></a><a name="saved_config"></a> 使用保存的配置文件配置日志记录
  本过程介绍如何通过加载以前保存的日志记录配置文件来为包中的新容器配置日志记录。  
  
 默认情况下，包中的所有容器与其父容器使用相同的日志记录配置。 例如，Foreach 循环中的任务使用与 Foreach 循环相同的日志记录配置。  
  
### <a name="to-configure-logging-for-a-container"></a>为容器配置日志记录  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在 **SSIS** 菜单上，单击 **“日志记录”** 。  
  
3.  展开包的树视图，并选择要配置的容器。  
  
4.  在 **“提供程序和日志”** 选项卡上，选择要用于该容器的日志。  
  
    > [!NOTE]  
    >  只能在包级创建日志。 有关详细信息，请参阅 [在 SQL Server Data Tools 中启用包日志记录](#ssdt)。  
  
5.  单击 **“详细信息”** 选项卡，单击 **“加载”** 。  
  
6.  找到要使用的日志记录配置文件，并单击 **“打开”** 。  
  
7.  （可选）通过在 **“事件”** 列中选中其复选框，选择要记录的其他日志项。 单击 **“高级”** 可以选择要为此项记录的信息类型。  
  
    > [!NOTE]  
    >  新容器可以包含最初用于创建日志记录配置的容器中所没有的其他日志项。 您必须手动选择这些其他日志项，才能对它们进行记录。  
  
8.  若要保存日志记录配置的更新后的版本，请单击 **“保存”** 。  
  
9. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="enable-logging-for-package-execution-on-the-ssis-server"></a><a name="server_logging"></a>在 SSIS 服务器上启用包执行的日志记录
  本主题介绍在运行已经部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包时，如何设置或更改该包的日志记录级别。 在运行包时设置的日志记录级别将覆盖你在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中设计时配置的包日志记录。 有关详细信息，请参阅 [在 SQL Server Data Tools 中启用包日志记录](#ssdt) 。  
  
 在 SQL Server“服务器属性”中，“服务器日志记录级别”属性下，可以选择默认服务器范围内的日志记录级别。 可以从本主题中介绍的内置日志记录中选择一项，或者选择现有的自定义日志记录级别。 默认情况下，所选的日志记录级别适用于部署到 SSIS 目录的所有包。 同时也默认适用于运行 SSIS 包的 SQL 代理作业步骤。  
  
 此外，也可使用下列方法之一对单个包指定日志记录级别。 本主题涵盖第一种方法。  
  
-   通过使用“执行包”对话框配置包执行的实例  
  
-   通过使用 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)设置执行实例的参数  
  
-   通过使用“新建作业步骤”对话框配置包执行的 SQL Server 代理作业。  
  
### <a name="set-the-logging-level-for-a-package-by-using-the-execute-package-dialog-box"></a>通过使用“执行包”对话框设置包的日志记录级别  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，导航到对象资源管理器中的包。  
  
2.  右键单击包，然后选择“执行”。  
  
3.  在 **“执行包”** 对话框中，选择 **“高级”** 选项卡。  
  
4.  在 **“日志记录级别”** 下，选择日志记录级别。 本主题包含可用值的说明。  
  
5.  完成所有其他包配置，然后单击 **“确定”** 运行该包。  
  
### <a name="select-a-logging-level"></a>选择日志记录级别  
 以下内置日志记录级别可用。 还可以选择现有的自定义日志记录级别。 本主题包含自定义日志记录级别的说明。  
  
|“日志记录级别”|说明|  
|-------------------|-----------------|  
|无|关闭日志记录。 仅记录包执行状态。|  
|基本|除了自定义事件和诊断事件之外，记录其余所有事件。 这是默认值。|  
|运行时沿袭|收集跟踪数据流中的沿袭信息所需的数据。 可以分析此沿袭信息以映射任务之间的沿袭关系。 使用此信息，ISV 和开发人员可以构建自定义沿袭映射工具。|  
|性能|仅记录性能统计信息、OnError 和 OnWarning 事件。<br /><br /> **“执行性能”** 报表显示包数据流组件的活动时间和总时间。 仅当上次包执行的日志记录级别设置为 **“性能”** 或 **“详细”** 时，此信息才可用。 有关详细信息，请参阅 [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)。<br /><br /> [catalog.execution_component_phases](../../integration-services/system-views/catalog-execution-component-phases.md) 视图显示数据流组件在执行的每个阶段的开始时间和结束时间。 仅当包执行的日志记录级别设置为 **“性能”** 或 **“详细”** 时，此视图才会为这些组件显示以上信息。|  
|“详细”|记录所有事件，包括自定义事件和诊断事件。<br /><br /> 自定义事件包括 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 任务记录的那些事件。 有关自定义事件的详细信息，请参阅 [Custom Messages for Logging](#custom_messages)。<br /><br /> 诊断事件的一个例子就是“DiagnosticEx”  事件。 每当执行包任务执行子包时，此事件均将捕获传递给子包的参数值。<br /><br /> “DiagnosticEx”  事件还有助于获取其中出现行级错误的列的名称。 此事件将数据流沿袭映射写入到日志中。 然后就可以使用由错误输出捕获的列标识符来查找此沿袭映射中的列名称。  有关详细信息，请参阅[数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。<br /><br /> **DiagnosticEx** 的消息列的值是 XML 文本。 若要查看包执行的消息文本，请查询 [catalog.operation_messages（SSISDB 数据库）](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)视图。 请注意，为了缩减日志大小， **DiagnosticEx** 事件不在其 XML 输出中保留空白。 若要提高可读性，请将日志复制到支持 XML 格式和语法突出显示的 XML 编辑器中 - 例如 Visual Studio 中的 XML 编辑器。<br /><br /> 每当数据流组件向下游组件发送数据时， [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) 视图就会显示一行。 日志记录级别必须设置为 **“详细”** ，才能在该视图中捕获此信息。|  
  
### <a name="create-and-manage-customized-logging-levels-by-using-the-customized-logging-level-management-dialog-box"></a>使用“自定义日志记录级别管理”对话框来创建和管理自定义日志记录级别  
 可以创建只收集想要的统计信息和事件的自定义日志记录级别。 还可以选择捕获事件上下文，包括变量值、连接字符串和组件属性。 运行包时，可在任何可以选择内置日志记录级别的位置处，选择自定义日志记录级别。  
  
> [!TIP]  
>  若要捕获包变量的值，必须将该变量的 **IncludeInDebugDump** 属性设置为 **True**。  
  
1.  要创建和管理自定义日志记录级别，在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，右键单击 SSISDB 数据库并选择“自定义日志记录级别”以打开“自定义日志记录级别管理”对话框。 “自定义日志记录级别”  列表包含所有现有的自定义日志记录级别。  
  
2.  要 **创建** 新的自定义日志记录级别，请单击“创建”  ，然后提供名称和描述。 在“统计信息”  和“事件”  选项卡上，选择想要收集的统计信息和事件。 在“事件”  选项卡上，可以选择对单个事件选择“包括上下文”  。 然后单击“保存”  。  
  
3.  要 **更新** 现有的自定义日志记录级别，请在列表中选中并重新配置它，然后单击“保存”  。  
  
4.  要 **删除** 现有的自定义日志记录级别，请在列表中选中它，然后单击“删除”  。  
  
 **自定义日志记录级别的权限。**  
  
-   SSISDB 数据库的所有用户都可以查看自定义日志记录级别，并可在运行包时选择一个自定义日志记录级别。  
  
-   只有 ssis_admin 或 sysadmin 角色中的用户可以创建、更新或删除自定义日志记录级别。  

## <a name="custom-messages-for-logging"></a><a name="custom_messages"></a> Custom Messages for Logging
SQL Server Integration Services 提供了一组丰富的自定义事件，可以用来写入包和很多任务的日志项。 使用这些项可以记录预定义的事件或用户定义的消息，供随后分析时使用，从而将有关执行进度、结果和问题的详细信息保存下来。 例如，可以记录大容量插入的开始和结束时间，从而找出包运行时的性能问题。  
  
 与对包和所有容器及任务可用的标准日志记录事件集相比，自定义日志项是一组不同的项。 您可以根据需要使用自定义日志项来捕获有关包中特定任务的有用信息。 例如，执行 SQL 任务的一个自定义日志项可以在日志中记录该任务所执行的 SQL 语句。  
  
 所有日志项都包括日期和时间信息，包括在包开始和完成时自动写入的日志项。 很多日志事件都会在日志中写入多个项。 这通常发生在事件有不同阶段时。 例如， **ExecuteSQLExecutingQuery** 日志事件写入三项：在任务获得与数据库的连接之后写入一项，在任务开始准备 SQL 语句之后写入另一项，并在执行完 SQL 语句之后再写入一项。  
  
 以下 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象有自定义日志项：  
  
 [包](#Package)  
  
 [大容量插入任务](#BulkInsert)  
  
 [数据流任务](#DataFlow)  
  
 [执行 DTS 2000 任务](#ExecuteDTS200)  
  
 [执行进程任务](#ExecuteProcess)  
  
 [执行 SQL 任务](#ExecuteSQL)  
  
 [文件系统任务](#FileSystem)  
  
 [FTP 任务](#FTP)  
  
 [消息队列任务](#MessageQueue)  
  
 [脚本任务](#Script)  
  
 [发送邮件任务](#SendMail)  
  
 [传输数据库任务](#TransferDatabase)  
  
 [传输错误消息任务](#TransferErrorMessages)  
  
 [传输作业任务](#TransferJobs)  
  
 [传输登录名任务](#TransferLogins)  
  
 [传输主存储过程任务](#TransferMasterStoredProcedures)  
  
 [传输 SQL Server 对象任务](#TransferSQLServerObjects)  
  
 [Web 服务任务](#WebServices)  
  
 [WMI 数据读取器任务](#WMIDataReader)  
  
 [WMI 事件观察器任务](#WMIEventWatcher)  
  
 [XML 任务](#XML)  
  
### <a name="log-entries"></a>日志项  
  
####  <a name="package"></a><a name="Package"></a> “包”  
 下表列出了包的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**PackageStart**|指示包开始运行。 此日志项自动写入日志。 无法排除它。|  
|**PackageEnd**|指示包已完成。 此日志项自动写入日志。 无法排除它。|  
|**诊断**|提供影响包执行的系统配置的相关信息，例如，可并发运行的可执行文件数。<br /><br /> **Diagnostic** 日志项还包括调用外部数据访问接口之前和之后的项。|  
  
####  <a name="bulk-insert-task"></a><a name="BulkInsert"></a> 大容量插入任务  
 下表列出了大容量插入任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**DTSBulkInsertTaskBegin**|指示大容量插入开始。|  
|**DTSBulkInsertTaskEnd**|指示大容量插入完成。|  
|**DTSBulkInsertTaskInfos**|提供有关任务的说明性信息。|  
  
####  <a name="data-flow-task"></a><a name="DataFlow"></a> Data Flow Task  
 下表列出了数据流任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**BufferSizeTuning**|指示数据流任务更改了缓冲区的大小。 日志条目描述了大小更改的原因，并列出了临时的新缓冲区大小。|  
|**OnPipelinePostEndOfRowset**|表示组件已经给出它的行集结束信号，该信号由对 **ProcessInput** 方法的最后一次调用设置。 对于数据流中处理输入的每个组件，都会写入一项。 该项包括组件的名称。|  
|**OnPipelinePostPrimeOutput**|指示组件已经完成它对 **PrimeOutput** 方法的最后一次调用。 取决于数据流，可能写入多个日志条目。 如果组件是源组件，这意味着该组件已经完成对行的处理。|  
|**OnPipelinePreEndOfRowset**|指示组件将要接收它的行集结束信号，该信号由对 **ProcessInput** 方法的最后一次调用设置。 对于数据流中处理输入的每个组件，都会写入一项。 该项包括组件的名称。|  
|**OnPipelinePrePrimeOutput**|指示组件将从 **PrimeOutput** 方法接收它的调用。 取决于数据流，可能写入多个日志条目。|  
|**OnPipelineRowsSent**|报告对 **ProcessInput** 方法的调用为组件输入所提供的行数。 此日志条目包括组件名。|  
|**PipelineBufferLeak**|提供在缓冲区管理器退出之后使缓冲区保持活动状态的任何组件的相关信息。 这意味着缓冲区资源未释放，并且可能导致内存泄漏。 日志条目提供组件的名称和缓冲区的 ID。|  
|**PipelineExecutionPlan**|报告数据流的执行计划。 它提供有关缓冲区将如何发送到组件的信息。 此信息与 PipelineExecutionTrees 项组合，一起描述在任务中所发生的事情。|  
|**PipelineExecutionTrees**|报告数据流中的布局的执行树。 数据流引擎的计划程序使用这些树生成数据流的执行计划。|  
|**PipelineInitialization**|提供有关任务的初始化信息。 此信息包括要用来临时存储 BLOB 数据、默认缓冲区大小和缓冲区行数的目录。 取决于数据流任务的配置，可能写入多个日志条目。|  
  
####  <a name="execute-dts-2000-task"></a><a name="ExecuteDTS200"></a> 执行 DTS 2000 任务  
 下表列出了执行 DTS 2000 任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ExecuteDTS80PackageTaskBegin**|指示任务开始运行 DTS 2000 包。|  
|**ExecuteDTS80PackageTaskEnd**|指示任务已完成。<br /><br /> 注意：任务结束之后，DTS 2000 包可能继续运行。|  
|**ExecuteDTS80PackageTaskTaskInfo**|提供有关任务的说明性信息。|  
|**ExecuteDTS80PackageTaskTaskResult**|报告该任务所运行的 DTS 2000 包的执行结果。|  
  
####  <a name="execute-process-task"></a><a name="ExecuteProcess"></a> 执行进程任务  
 下表列出了执行进程任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|提供有关进程的信息，该进程运行此任务按配置要求要运行的可执行文件。<br /><br /> 写入两个日志条目。 一个日志项包含有关任务所运行的可执行文件的名称和位置的信息，另一个则记录从可执行文件退出的信息。|  
|**ExecuteProcessVariableRouting**|提供有关哪些变量被路由到可执行文件的输入和输出的信息。 将为 stdin（输入）、stdout（输出）和 stderr（错误输出）写入日志条目。|  
  
####  <a name="execute-sql-task"></a><a name="ExecuteSQL"></a> 执行 SQL 任务  
 下表介绍了执行 SQL 任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|提供有关 SQL 语句的执行阶段的信息。 在任务获得与数据库的连接时、任务开始准备 SQL 语句时以及执行完 SQL 语句之后写入日志项。 准备阶段的日志条目包括任务所使用的 SQL 语句。|  
  
####  <a name="file-system-task"></a><a name="FileSystem"></a> 文件系统任务  
 下表介绍了文件系统任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**FileSystemOperation**|报告任务所执行的操作。 在文件系统操作开始时写入日志项，日志项包括有关源和目标的信息。|  
  
####  <a name="ftp-task"></a><a name="FTP"></a> FTP 任务  
 下表列出了 FTP 任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**FTPConnectingToServer**|指示任务已启动与 FTP 服务器的连接。|  
|**FTPOperation**|报告任务所执行的 FTP 操作的开始及其类型。|  
  
####  <a name="message-queue-task"></a><a name="MessageQueue"></a> 消息队列任务  
 下表列出了消息队列任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**MSMQAfterOpen**|指示任务已完成打开消息队列的操作。|  
|**MSMQBeforeOpen**|指示任务开始打开消息队列。|  
|**MSMQBeginReceive**|指示任务开始接收消息。|  
|**MSMQBeginSend**|指示任务开始发送消息。|  
|**MSMQEndReceive**|指示任务完成接收消息。|  
|**MSMQEndSend**|指示任务完成发送消息的操作。|  
|**MSMQTaskInfo**|提供有关任务的说明性信息。|  
|**MSMQTaskTimeOut**|指示任务已超时。|  
  
####  <a name="script-task"></a><a name="Script"></a> 脚本任务  
 下表介绍了脚本任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|报告在脚本中实现日志记录的结果。 在每次调用 **Log** 对象的 **Dts** 方法时将写入日志项。 代码运行时将写入日志项。 有关详细信息，请参阅 [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md)。|  
  
####  <a name="send-mail-task"></a><a name="SendMail"></a> 发送邮件任务  
 下表列出了发送邮件任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**SendMailTaskBegin**|指示任务开始发送电子邮件。|  
|**SendMailTaskEnd**|指示任务已发送完电子邮件。|  
|**SendMailTaskInfo**|提供有关任务的说明性信息。|  
  
####  <a name="transfer-database-task"></a><a name="TransferDatabase"></a> 传输数据库任务  
 下表列出了传输数据库任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**SourceDB**|指定任务所复制的数据库。|  
|**SourceSQLServer**|指定从中复制数据库的计算机。|  
  
####  <a name="transfer-error-messages-task"></a><a name="TransferErrorMessages"></a> 传输错误消息任务  
 下表列出了传输错误消息任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**TransferErrorMessagesTaskFinishedTransferringObjects**|指示任务已完成传输错误消息的操作。|  
|**TransferErrorMessagesTaskStartTransferringObjects**|指示任务已开始传输错误消息。|  
  
####  <a name="transfer-jobs-task"></a><a name="TransferJobs"></a> 传输作业任务  
 下表列出传输作业任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**TransferJobsTaskFinishedTransferringObjects**|指示任务已完成传输 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|  
|**TransferJobsTaskStartTransferringObjects**|指示任务已开始传输 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|  
  
####  <a name="transfer-logins-task"></a><a name="TransferLogins"></a> 传输登录名任务  
 下表列出了传输登录名任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**TransferLoginsTaskFinishedTransferringObjects**|指示任务已传输完登录名。|  
|**TransferLoginsTaskStartTransferringObjects**|指示任务已开始传输登录名。|  
  
####  <a name="transfer-master-stored-procedures-task"></a><a name="TransferMasterStoredProcedures"></a> 传输主存储过程任务  
 下表列出可传输主存储过程任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**TransferStoredProceduresTaskFinishedTransferringObjects**|指示任务已传输完在 **master** 数据库中存储的用户定义的存储过程。|  
|**TransferStoredProceduresTaskStartTransferringObjects**|指示任务已开始传输 **master** 数据库中存储的用户定义的存储过程。|  
  
####  <a name="transfer-sql-server-objects-task"></a><a name="TransferSQLServerObjects"></a> 传输 SQL Server 对象任务  
 下表列出了传输 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**TransferSqlServerObjectsTaskFinishedTransferringObjects**|指示任务已完成传输 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象。|  
|**TransferSqlServerObjectsTaskStartTransferringObjects**|指示任务已开始传输 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象。|  
  
####  <a name="web-services-task"></a><a name="WebServices"></a> Web 服务任务  
 下表列出了可以为 Web 服务任务启用的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**WSTaskBegin**|任务已开始访问 Web 服务。|  
|**WSTaskEnd**|任务已完成 Web 服务方法。|  
|**WSTaskInfo**|有关任务的说明性信息。|  
  
####  <a name="wmi-data-reader-task"></a><a name="WMIDataReader"></a> WMI 数据读取器任务  
 下表列出了 WMI 数据读取器任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|指示任务已开始读取 WMI 数据。|  
|**WMIDataReaderOperation**|报告任务所运行的 WQL 查询。|  
  
####  <a name="wmi-event-watcher-task"></a><a name="WMIEventWatcher"></a> WMI 事件观察器任务  
 下表列出了 WMI 事件观察器任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|表示发生了任务正在监视的事件。|  
|**WMIEventWatcherTimedout**|指示任务已超时。|  
|**WMIEventWatcherWatchingForWMIEvents**|指示任务已开始执行 WQL 查询。 日志项包括查询。|  
  
####  <a name="xml-task"></a><a name="XML"></a> XML 任务  
 下表介绍了 XML 任务的自定义日志项。  
  
|日志项|说明|  
|---------------|-----------------|  
|**XMLOperation**|提供任务所执行的操作的相关信息|  

## <a name="related-tasks"></a>Related Tasks  
 下面的列表包含一些链接，这些链接指向的主题说明如何执行与日志记录功能相关的任务。  
  
-   [Integration Services 包记录的事件](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
  
## <a name="related-content"></a>相关内容  
 [用于完整和详细日志记录的 DTLoggedExec 工具（CodePlex 项目）](https://go.microsoft.com/fwlink/?LinkId=150579)  
