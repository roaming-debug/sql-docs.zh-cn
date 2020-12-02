---
title: 向 SQL Server 的实例添加功能（安装程序）
description: 本文提供用于向 SQL Server 2019 实例添加识别实例的功能的分步过程。
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- " SQL Server, features"
- adding features to  SQL Server
ms.assetid: 97931fdc-d943-48dd-81b9-ae8b8d2c6dad
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 09/07/2019
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: b7fdbda79c36c8ddf939477db9ea197c22507b64
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126042"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>向 SQL Server 的实例添加功能（安装程序）

[!INCLUDE [ SQL Server - Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文提供用于向 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 实例添加功能的分步过程。 某些 SQL Server 组件或服务特定于 SQL Server 的实例。 它们也称为识别实例的组件或服务。 它们与托管它们的实例共享相同的版本，并且专用于该实例。 你可以向 SQL Server 实例添加识别实例的组件以及共享组件（如果尚未安装此类组件）。 有关 SQL Server 各版本支持的功能列表，请参阅 [SQL Server 2017 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。

若要从命令提示符向 SQL Server 实例添加功能，请参阅[从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)。

## <a name="prerequisites"></a>先决条件

继续之前，请查阅[计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)中的文章。

> [!NOTE]
> 对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 SQL Server，则必须使用对远程共享具有读取权限的域帐户。  
  
> [!NOTE]
> 在向 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的实例中添加功能时，现有的使用情况报告设置将应用于新添加的功能。 若要更改这些设置，请使用 SQL Server“配置工具”菜单上的“SQL Server 错误和使用情况报告”工具。

## <a name="procedures"></a>过程

#### <a name="to-add-features-to-an-instance-of-sscurrent"></a>若要从命令提示符向 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

1. 插入 SQL Server 安装介质。 然后双击根文件夹中的 setup.exe。 若要从网络共享进行安装，请导航到共享中的根文件夹，然后双击 setup.exe。 如果出现“[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 安装”对话框，请选择“确定”安装必备组件，然后选择“取消”退出 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 安装。  

2. 安装向导将启动 SQL Server 安装中心。 若要向现有 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 实例添加新功能，请在左侧导航区域中选择“安装”，然后选择“全新 SQL Server 独立安装或向现有安装添加功能”。

3. 系统配置检查器将在您的计算机上运行发现操作。 若要查看验证详细信息，请选择“查看详细信息”。 若要继续操作，请选择“确定”。

4. “产品更新”页显示最新 SQL Server 产品更新。 如果不想包括更新，则取消选中“包括 SQL Server 产品更新”复选框。 如果未发现任何产品更新，SQL Server 安装程序将不会显示该页并且自动前进到“安装安装程序文件”页。

5. 在“安装安装程序文件”页上，安装程序将提供下载、提取和安装这些安装程序文件的进度。 如果找到了针对 SQL Server 安装程序的更新，并且指定了包括该更新，那么也将安装该更新。 选择“安装”以安装安装程序支持文件。  

6. 系统配置检查器将在安装继续之前检验计算机的系统状态。  

7. 在“安装类型”页上，选择“向 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的现有实例中添加功能”选项，然后选择要更新的实例。

8. 在“功能选择”页上，选择要安装的组件。 选择功能名称后，右侧窗格中会显示每个组件组的说明。 您可以选中任意一些复选框。 有关详细信息，请参阅 [SQL Server 2017 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。 每个组件都只能在给定的 SQL Server 实例上安装一次。 若要安装多个组件，则必须安装其他的 SQL Server 实例。

    在右侧窗格中显示所选功能的必备组件。 SQL Server 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。

    系统配置检查器将在安装继续之前检验计算机的系统状态。 选择“下一步”继续操作。

9. “磁盘空间要求”页计算指定的功能所需的磁盘空间，并将磁盘空间要求与正在运行安装程序的计算机上的可用磁盘空间进行比较。

10. 本文中的其余工作流取决于要安装的功能。 您可能不会看到所有的页面，具体取决于您进行的选择。

11. 在“服务器配置 - 服务帐户”页上指定 SQL Server 服务的登录帐户。 此页上配置的实际服务取决于您选择安装的功能。

    可以为所有的 SQL Server 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 您还可以指定是自动启动、手动启动还是禁用服务。 Microsoft 建议对各服务帐户进行单独配置，以便为每项服务提供最低特权，即向 SQL Server 服务授予它们完成各自任务所需要拥有的最低权限。 有关详细信息，请参阅 [服务器配置 - 服务帐户](./install-sql-server.md) 和 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。

    若要为此 SQL Server 实例中的所有服务帐户指定相同的登录帐户，请在页面底部的字段中提供凭据。

    **安全说明** ： [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]

    为 SQL Server 服务指定登录信息后，请选择“下一步”。

12. 使用“服务器配置 - 排序规则”选项卡为数据库引擎和 Analysis Services 指定非默认的排序规则。 有关详细信息，请参阅[服务器配置 - 排序规则](./install-sql-server.md)。

13. 使用“数据库引擎配置 - 帐户设置”页指定以下事项：  

    - 安全模式 - 为 SQL Server 实例选择 Windows 身份验证或混合模式身份验证。 如果选择“混合模式身份验证”，则必须为内置 SQL Server 系统管理员帐户提供强密码。

        在设备与 SQL Server 成功建立连接之后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。 有关详细信息，请参阅 [数据库引擎配置 - 服务器配置](./install-sql-server.md)。  

    - SQL Server 管理员 - 必须为 SQL Server 实例指定至少一个系统管理员。 若要添加正在运行 SQL Server 安装程序的帐户，请选择“添加当前用户”。 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 SQL Server 实例拥有管理员权限的用户、组或计算机列表。 有关详细信息，请参阅 [数据库引擎配置 - 服务器配置](./install-sql-server.md)。

    编辑完列表后，选择“确定”。 验证配置对话框中的管理员列表。 如果列表是完整的，请单击“下一步”。

14. 使用“数据库引擎配置 - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。  

    > [!IMPORTANT]
    > 如果指定非默认的安装目录，请确保安装文件夹对于此 SQL Server 实例是唯一的。 此对话框中的任何目录都不应与其他 SQL Server 实例的目录共享。

    有关详细信息，请参阅 [数据库引擎配置 - 数据目录](./install-sql-server.md)。

15. 使用“数据库引擎配置 - FILESTREAM”页为 SQL Server 实例启用 FILESTREAM。 有关 FILESTREAM 的详细信息，请参阅 [数据库引擎配置 - 文件流](./install-sql-server.md)。 若要继续操作，请选择“下一步”。

16. 使用“Analysis Services - 帐户预配”页指定服务器模式，以及对 Analysis Services 拥有管理员权限的用户或帐户。 服务器模式决定哪些内存和存储子系统用于服务器。 不同的解决方案类型在不同的服务器模式下运行。 如果您计划在服务器上运行多维数据集数据库，则选择默认选项“多维”和“数据挖掘”服务器模式。 对于管理员权限，必须为 Analysis Services 指定至少一个系统管理员。 若要添加正在运行 SQL Server 安装程序的帐户，请选择“添加当前用户”。 若要在系统管理员列表中添加或删除帐户，请先选择“添加”或“删除”，再编辑对 Analysis Services 拥有管理员权限的用户、组或计算机列表。 有关服务器模式和管理员权限的详细信息，请参阅 [Analysis Services 配置-帐户设置](./install-sql-server.md)。

    编辑完列表后，选择“确定”。 验证配置对话框中的管理员列表。 如果列表是完整的，请单击“下一步”。

17. 使用“Analysis Services - 数据目录”页指定非默认安装目录。 若要安装到默认目录，请选择“下一步”。

    > [!IMPORTANT]
    > 如果指定非默认的安装目录，请确保安装文件夹对于此 SQL Server 实例是唯一的。 此对话框中的任何目录都不应与其他 SQL Server 实例的目录共享。

    有关详细信息，请参阅 [Analysis Services 配置 - 数据目录](./install-sql-server.md)。

18. 使用“Reporting Services 配置”页可以指定要创建的 Reporting Services 安装的类型。 有关 Reporting Services 配置模式的详细信息，请参阅 [Reporting Services 配置选项 (SSRS)](./install-sql-server.md)。

19. 使用“Distributed Replay 控制器配置”页可以指定您希望向其授予针对 Distributed Replay 控制器服务的管理权限的用户。 具有管理权限的用户将可以不受限制地访问 Distributed Replay 控制器服务。

    选择“添加当前用户”按钮可以添加要向其授予针对 Distributed Replay 控制器服务的访问权限的用户。 选择“添加”按钮可以添加针对 Distributed Replay 控制器服务的访问权限。 选择“删除”按钮，可撤消 Distributed Replay 控制器服务访问权限。

    若要继续操作，请选择“下一步”。

20. 使用“ Distributed Replay 客户端配置”页可以指定您希望向其授予针对 Distributed Replay 客户端服务的管理权限的用户。 具有管理权限的用户将可以不受限制地访问 Distributed Replay 客户端服务。

    “控制器名称”是一个可选参数，并且默认值为 \<*blank*>。 输入客户端计算机将与 Distributed Replay 客户端服务进行通信的控制器的名称。 注意以下事项：

    - 如果您已经设置了一个控制器，则在配置每个客户端时输入该控制器的名称。

    - 如果您尚未设置控制器，则可以将控制器名称保留为空白。 但是，您必须在 **“客户端配置”** 文件中手动输入控制器名称。

    为 Distributed Replay 客户端服务指定 **“工作目录”** 。 默认工作目录为 \<*drive letter*>:\Program Files\Microsoft\SQL Server\DReplayClient\WorkingDir。

    为 Distributed Replay 客户端服务指定 **“结果目录”** 。 默认结果目录为 \<*drive letter*>:\Program Files\Microsoft\SQL Server\DReplayClient\ResultDir。

    若要继续操作，请选择“下一步”。

21. 在“错误报告”页上指定要发送到 Microsoft 以帮助改善 SQL Server 的信息。 默认情况下，将启用用于错误报告的选项。

22. 系统配置检查器将再运行一组规则来针对你指定的 SQL Server 功能验证你的计算机配置。  

23. “准备安装”页显示安装期间指定的安装选项的树视图。 在此页上，安装程序指示是启用还是禁用产品更新功能以及最终的更新版本。 选择“安装”后，SQL Server 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  

24. 在安装过程中，“安装进度”页会提供相应的状态，因此您可以在安装过程中监视安装进度。  

25. 安装完成后，“完成”页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 SQL Server 安装过程，请选择“关闭”。

26. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

## <a name="next-steps"></a>后续步骤

配置 SQL Server 安装

- 为了减少系统的可攻击外围应用，SQL Server 将有选择地安装和激活一些密钥服务和功能。 有关详细信息，请参阅 [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md)。

## <a name="see-also"></a>另请参阅
- [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [验证 SQL Server 安装](../../database-engine/install-windows/validate-a-sql-server-installation.md)
- [修复失败的 SQL Server 2016 安装](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
- [使用安装向导升级 SQL Server（安装程序）](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
- [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)
