---
description: 包的 SQL Server 代理作业
title: 包的 SQL Server 代理作业 | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- jobs [Integration Services]
- automatic package execution
- scheduling packages [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: ecf7a5f9-b8a7-47f1-9ac0-bac07cb89e31
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 353acdd28f3611bcc387bd4122ca377a2d8d0ca5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346867"
---
# <a name="sql-server-agent-jobs-for-packages"></a>包的 SQL Server 代理作业

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理可以自动化和计划 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的执行。 您可以计划部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包，以及存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区和文件系统中的包。  
 
> [!NOTE]
> 本文介绍如何在一般情况下计划 SSIS 包以及如何在本地计划包。 还可在以下平台上运行并计划 SSIS 包：
> - **Microsoft Azure 云**。 有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)和[计划 Azure 中的 SSIS 包执行](../lift-shift/ssis-azure-schedule-packages.md)。
> - **Linux**。 有关详细信息，请参阅[在 Linux 上使用 SSIS 提取、转换和加载数据](../../linux/sql-server-linux-migrate-ssis.md)和[在 Linux 上使用 cron 计划 SQL Server Integration Services 包执行](../../linux/sql-server-linux-schedule-ssis-packages.md)。

## <a name="sections-in-this-topic"></a>本主题的内容  
 本主题包含以下各节：  
  
-   [在 SQL Server 代理中计划作业](#jobs)  
  
-   [计划 Integration Services 包](#packages)  
  
-   [对计划的包进行故障排除](#trouble)  
  
##  <a name="scheduling-jobs-in-sql-server-agent"></a><a name="jobs"></a> Scheduling Jobs in SQL Server Agent  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的服务，使您能够通过运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业自动执行任务和计划任务的执行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务必须处于运行状态，作业才能自动运行。 有关详细信息，请参阅 [Configure SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md)。  
  
 在您连接到 **的实例时，** “SQL Server 代理” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 节点将出现在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的对象资源管理器中。  
  
 若要自动执行某一重复发生的任务，您可以通过使用 **“新建作业”** 对话框创建某个作业。 有关详细信息，请参阅 [执行作业](../../ssms/agent/implement-jobs.md)。  
  
 创建该作业后，必须至少添加一个步骤。 一个作业可以包括多个步骤，并且每个步骤可以执行不同的任务。 有关详细信息，请参阅 [Manage Job Steps](../../ssms/agent/manage-job-steps.md)。  
  
 在创建作业和作业步骤后，可以创建一个运行作业的计划。 不过，您还可以创建手动运行的无人参与的作业。 有关详细信息，请参阅 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)。  
  
 可以通过设置通知选项来增强作业，如指定在作业完成时向某个操作员发送电子邮件或添加警报。 有关详细信息，请参阅 [“警报”](../../ssms/agent/alerts.md)。  
  
##  <a name="scheduling-integration-services-packages"></a><a name="packages"></a> Scheduling Integration Services Packages  
 在您创建一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业以便计划 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包后，必须至少添加一个步骤，并将该步骤的类型设置为 **“SQL Server Integration Services 包”** 。 一个作业可以包括多个步骤，并且每个步骤可以运行不同的包。  
  
 从作业步骤中运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包类似于使用 **dtexec** (dtexec.exe) 和 **DTExecUI** (dtexecui.exe) 实用工具运行包。 可以在“新建作业步骤”对话框中设置运行时选项，而不是使用命令行选项或“执行包实用工具”对话框来设置包的运行时选项。 有关运行包的选项的详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。  
  
 有关详细信息，请参阅 [使用 SQL Server 代理计划包](#schedule)。  
  
 有关演示如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来运行包的视频，请观看 MSDN 库中的视频主页[操作说明：使用 SQL Server 代理自动执行包（SQL Server 视频）](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100))。  
  
##  <a name="troubleshooting"></a><a name="trouble"></a> 故障排除  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤可能无法启动某个包，即便该包可以在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中以及从命令行成功运行。 该问题有一些常见的原因和一些推荐的解决方法。 有关详细信息，请参阅以下资源。  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] 知识库文章： [当从 SQL Server 代理作业步骤调用 SSIS 包时 SSIS 包不运行](https://support.microsoft.com/kb/918760)  
  
-   MSDN 库中的视频[排除故障：使用 SQL Server 代理执行包（SQL Server 视频）](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100))。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤启动某个包后，该包可能无法执行，或者包可能成功运行但出现意外结果。 可以使用下列工具来解决这些问题。  
  
-   对于存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] MSDB 数据库、 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区或本地计算机上的文件夹中的包，可以使用 **“日志文件查看器”** 以及在包执行期间生成的任何日志和调试转储文件。  
  
     **若要使用日志文件查看器，请执行下列操作。**  
  
    1.  右键单击对象资源管理器中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业，然后单击“查看历史记录”。  
  
    2.  在 **“日志文件摘要”** 框中，找到 **“消息”** 列中标有 **“作业失败”** 消息的作业执行。  
  
    3.  展开该作业节点，然后单击作业步骤查看 **“日志文件摘要”** 框下方区域中的消息的详细信息。  
  
-   对于存储在 SSISDB 数据库中的包，也可以使用 **“日志文件查看器”** 以及在包执行期间生成的任何日志和调试转储文件。 此外，还可以使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的报告。  
  
     **若要查找报告中与作业执行关联的包执行的信息，请执行以下操作。**  
  
    1.  按照如上步骤查看作业步骤消息的详细信息。  
  
    2.  找到消息中列出的执行 ID。  
  
    3.  在对象资源管理器中，展开“Integration Services 目录”节点。  
  
    4.  右键单击 SSISDB，依次指向“报表”和“标准报表”，然后单击“所有执行”。  
  
    5.  在 **“所有执行”** 报告的 **ID** 列中，找到该执行 ID。 单击 **“概述”** 、 **“所有消息”** 或 **“执行性能”** ，查看有关此包执行的信息。  
  
    有关“概述”、“所有消息”和“执行性能”报告的详细信息，请参阅 [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)。  

## <a name="schedule-a-package-by-using-sql-server-agent"></a><a name="schedule"></a> 使用 SQL Server 代理计划包
  下面的过程提供了通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤运行包来自动执行该包的步骤。  
  
### <a name="to-automate-package-execution-by-using-sql-server-agent"></a>通过使用 SQL Server 代理自动执行包  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到要在其上创建作业的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，或者打开包含要向其中添加步骤的作业的实例。  
  
2.  在对象资源管理器中展开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点，然后执行下列任务之一：  
  
    -   若要新建作业，请右键单击“作业”，再单击“新建作业”。  
  
    -   若要向现有作业添加步骤，请展开“作业”，右键单击该作业，再单击“属性”。  
  
3.  在 **“常规”** 页上，如果要创建新的作业，请提供作业名称，选择所有者和作业类别，还可以选择提供作业说明。  
  
4.  若要使作业可以进行安排，请选择 **“已启用”** 。  
  
5.  若想为要计划的包创建作业步骤，请单击 **“步骤”** ，然后单击 **“新建”** 。  
  
6.  选择 **“Integration Services 包”** 作为作业步骤类型。  
  
7.  在 **“运行身份”** 列表中，选择 **“SQL Server 代理服务帐户”** 或选择该作业步骤要使用的凭据所属的代理帐户。 有关创建代理帐户的信息，请参阅 [Create a SQL Server Agent Proxy](../../ssms/agent/create-a-sql-server-agent-proxy.md)。  

    用代理帐户来代替 **“SQL Server 代理服务帐户”** 可以解决在使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行包时可能出现的常见问题。 有关这些问题的详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 知识库文章 [从 SQL Server 代理作业步骤调用 SSIS 包时 SSIS 包不运行](https://support.microsoft.com/kb/918760)。 
     
    - 使用代理运行作业时，以下安全项必须就位，这样作业才能成功运行。

        代理使用的凭据登录名、运行 SQL Server 代理的帐户，以及运行 SQL Server 服务的帐户     必须拥有以下权限：

        - 本地安全策略属性：替换进程级令牌
        - 完全控制 %SYSTEMROOT%\Temp 

        如果安全项未就位，作业就会失败，并生成如下错误消息：作业失败。 客户端没有所需的特权。

        > **注意：** 如果代理帐户所用凭据的密码更改，那么您需要更新凭据密码。 否则，该作业步骤将失败。  

        有关配置 SQL Server 代理服务帐户的信息，请参阅[为 SQL Server 代理设置服务启动帐户（SQL Server 配置管理器）](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md)。  

8.  在 **“包源”** 列表框中，单击该包的源，然后为作业步骤配置选项。  
  
    **下表说明了可能的包源。**  
  
    |“包源”|说明|  
    |--------------------|-----------------|  
    |**SSIS 目录**|存储在 SSISDB 数据库中的包。 部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目中包含的包。|  
    |**SQL Server**|存储在 MSDB 数据库中的包。 可使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务来管理这些包。|  
    |**SSIS 包存储区**|存储在您计算机上默认文件夹中的包。 默认文件夹为 \<drive>:\Program Files\Microsoft SQL Server\110\DTS\Packages。 可使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务来管理这些包。<br /><br /> 注意：可以修改 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的配置文件，从而在文件系统中指定其他一个或多个文件夹，以供 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务管理。 有关详细信息，请参阅 [Integration Services 服务（SSIS 服务）](../../integration-services/service/integration-services-service-ssis-service.md)。|  
    |**文件系统**|存储在您本地计算机上任意文件夹中的包。|  
    |||
  
    **以下各表说明可用于作业步骤的配置选项（具体选项取决于您所选的包源）。**  
  
    > **重要提示：** 如果包受密码保护，当单击“新建作业步骤”的“常规”页上除“包”选项卡之外的任何选项卡时，需要在出现的“包密码”对话框中输入密码。 如果不输入， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业将无法运行该包。  
  
    **包源**：SSIS 目录  
  
    |选项卡|选项|  
    |---------|-------------|  
    |**包**|**Server**<br /><br /> 键入或选择承载 SSISDB 目录的数据库服务器实例的名称。<br /><br /> 如果 **“SSIS 目录”** 为包源，则可以仅使用 Microsoft Windows 用户帐户登录该服务器。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证不可用。|  
    ||**包**<br /><br /> 单击省略号按钮并选择一个包。<br /><br /> 您需要在 **“对象资源管理器”** 的 **“Integration Services 目录”** 节点下的文件夹中选择包。|  
    |**参数**<br /><br /> 位于 **“配置”** 选项卡上。|**“Integration Services 项目转换向导”** 支持您使用参数替换包配置。<br /><br /> **“参数”** 选项卡显示您在设计包（例如通过使用 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]来设计包）时就已添加的参数。 该选项卡还显示在将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目从包部署模型转换为项目部署模型后添加到包中的参数。 为包中包含的参数输入新值。 您可以输入一个文字值，或使用已经映射到该参数的服务器环境变量所包含的值。<br /><br /> 若要输入文字值，请单击参数旁边的省略号按钮。 随即出现 **“编辑用于执行的文字值”** 对话框。<br /><br /> 若要使用环境变量，请单击 **“环境”** ，然后选择包含要使用的变量的环境。<br /><br /> **\*\* 重要说明 \*\*** 如果将多个参数和/或连接管理器属性映射到了多个环境中包含的变量， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将显示一条错误消息。 对于给定的执行，只能使用单个服务器环境中包含的值来执行包。<br /><br /> 有关如何创建服务器环境并将变量映射到参数的信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。|  
    |**连接管理器**<br /><br /> 位于 **“配置”** 选项卡上。|更改连接管理器属性的值。 例如，您可以更改服务器名称。 将在 SSIS 服务器上为连接管理器属性自动生成参数。 若要更改某个属性值，可以输入一个文字值，或使用已经映射到连接管理器属性的服务器环境变量所包含的值。<br /><br /> 若要输入文字值，请单击参数旁边的省略号按钮。 随即出现 **“编辑用于执行的文字值”** 对话框。<br /><br /> 若要使用环境变量，请单击 **“环境”** ，然后选择包含要使用的变量的环境。<br /><br /> **\*\* 重要说明 \*\*** 如果将多个参数和/或连接管理器属性映射到了多个环境中包含的变量， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将显示一条错误消息。 对于给定的执行，只能使用单个服务器环境中包含的值来执行包。<br /><br /> 有关如何创建服务器环境并将变量映射到连接管理器属性的信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。|  
    |**高级**<br /><br /> 位于 **“配置”** 选项卡上。|为包执行配置以下附加设置：|  
    ||**属性覆盖**：<br /><br /> 单击 **“添加”** 为包属性输入新值，指定属性路径并指示该属性值是否为敏感值。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器将对敏感数据加密。 若要编辑或删除某个属性的设置，请单击 **“属性”** 覆盖框中的某一行，然后单击 **“编辑”** 或 **“删除”** 。 你可以通过执行以下操作之一查找属性路径：<br /><br /> \- 从 XML 配置文件 (\*.dtsconfig) 复制属性路径。 该路径列在该文件的 Configuration 部分中，作为 Path 属性的值。 下面是 MaximumErrorCount 属性的路径的示例：\Package.Properties[MaximumErrorCount]<br /><br /> \- 运行“包配置向导”，并从最后的“完成向导”页中复制属性路径。 随后可以取消该向导。<br /><br /> <br /><br /> 注意：“属性替代”选项用于所含配置已从旧版 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 升级的包。 使用 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 创建并部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包使用参数而不是配置。|  
    ||**日志记录级别**<br /><br /> 为执行包选择以下日志记录级别之一。 请注意，选择“性能”  或“详细”  日志记录级别可能影响包的执行性能。<br /><br /> **无**：<br />                          关闭日志记录。 仅记录包执行状态。<br /><br /> **基本**：<br />                          除了自定义事件和诊断事件之外，记录其余所有事件。 这是日志记录级别的默认值。<br /><br /> **性能**：<br />                          仅记录性能统计信息、OnError 和 OnWarning 事件。<br /><br /> **详细**：<br />                          记录所有事件，包括自定义事件和诊断事件。<br /><br /> 所选的日志记录级别决定在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的 SSISDB 视图和报告中显示的信息。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。|  
    ||**出错时转储**<br /><br /> 指定在包执行过程中发生任何错误时是否生成调试转储文件。 这些文件包含有关包的执行信息，可帮助您解决出现的问题。 若选择此选项，当在执行过程中出现错误时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建一个 .mdmp 文件（二进制文件）和一个 .tmp 文件（文本文件）。 默认情况下，[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将这些文件存储在 \<drive>:\Program Files\Microsoft SQL Server\110\Shared\ErrorDumps 文件夹中。|  
    ||**32 位运行时**<br /><br /> 指示是否在已安装 64 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的 64 位计算机上，使用 32 位版本的 dtexec 实用工具运行包。<br /><br /> 在特定情况下，您可能需要使用 32 位版本的 dtexec 来运行包，比如在您的包使用在 64 位版本中不可用的本机 OLE DB 访问接口的情况下。 有关详细信息，请参阅 [Integration Services 的 64 位注意事项](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx)。<br /><br /> 默认情况下，当您选择 **“SQL Server Integration Services 包”** 作业步骤类型时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理会使用系统自动调用的 dtexec 实用工具版本来运行该包。 系统会根据计算机处理器以及在计算机上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的版本，来调用 32 位或 64 位版本的实用工具。|  
  
     **包源**：SQL Server、SSIS 包存储或文件系统  
  
     可以为存储在 SQL Server、SSIS 包存储区或文件系统中的包设置的许多选项都对应于 **dtexec** 命令提示实用工具的命令行选项。 有关该实用工具和命令行选项的详细信息，请参阅 [dtexec 实用工具](../../integration-services/packages/dtexec-utility.md)。  
  
    |选项卡|选项|  
    |---------|-------------|  
    |**包**<br /><br /> 这些是存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区中的包的选项卡选项。|**Server**<br /><br /> 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务键入或选择数据库服务器实例的名称。|  
    ||**Use Windows Authentication**<br /><br /> 选择此选项可以使用 Microsoft Windows 用户帐户登录该服务器。|  
    ||**Use SQL Server Authentication**<br /><br /> 当用户使用指定的登录名和密码从不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将通过检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码匹配，来进行身份验证。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 找不到登录帐户，则身份验证会失败，用户将收到错误消息。|  
    ||**用户名**|  
    ||**密码**|  
    ||**包**<br /><br /> 单击省略号按钮并选择包。<br /><br /> 您需要在 **“对象资源管理器”** 的 **“已存储的包”** 节点下的文件夹中选择包。|  
    |**包**<br /><br /> 这些是存储在文件系统中的包的选项卡选项。|**包**<br /><br /> 键入包文件的完整路径，或单击省略号按钮选择包。|  
    |**配置**|添加 XML 配置文件以便使用特定配置来运行包。 使用包配置可在运行时更新包属性的值。<br /><br /> 此选项对应于 **dtexec** 的 **/ConfigFile** 选项。<br /><br /> 若要了解如何应用包配置，请参阅 [Package Configurations](./legacy-package-deployment-ssis.md)。 有关如何创建包配置的信息，请参阅 [Create Package Configurations](./legacy-package-deployment-ssis.md)。|  
    |**命令文件**|在单独的文件中指定要使用 **dtexec** 运行的附加选项。<br /><br /> 例如，可以包括一个包含 /Dump errorcode 选项的文件，以便在包运行期间发生一个或多个指定事件时生成调试转储文件。<br /><br /> 您可以使用不同的选项组运行包，只需创建多个文件，然后通过使用 **“命令文件”** 选项指定正确的文件即可。<br /><br /> “命令文件”选项对应于 **dtexec** 的 **/CommandFile** 选项。|  
    |**数据源**|查看包中包含的连接管理器。 若要修改某个连接字符串，请单击连接管理器，再单击该连接字符串。<br /><br /> 此选项对应于 **dtexec** 的 **/Connection** 选项。|  
    |**执行选项**|**出现验证警告时包失败**<br /> 指示是否将警告消息视为错误。 如果选择此选项并且在验证期间出现警告，包将无法通过验证。 此选项对应于 **dtexec** 的 **/WarnAsError** 选项。<br /><br /> **验证但不执行包**<br /> 指示是否在验证阶段之后停止执行包，而不实际运行包。 此选项对应于 **dtexec** 的 **/Validate** 选项。<br /><br /> **覆盖 MacConcurrentExecutables 属性**<br /> 指定包可以同时执行的可执行文件数。 如果值为 -1，则表示包可以运行的最大可执行文件数等于执行包的计算机上的处理器总数加二。 此选项对应于 **dtexec** 的 **/MaxConcurrent** 选项。<br /><br /> **启用包检查点**<br /> 指示包在执行期间是否使用检查点。 有关详细信息，请参阅 [通过使用检查点重新启动包](../../integration-services/packages/restart-packages-by-using-checkpoints.md)。<br /><br /> 此选项对应于 **dtexec** 的 **/CheckPointing** 选项。<br /><br /> **覆盖重新启动选项**<br /> 指示是否为包的 **CheckpointUsage** 属性设置了新值。 从 **“重新启动选项”** 列表框中选择一个值。<br /><br /> 此选项对应于 **dtexec** 的 **/Restart** 选项。<br /><br /> **使用 32 位运行时**<br /> 指示是否在已安装 64 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的 64 位计算机上，使用 32 位版本的 dtexec 实用工具运行包。<br /><br /> 在特定情况下，您可能需要使用 32 位版本的 dtexec 来运行包，比如在您的包使用在 64 位版本中不可用的本机 OLE DB 访问接口的情况下。 有关详细信息，请参阅 [Integration Services 的 64 位注意事项](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx)。<br /><br /> 默认情况下，当您选择 **“SQL Server Integration Services 包”** 作业步骤类型时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理会使用系统自动调用的 dtexec 实用工具版本来运行该包。 系统会根据计算机处理器以及在计算机上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的版本，来调用 32 位或 64 位版本的实用工具。|  
    |**Logging**|将日志提供程序与包执行操作相关联。<br /><br /> **用于文本文件的 SSIS 日志提供程序**<br /> 将日志条目写入 ASCII 文本文件<br /><br /> **用于 SQL Server 的 SSIS 日志提供程序**<br /> 将日志条目写入 MSDB 数据库的 sysssislog 表中。<br /><br /> **用于 SQL Server Profiler 的 SSIS 日志提供程序**<br /> 写入可用 SQL Server 事件探查器查看的跟踪。<br /><br /> **用于 Windows 事件日志的 SSIS 日志提供程序**<br /> 将日志条目写入 Windows 事件日志中的应用程序日志。<br /><br /> **用于 XML 文件的 SSIS 日志提供程序**<br /> 将日志文件写入 XML 文件。<br /><br /> 对于文本文件、XML 文件以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件探查器日志提供程序，您需要选择包内所含的文件连接管理器。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志提供程序，您需要选择包内所含的 OLE DB 连接管理器。<br /><br /> 此选项对应于 **dtexec** 的 **/Logger** 选项。|  
    |**设置值**|覆盖包属性设置。 在 **“属性”** 框中的 **“属性路径”** 和 **“值”** 列中输入值。 为一个属性输入值后， **“属性”** 框中会出现一个空行，支持您为另一个属性输入值。<br /><br /> 若要从“属性”框中删除某个属性，请单击该行，然后单击 **“删除”** 。<br /><br /> 你可以通过执行以下操作之一查找属性路径：<br /><br /> \- 从 XML 配置文件 (\*.dtsconfig) 复制属性路径。 该路径列在该文件的 Configuration 部分中，作为 Path 属性的值。 下面是 MaximumErrorCount 属性的路径的示例：\Package.Properties[MaximumErrorCount]<br /><br /> \- 运行“包配置向导”，并从最后的“完成向导”页中复制属性路径。 随后可以取消该向导。|  
    |**验证**|**仅执行已签名的包**<br /> 指示是否已检查包签名。 如果包未签名或签名无效，则包将失败。 此选项对应于 **dtexec** 的 **/VerifySigned** 选项。<br /><br /> **验证包内部版本**<br /> 指示是否根据在此选项旁边的 **“内部版本”** 框中输入的内部版本号，验证包的内部版本号。 如果出现不匹配，则将不执行包。 此选项对应于 **dtexec** 的 **/VerifyBuild** 选项。<br /><br /> **验证包 ID**<br /> 指示是否通过将包的 GUID 与此选项旁边的 **“包 ID”** 框中输入的包 ID 进行比较，对该 GUID 进行验证。 此选项对应于 **dtexec** 的 **/VerifyPackageID** 选项。<br /><br /> **验证版本 ID**<br /> 指示是否通过将包的版本 GUID 与此选项旁边的 **“版本 ID”** 框中输入的版本 ID 进行比较，对该版本 GUID 进行验证。 此选项对应于 **dtexec** 的 **/VerifyVersionID** 选项。|  
    |**命令行**|修改 dtexec 的命令行选项。 有关这些选项的详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。<br /><br /> **还原原始选项**<br /> 使用在“作业集属性”对话框的“包”、“配置”、“命令文件”、“数据源”、“执行选项”、“日志记录”、“设置值”和“验证”选项卡上设置的命令行选项。<br /><br /> **手动编辑命令**<br /> 在“命令行”框中键入附加命令行选项。<br /><br /> 单击“确定”将更改保存至作业步骤之前，可以通过单击“还原原始选项”删除在“命令行”框中键入的所有附加选项。  <br /><br /> **\*\* 提示 \*\*** 你可以将命令行复制到命令提示符窗口，添加 `dtexec`，然后从该命令行运行包。 这是一种生成命令行文本的简单方法。|  
  
9. 单击 **“确定”** 保存设置，然后关闭 **“新建作业步骤”** 对话框。  
  
    > [!NOTE]
    > 对于存储在 **“SSIS 目录”** 中的包，如果有未解析的参数或连接管理器属性设置，则禁用 **“确定”** 按钮。 在使用服务器环境变量包含的值设置参数或属性，但存在以下条件之一时，就会出现未解析的设置：  
    >   
    >  未选中 **“配置”** 选项卡上的 **“环境”** 复选框。  
    >   
    >  没有在 **“配置”** 选项卡上的列表框中选择包含变量的服务器环境。  
  
10. 若要为作业步骤创建计划，请单击 **“选择页”** 窗格中的 **“计划”** 。 有关如何配置计划的信息，请参阅 [Schedule a Job](../../ssms/agent/schedule-a-job.md)。  
  
    > [!TIP]  
    >  为计划命名时，请考虑使用唯一的描述性名称，以便与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理计划区分。  

## <a name="see-also"></a>另请参阅  
 [项目和包的执行](deploy-integration-services-ssis-projects-and-packages.md)  

## <a name="external-resources"></a>外部资源  
  
-   [网站上的知识库文章：](https://support.microsoft.com/kb/918760)当从 SQL Server 代理作业步骤调用 SSIS 包时 SSIS 包不运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)]  
  
-   MSDN 库中的视频[排除故障：使用 SQL Server 代理执行包（SQL Server 视频）](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100))  
  
-   MSDN 库中的视频[操作说明：使用 SQL Server 代理自动执行包（SQL Server 视频）](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100))  
  
-   mssqltips.com 上的技术文章 [Checking SQL Server Agent jobs using Windows PowerShell](https://go.microsoft.com/fwlink/?LinkId=165675)（使用 Windows PowerShell 检查 SQL Server 代理作业）  
  
-   mssqltips.com 上的技术文章 [Auto alert for SQL Agent jobs when they are enabled or disabled](https://go.microsoft.com/fwlink/?LinkId=165676)（在 SQL 代理作业启用或禁用时针对它们的自动警报）  
  
-   mssqltips.com 上的博客文章 [配置 SQL 代理作业以便写入 Windows 事件日志](https://go.microsoft.com/fwlink/?LinkId=220745)。  
  
