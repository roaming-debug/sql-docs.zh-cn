---
title: 使用图形用户界面安装
description: 本文介绍了使用 SQL Server 安装程序安装向导来安装新 SQL Server 实例的分步过程。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, steps
- Setup [SQL Server], steps
- SQL Server, installing
ms.assetid: 6ad23de1-2bab-4933-9122-c09f5565028d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d2c5f2fc245c981cf0c6d045333528aeff32536c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170189"
---
# <a name="install-sql-server-from-the-installation-wizard-setup"></a>使用安装向导安装 SQL Server（安装程序）

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文介绍如何使用安装向导安装 SQL Server。 它适用于 [!INCLUDE[SQLServer2016](../../includes/sssql16-md.md)] 和 [!INCLUDE[SQLServer2017](../../includes/sssqlv14-md.md)]。

本文介绍了使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序安装向导来安装新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的分步过程。 此安装向导提供了一个用于安装所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件的功能树，这样你就不必逐个安装这些组件了。 若要逐个安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，请参阅[安装 SQL Server](../../database-engine/install-windows/install-sql-server.md#individual-component-installation)。  

有关安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他方法，请参阅：  

* [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)  
* [使用配置文件安装 SQL Server](./install-sql-server-using-a-configuration-file.md)  
* [使用 SysPrep 安装 SQL Server](../../database-engine/install-windows/install-sql-server-using-sysprep.md)  
* [新建 SQL Server 故障转移群集（安装程序）](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
* [使用安装向导升级 SQL Server（安装程序）](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]
  
## <a name="prerequisites"></a>先决条件

安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 前，请先审阅[计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)。  
  
> [!NOTE]  
> 对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。  

::: moniker range=">=sql-server-2016 <=sql-server-2017"

###  <a name="install-patch-requirement"></a><a name="bkmk_ga_instalpatch"></a> 安装修补程序要求

Microsoft 已发现，作为 SQL Server 2016 和 2017 系统必备组件安装的 Microsoft Visual C++ 2013 运行时二进制文件存在问题。 现在有可用的更新来修复该问题。 如果 Visual C++ 运行时二进制文件的此更新未安装，SQL Server 可能会在某些情况下出现稳定性问题。 安装 SQL Server 前，请先按照 [SQL Server 发行说明](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch)中的说明操作，以确定计算机是否需要 Visual C++ 运行时二进制文件的修补程序。 

这不适用于 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]。

## <a name="to-install-sql-server-2016-and-2017"></a>安装 SQL Server 2016 和 2017  

1. 插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 **Setup.exe**。 若要从网络共享进行安装，请先在共享中找到根文件夹，再双击“Setup.exe”。  
  
1. 安装向导将运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心。 若要新建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，请依次选择左侧导航区域中的“安装”和“新建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立安装或向现有安装添加功能”。  

1. 在“产品密钥”页中，选择选项来指明是要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 免费版本，还是要安装有 PID 密钥的生产版本。 有关详细信息，请参阅 [SQL Server 2017 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。  
  
   若要继续操作，请选择“下一步”。

1. 在“许可条款”页中，审阅许可协议。 如果同意，请选中“我接受许可条款”复选框，再选择“下一步”。  
    
   > [!NOTE]
   > SQL Server 传输有关安装体验的信息，以及其他使用情况和性能数据，以帮助 Microsoft 改进产品。 若要详细了解 SQL Server 数据处理和隐私控制，请参阅[隐私声明](https://privacy.microsoft.com/privacystatement)和[将 SQL Server 配置为向 Microsoft 发送反馈](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)。

1. 在“全局规则”页中，如果没有规则错误，安装程序会自动跳转到“产品更新”页。  
  
1. 如果未在依次转到“控制面板” > “所有控制面板项” > “Windows 更新” > “更改设置”后选中“Microsoft 更新”复选框，接下来会转到“Microsoft 更新”页。 选中“Microsoft 更新”复选框会将计算机设置更改为，在扫描是否有 Windows 更新时，扫描所有 Microsoft 产品的最新更新。  

1. “产品更新”页显示最新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新。 如果未发现任何产品更新，安装程序就不会显示此页，并自动跳转到“安装安装程序文件”页。  

1. 在“安装安装程序文件”页中，安装程序显示安装程序文件的下载、提取和安装进度。 如果找到了安装程序更新，且你指定包括它，也会安装此更新。 如果未找到任何更新，安装程序将自动进行。
  
1. 在“安装规则”页中，安装程序会检查是否有可能会在运行安装程序时发生的潜在问题。 如果检查到故障，请选择“状态”列中的项，以了解详细信息。 否则，请选择“下一步”。

1. 如果这是首次在计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，安装程序会跳过“安装类型”页，并直接转到“功能选择”页。 如果系统中已安装有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，你可以使用“安装类型”页来选择是执行新安装，还是向现有安装添加功能。 若要继续操作，请选择“下一步”。
  
1. 在“功能选择”页上，选择要安装的组件。 例如，要安装新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，请选择“数据库引擎服务”。

    选择功能名称后， **“功能说明”** 窗格中会显示每个组件组的说明。 您可以选中任意一些复选框。 有关详细信息，请参阅 [SQL Server 2016 版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md)或 [SQL Server 2017 版本和组件](../../sql-server/editions-and-components-of-sql-server-2017.md)。
  
     **“所选功能的必备组件”** 窗格中将显示所选功能的必备组件。 安装程序在本过程后面所述的安装步骤中安装尚未安装的系统必备。  
  
     也可以使用“功能选择”页底部的字段，指定共用组件的自定义目录。 若要更改共用组件的安装路径，请更新对话框底部字段中的路径，或选择“浏览”转到安装目录。 默认安装路径为 [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]。  
  
     > [!NOTE]
     > 为共享组件指定的路径必须是绝对路径。 文件夹不能压缩或加密。 不支持映射驱动器。  
  
     SQL Server 对共享功能使用两个目录：
  
     * 共享功能目录  
     * 共享功能目录 (x86)  
  
     > [!NOTE]
     > 为以上每个选项指定的路径必须不同。  
  
1. 如果所有规则都通过，“功能规则”页会自动跳转。  
  
1. 在“实例配置”页中，指定是要安装默认实例，还是命名实例。 有关详细信息，请参阅[实例配置](../../sql-server/install/instance-configuration.md#instance-configuration-page)。  
  
     * **实例 ID**：默认情况下，实例名称用作实例 ID。 此 ID 用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的安装目录和注册表项。 对于默认实例和命名实例，行为是相同的。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认实例 ID，请在“实例 ID”文本框中指定其他值。  
  
       > [!NOTE]  
       > 典型 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 独立实例（无论是默认实例，还是命名实例）不会使用非默认实例 ID 值。  
  
       所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的每个组件。  
  
     * **已安装实例**：此网格显示正在运行安装程序的计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 如果计算机上已经安装了一个默认实例，则必须安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]的命名实例。  
  
     安装程序的其余工作流视你已为安装指定的功能而定。 你可能不会看到所有页，具体视你的选择而定。 

1. 选择安装 Polybase 功能会将“PolyBase 配置”页添加到 SQL Server 安装程序，并显示在“实例配置”页之后 。 PolyBase 需要 Oracle JRE 7 更新 51（至少），如果未安装该更新，安装进程会受到阻止。 在“Polybase 配置”页上，可以选择使用 SQL Server 作为一个启用了 Polybase 的独立实例，也可以将此 SQL Server 作为 PolyBase 横向扩展组的一部分。 如果选择使用横向扩展组，需要指定最多 6 个（或更多）端口的端口范围。 


1. 使用“服务器配置 - 服务帐户”页指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的登录帐户。 你在此页中配置的实际服务取决于你已选择安装的功能。 若要详细了解配置设置，请参阅[安装向导帮助](../../sql-server/install/instance-configuration.md#serverconfig)。
  
     可以为所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 还可以指定是自动启动、手动启动还是禁用服务。 建议单独配置服务帐户，以提供每个服务的最小特权。 请务必向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务授予完成任务所必需的最低权限。 有关详细信息，请参阅[配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
     若要为此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的所有服务帐户指定同一个登录帐户，请在此页底部的字段中提供凭据。  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > 自 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 起，选中“向 SQL Server 数据库引擎服务授予执行卷维护任务权限”复选框，可以让 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]服务帐户使用[数据库即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。
  
1. 使用“服务器配置 - 排序规则”页为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 指定非默认排序规则。    

   默认安装设置由操作系统 (OS) 区域设置确定。 服务器级排序规则可以在安装期间更改，也可以在安装前通过更改 OS 区域设置进行更改。 默认排序规则设置为与每个特定区域设置关联的最早可用版本。 这是出于向后兼容的原因。 因此，不推荐总是使用默认排序规则。 更改 Windows 排序规则的默认安装设置可充分利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 例如，对于 OS 区域设置“英语(美国)”（代码页 1252），安装过程中的默认排序规则是 SQL_Latin1_General_CP1_CI_AS，可将其更改为最接近的 Windows 对等排序规则 Latin1_General_100_CI_AS_SC  。

   有关详细信息，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
1. 使用“数据库配置 - 服务器配置”页指定以下各个选项：  
  
    * **安全模式**：为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例选择“Windows 身份验证”或“混合模式身份验证”。 如果选择“混合模式身份验证”，必须为内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员帐户提供强密码。  
  
       在设备与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 成功建立连接后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。 有关详细信息，请参阅[“数据库引擎配置 - 服务器配置”页](../../sql-server/install/instance-configuration.md#serverconfig)。
  
    * **SQL Server 管理员**：必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例指定至少一个系统管理员。 若要添加用于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的帐户，请选择“添加当前用户”。 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例拥有管理员权限的用户、组或计算机列表。  
  
     使用“数据库引擎配置 - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。  
  
    > [!IMPORTANT]  
    > 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。  
  
     有关详细信息，请参阅[“数据库引擎配置 - 数据目录”页](../../sql-server/install/instance-configuration.md#datadir)。

     使用“数据库引擎配置 - TempDB”页配置 tempdb 的文件大小、文件数、非默认安装目录和文件增长设置。 有关详细信息，请参阅[“数据库引擎配置 - TempDB”页](../../sql-server/install/instance-configuration.md#tempdb)。 

 
     使用“数据库引擎配置 - FILESTREAM”页为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用 FILESTREAM。 有关详细信息，请参阅[“数据库引擎配置 - FILESTREAM”页](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page)。  
  
1. 使用“Analysis Services - 帐户预配”页指定服务器模式，以及对 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 拥有管理员权限的用户或帐户。 服务器模式决定了哪些内存和存储子系统用于服务器。 不同的解决方案类型在不同的服务器模式下运行。 如果计划在服务器上运行多维数据集数据库，请选择默认服务器模式选项“多维和数据挖掘”。

    必须为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 指定至少一个系统管理员：

    * 若要添加正在运行 SQL Server 安装程序的帐户，请选择“添加当前用户”。

    * 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 拥有管理员权限的用户、组或计算机列表。

    若要详细了解服务器模式和管理员权限，请参阅[“Analysis Services 配置 - 帐户预配”页](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page)。

    编辑完列表后，选择“确定”。 验证配置对话框中的管理员列表。 在列表是完整后，单击“下一步”。

    使用“Analysis Services - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。  

    > [!IMPORTANT]  
    > 安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，如果你为 INSTANCEDIR 和 SQLUSERDBDIR 指定相同的目录路径，SQL Server 代理和全文搜索不会启动，因为缺少权限。  
    >  
    > 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。  

    有关详细信息，请参阅[“Analysis Services 配置 - 数据目录”页](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page)。  

1. 使用“Distributed Replay 控制器配置”页指定要向其授予 Distributed Replay 控制器服务管理权限的用户。 拥有管理权限的用户可以不受限制地访问 Distributed Replay 控制器服务。  
  
     * 若要向正在运行 SQL Server 安装程序的用户授予 Distributed Replay 控制器服务访问权限，请选择“添加当前用户”按钮。

     * 若要向其他用户授予 Distributed Replay 控制器服务访问权限，请选择“添加”按钮。

     * 若要撤消 Distributed Replay 控制器服务访问权限，请选择“删除”按钮。

     * 若要继续操作，请选择“下一步”。  
  
1. 使用“Distributed Replay 客户端配置”页指定要向其授予 Distributed Replay 客户端服务管理权限的用户。 拥有管理权限的用户可以不受限制地访问 Distributed Replay 客户端服务。  
  
     * “控制器名称”是可选选项。 默认值是 \<*blank*>。 输入客户端计算机用来与 Distributed Replay 客户端服务通信的控制器的名称：  
  
       * 如果已设置控制器，请在配置每个客户端时输入控制器名称。  
  
       * 如果尚未设置控制器，可以将控制器名称保留为空白。 但是，您必须在 **“客户端配置”** 文件中手动输入控制器名称。  
  
     * 为 Distributed Replay 客户端服务指定 **“工作目录”** 。 默认工作目录为 \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\。  
  
     * 为 Distributed Replay 客户端服务指定 **“结果目录”** 。 默认结果目录为 \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\。  
  
     * 若要继续操作，请选择“下一步”。  
  
1. “准备安装”页显示你在使用安装程序期间指定的安装选项的树状视图。 在此页中，安装程序指明“产品更新”功能是启用还是禁用，以及最终更新版本。  
  
     若要继续操作，请选择“安装”。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序先安装选定功能的系统必备，再安装选定功能。  
  
1. 在安装过程中，“安装进度”页显示状态更新，以便你能在安装程序继续运行时监视安装进度。  
  
1. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。
  
    > [!IMPORTANT]
    > 运行完安装程序后，请务必阅读安装向导中的消息。 有关详细信息，请参阅[查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

    若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击“关闭”。  
  
1. 如果系统指示你重启计算机，请立即重启。

::: moniker-end

::: moniker range=">=sql-server-ver15"
## <a name="to-install-sql-server-2019"></a>安装 SQL Server 2019 
  
1. 插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质， 然后双击根文件夹中的 **Setup.exe**。 若要从网络共享进行安装，请先在共享中找到根文件夹，再双击“Setup.exe”。  
  
1. 安装向导将运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心。 若要新建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，请依次选择左侧导航区域中的“安装”和“新建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立安装或向现有安装添加功能”。  

1. 在“产品密钥”页中，选择选项来指明是要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 免费版本，还是要安装有 PID 密钥的生产版本。 有关详细信息，请参阅 [SQL Server 2017 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)。  
  
   若要继续操作，请选择“下一步”。
  
1. 在“许可条款”页中，审阅许可协议。 如果同意，请选中“我接受许可条款和[隐私声明](https://privacy.microsoft.com/privacystatement)”复选框，再选择“下一步”。  

   > [!NOTE]
   > 如果输入了企业服务器/CAL 许可证产品密钥，且计算机上有 20 多个物理内核，或者在启用超线程时有 40 个逻辑内核，则安装过程中会显示警告。 要继续安装，请选择“选中此框确认此限制或单击‘返回/取消’输入支持操作系统最大处理器数的企业内核产品许可证”复选框，或者单击“返回”并输入支持操作系统最大处理器数的许可证密钥 。

   > [!NOTE]
   > SQL Server 传输有关安装体验的信息，以及其他使用情况和性能数据，以帮助 Microsoft 改进产品。 若要详细了解 SQL Server 数据处理和隐私控制，请参阅[隐私声明](https://privacy.microsoft.com/privacystatement)和[将 SQL Server 配置为向 Microsoft 发送反馈](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)。

1. 在“全局规则”页中，如果没有规则错误，安装程序会自动跳转到“产品更新”页。  
  
1. 如果未在依次转到“控制面板” > “所有控制面板项” > “Windows 更新” > “更改设置”后选中“Microsoft 更新”复选框，接下来会转到“Microsoft 更新”页。 选中“Microsoft 更新”复选框会将计算机设置更改为，在扫描是否有 Windows 更新时，扫描所有 Microsoft 产品的最新更新。  

1. “产品更新”页显示最新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新。 如果未发现任何产品更新，安装程序就不会显示此页，并自动跳转到“安装安装程序文件”页。  

1. 在“安装安装程序文件”页中，安装程序显示安装程序文件的下载、提取和安装进度。 如果找到了安装程序更新，且你指定包括它，也会安装此更新。 如果未找到任何更新，安装程序将自动进行。
  
1. 在“安装规则”页中，安装程序会检查是否有可能会在运行安装程序时发生的潜在问题。 如果检查到故障，请选择“状态”列中的项，以了解详细信息。 否则，请选择“下一步”。

1. 如果这是首次在计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，安装程序会跳过“安装类型”页，并直接转到“功能选择”页。 如果系统中已安装有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，你可以使用“安装类型”页来选择是执行新安装，还是向现有安装添加功能。 若要继续操作，请选择“下一步”。
  
1. 在“功能选择”页上，选择要安装的组件。 例如，要安装新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，请选择“数据库引擎服务”。

    选择功能名称后， **“功能说明”** 窗格中会显示每个组件组的说明。 您可以选中任意一些复选框。 有关详细信息，请参阅 [SQL Server 2016 版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md)或 [SQL Server 2017 版本和组件](../../sql-server/editions-and-components-of-sql-server-2017.md)。
  
     **“所选功能的必备组件”** 窗格中将显示所选功能的必备组件。 安装程序在本过程后面所述的安装步骤中安装尚未安装的系统必备。  
  
     也可以使用“功能选择”页底部的字段，指定共用组件的自定义目录。 若要更改共用组件的安装路径，请更新对话框底部字段中的路径，或选择“浏览”转到安装目录。 默认安装路径为 [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]。  
  
     > [!NOTE]
     > 为共享组件指定的路径必须是绝对路径。 文件夹不能压缩或加密。 不支持映射驱动器。  
  
     SQL Server 对共享功能使用两个目录：
  
     * 共享功能目录  
     * 共享功能目录 (x86)  
  
     > [!NOTE]
     > 为以上每个选项指定的路径必须不同。  
  
1. 如果所有规则都通过，“功能规则”页会自动跳转。  
  
1. 在“实例配置”页中，指定是要安装默认实例，还是命名实例。 有关详细信息，请参阅[实例配置](../../sql-server/install/instance-configuration.md#instance-configuration-page)。  
  
     * **实例 ID**：默认情况下，实例名称用作实例 ID。 此 ID 用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的安装目录和注册表项。 对于默认实例和命名实例，行为是相同的。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认实例 ID，请在“实例 ID”文本框中指定其他值。  
  
       > [!NOTE]  
       > 典型 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 独立实例（无论是默认实例，还是命名实例）不会使用非默认实例 ID 值。  
  
       所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的每个组件。  
  
     * **已安装实例**：此网格显示正在运行安装程序的计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 如果计算机上已经安装了一个默认实例，则必须安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]的命名实例。  
  
     安装程序的其余工作流视你已为安装指定的功能而定。 你可能不会看到所有页，具体视你的选择而定。 

1. 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，PolyBase 不再要求在安装此功能前预先在计算机上安装 Oracle JRE 7 Update 51（最低版本）。 选择安装 Polybase 功能会将“Java 安装位置”页添加到 SQL Server 安装程序，并显示在“实例配置”页之后 。 在“Java 安装位置”页上，可以选择安装 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 安装随附的 Azul Zulu Open JRE，也可以提供已在计算机上安装的另一个 JRE 或 JDK 的位置。

1. 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，Java 已经添加了语言扩展。 选择安装 Java 功能会将“Java 安装位置”页添加到 SQL Server 安装程序对话框窗口，并显示在“实例配置”页之后 。 在“Java 安装位置”页上，可以选择安装 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 安装随附的 Zulu Open JRE，也可以提供已在计算机上安装的另一个 JRE 或 JDK 的位置。

1. 使用“服务器配置 - 服务帐户”页指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的登录帐户。 你在此页中配置的实际服务取决于你已选择安装的功能。 若要详细了解配置设置，请参阅[安装向导帮助](../../sql-server/install/instance-configuration.md#serverconfig)。
  
     可以为所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 还可以指定是自动启动、手动启动还是禁用服务。 建议单独配置服务帐户，以提供每个服务的最小特权。 请务必向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务授予完成任务所必需的最低权限。 有关详细信息，请参阅[配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
     若要为此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的所有服务帐户指定同一个登录帐户，请在此页底部的字段中提供凭据。  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > 自 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 起，选中“向 SQL Server 数据库引擎服务授予执行卷维护任务权限”复选框，可以让 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]服务帐户使用[数据库即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。
  
     使用“服务器配置 - 排序规则”页指定[!INCLUDE[ssDE](../../includes/ssde-md.md)]和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的非默认排序规则。 有关详细信息，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
1. 使用“数据库配置 - 服务器配置”页指定以下各个选项：  
  
    * **安全模式**：为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例选择“Windows 身份验证”或“混合模式身份验证”。 如果选择“混合模式身份验证”，则必须为内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员帐户 (sa) 提供强密码。  
  
       在设备与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 成功建立连接后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。 有关详细信息，请参阅[“数据库引擎配置 - 服务器配置”页](../../sql-server/install/instance-configuration.md#serverconfig)。
  
    * **SQL Server 管理员**：必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例指定至少一个系统管理员。 若要添加用于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的帐户，请选择“添加当前用户”。 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例拥有管理员权限的用户、组或计算机列表。  你还可以添加 Windows 域组，以通过 SQL Server 的 sysadmin 访问权限在 Active Directory 中建立共享的 SQL 管理员组。 
  
     使用“数据库引擎配置 - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。  
  
    > [!IMPORTANT]  
    > 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。  
  
     有关详细信息，请参阅[“数据库引擎配置 - 数据目录”页](../../sql-server/install/instance-configuration.md#datadir)。

     使用“数据库引擎配置 - TempDB”页配置 tempdb 的文件大小、文件数、非默认安装目录和文件增长设置。 有关详细信息，请参阅[“数据库引擎配置 - TempDB”页](../../sql-server/install/instance-configuration.md#tempdb)。

     使用“[!INCLUDE[ssDE](../../includes/ssde-md.md)]配置 - MaxDOP”页指定最大并行度。 此设置决定了一个语句可以在执行期间使用多少个处理器。 系统自动在安装期间计算建议值。 
     
    > [!NOTE]  
    > 仅自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，才能在“设置”中使用此页。 
    
    有关详细信息，请参阅[“数据库引擎配置 - MaxDOP”页](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#maxdop)。 

     使用“数据库引擎配置 - 内存”页，指定此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在启动后使用的“最小服务器内存和“最大服务器内存”值。 可以使用默认值、计算出的建议值，也可以在选择“推荐”选项后手动指定你自己的值。
     
    > [!NOTE]  
    > 仅自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起，才能在“设置”中使用此页。 
    
    有关详细信息，请参阅[“数据库引擎配置 - 内存”页](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#memory)。 

     使用“数据库引擎配置 - FILESTREAM”页为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用 FILESTREAM。 有关详细信息，请参阅[“数据库引擎配置 - FILESTREAM”页](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page)。  
  
1. 使用“Analysis Services - 帐户预配”页指定服务器模式，以及对 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 拥有管理员权限的用户或帐户。 服务器模式决定了哪些内存和存储子系统用于服务器。 不同的解决方案类型在不同的服务器模式下运行。 如果计划在服务器上运行多维数据集数据库，请选择默认服务器模式选项“多维和数据挖掘”。

    必须为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 指定至少一个系统管理员：

    * 若要添加正在运行 SQL Server 安装程序的帐户，请选择“添加当前用户”。

    * 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 拥有管理员权限的用户、组或计算机列表。

    若要详细了解服务器模式和管理员权限，请参阅[“Analysis Services 配置 - 帐户预配”页](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page)。

    编辑完列表后，选择“确定”。 验证配置对话框中的管理员列表。 在列表是完整后，单击“下一步”。

    使用“Analysis Services - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。  

    > [!IMPORTANT]  
    > 安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，如果你为 INSTANCEDIR 和 SQLUSERDBDIR 指定相同的目录路径，SQL Server 代理和全文搜索不会启动，因为缺少权限。  
    >  
    > 如果指定非默认安装目录，请确保安装文件夹对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的目录共享。  

    有关详细信息，请参阅[“Analysis Services 配置 - 数据目录”页](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page)。  

1. 使用“Distributed Replay 控制器配置”页指定要向其授予 Distributed Replay 控制器服务管理权限的用户。 拥有管理权限的用户可以不受限制地访问 Distributed Replay 控制器服务。  
  
     * 若要向正在运行 SQL Server 安装程序的用户授予 Distributed Replay 控制器服务访问权限，请选择“添加当前用户”按钮。

     * 若要向其他用户授予 Distributed Replay 控制器服务访问权限，请选择“添加”按钮。

     * 若要撤消 Distributed Replay 控制器服务访问权限，请选择“删除”按钮。

     * 若要继续操作，请选择“下一步”。  
  
1. 使用“Distributed Replay 客户端配置”页指定要向其授予 Distributed Replay 客户端服务管理权限的用户。 拥有管理权限的用户可以不受限制地访问 Distributed Replay 客户端服务。  
  
     * “控制器名称”是可选选项。 默认值是 \<*blank*>。 输入客户端计算机用来与 Distributed Replay 客户端服务通信的控制器的名称：  
  
       * 如果已设置控制器，请在配置每个客户端时输入控制器名称。  
  
       * 如果尚未设置控制器，可以将控制器名称保留为空白。 但是，您必须在 **“客户端配置”** 文件中手动输入控制器名称。  
  
     * 为 Distributed Replay 客户端服务指定 **“工作目录”** 。 默认工作目录为 \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\。  
  
     * 为 Distributed Replay 客户端服务指定 **“结果目录”** 。 默认结果目录为 \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\。  
  
     * 若要继续操作，请选择“下一步”。  
  
1. “准备安装”页显示你在使用安装程序期间指定的安装选项的树状视图。 在此页中，安装程序指明“产品更新”功能是启用还是禁用，以及最终更新版本。  
  
     若要继续操作，请选择“安装”。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序先安装选定功能的系统必备，再安装选定功能。  
  
1. 在安装过程中，“安装进度”页显示状态更新，以便你能在安装程序继续运行时监视安装进度。  
  
1. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。
  
    > [!IMPORTANT]
    > 运行完安装程序后，请务必阅读安装向导中的消息。 有关详细信息，请参阅[查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

    若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击“关闭”。  
  
1. 如果系统指示你重启计算机，请立即重启。

::: moniker-end

## <a name="next-steps"></a>后续步骤

[配置新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装](../../database-engine/configure-windows/database-engine-instances-sql-server.md)。  
  
为了减少系统的可攻击外围应用， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有选择地安装和启用了一些关键服务和功能。 有关详细信息，请参阅[外围应用配置](../../relational-databases/security/surface-area-configuration.md)。  
  
## <a name="see-also"></a>另请参阅
  
* [验证 SQL Server 安装](../../database-engine/install-windows/validate-a-sql-server-installation.md)  
* [修复失败的 SQL Server 安装](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
* [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
* [使用安装向导升级 SQL Server（安装程序）](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)