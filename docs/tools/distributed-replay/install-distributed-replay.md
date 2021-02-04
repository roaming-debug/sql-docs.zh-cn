---
title: 安装 Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: 本文介绍如何安装 Distributed Replay：使用安装向导、命令提示符窗口或配置文件。
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea1171da-f50e-4f16-bedc-5e468a46477f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4909975e83ea4438d969685ed9b76ae9f02a5631
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077075"
---
# <a name="install-distributed-replay"></a>安装 Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

可以采用以下三种方法之一安装 Distributed Replay：  
  
-   [从安装向导安装 Distributed Replay](#bkmk_wizard)  
  
-   [从命令提示符安装 Distributed Replay](#bkmk_command_prompt)  
  
-   [使用配置文件安装 Distributed Replay](#bkmk_configuration_file)  
  
##  <a name="install-distributed-replay-from-the-installation-wizard"></a><a name="bkmk_wizard"></a> 从安装向导安装 Distributed Replay  
 使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Distributed Replay 功能。 在计划安装这些功能的位置时，请考虑以下方面：  
  
-   您可以将管理工具与 Distributed Replay 控制器安装在同一台计算机上，也可以安装在不同的计算机上。  
  
-   在每个 Distributed Replay 环境中只能有一个控制器。  
  
-   您可以将客户端服务最多安装在 16 个（物理或虚拟）计算机上。  
  
-   只能有客户端服务的一个实例安装在 Distributed Replay 控制器计算机上。 如果您的 Distributed Replay 环境将具有多个客户端，我们建议不要将客户端服务与该控制器安装在同一台计算机上。 这样做可能会降低 Distributed Replay 的整体速度。  
  
-   对于性能测试方案，我们建议不要将管理工具、分布式重播实用工具控制器服务或客户端服务安装在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的目标实例上。 在目标服务器上安装所有这些功能应限于应用程序兼容性功能测试。  
  
-   在安装后，控制器服务（即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器）必须首先运行，然后才能在客户端启动 Distributed Replay 客户端服务。  
  
> [!NOTE]  
>  若要删除或更改 Distributed Replay 功能，请使用 **“控制面板”** 中的 Windows **“程序和功能”** 窗口。 在“卸载或更改程序” [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 窗口中选择  ，然后单击 **“删除”** 以便打开 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装向导。 在 **“选择功能”** 页上，选中要删除的 Distributed Replay 功能。  
  
 **先决条件：**  
  
-   请确保您要使用的计算机满足在 [Distributed Replay Requirements](../../tools/distributed-replay/distributed-replay-requirements.md)主题中介绍的要求。  
  
-   在开始此过程之前，请创建运行控制器和客户端服务所基于的域用户帐户。 我们建议这些帐户不是 Windows Administrators 组的成员。 有关详细信息，请参阅 [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md) 主题中的“用户和服务帐户”部分。  
  
    > [!NOTE]  
    >  如果您在同一台计算机上运行管理工具、控制器服务和客户端服务，则可以使用本地用户帐户。  
  
 **安装位置：**  
  
 假定您在使用默认文件位置和标准安装，则基目录位于 C:\Program Files\Microsoft SQL Server。 其中，下面是二进制文件和程序集将安装到的位置：  
  
-   在 32 位系统上：  
  
     [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]工具  
  
     \- 或 -  
  
     \<Share Feature Directory>\Tools\\（用户提供的替代共享功能目录）  
  
-   在 64 位系统上：  
  
     C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (x86)\130\Tools  
  
     \- 或 -  
  
     \<Share Feature Directory (x86)>\Tools\\（用户提供的替代共享功能 (x86) 目录）  
  
#### <a name="to-install-distributed-replay-features"></a>安装 Distributed Replay 功能  
  
1.  若要开始安装任何 Distributed Replay 功能，请启动 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装向导。  
  
2.  **“安装程序支持规则”** 页将标识在安装 SQL Server 安装程序支持文件时可能发生的问题。 在继续安装之前，您必须纠正任何安装程序支持问题。  
  
3.  在 **“产品密钥”** 页上，选择某一选项按钮，该按钮指示您是安装免费版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，还是安装具有 PID 密钥的产品的生产版本。 有关详细信息，请参阅 [SQL Server 2016 的版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
4.  在 **“许可条款”** 页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。  
  
5.  在 **“安装程序支持文件”** 页，单击 **“安装”** 以便安装或更新 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的安装程序支持文件。  
  
6.  在 **“设置角色”** 页上，选择 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能安装”** ，然后单击 **“下一步”** 以继续进入 **“功能选择”** 页。  
  
7.  在 **“功能选择”** 页上，配置要安装的功能。  
  
    -   若要安装管理工具，请选择“管理工具 - 基本”  。  
  
    -   若要安装控制器服务，请选择 **“Distributed Replay 控制器”** 。  
  
    -   若要安装客户端服务，请选择 **“Distributed Replay 客户端”** 。  
  
     **重要说明**：在你配置 Distributed Replay 控制器时，可以指定将用于运行 Distributed Replay 客户端服务的一个或多个用户帐户。 下面是支持的帐户的列表：  
  
    -   域用户帐户  
  
    -   用户创建的本地用户帐户  
  
    -   管理员  
  
    -   虚拟帐户和 MSA（托管服务帐户）  
  
    -   Network Services、Local Services 和 System  
  
     不接受组帐户（本地或域）和其他内置帐户（如 Everyone）。  
  
8.  （可选）单击省略号 (...) 按钮，以更改共享功能目录路径。  
  
    1.  在 32 位计算机上，默认安装路径为 **C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
    2.  在 64 位计算机上，默认安装路径为 C:\Program Files (x86)\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\   
  
9. 完成后，单击“下一步”。   
  
10. 在 **“安装规则”** 页上， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将验证您的计算机配置。 在验证过程完成后，单击 **“下一步”** 。  
  
11. **“磁盘空间要求”** 页计算指定的功能所需的磁盘空间， 然后将所需空间与可用磁盘空间进行比较。  
  
12. 在 **“错误报告”** 页上，指定要发送到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 以帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的信息。 默认情况下，将启用用于错误报告的选项。  
  
13. 在 **“安装配置规则”** 页上，系统配置检查器将运行多组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能对您的计算机配置进行验证。  
  
14. 在 **“准备安装程序”** 页上，单击 **“安装”** 。  
  
    > [!IMPORTANT]  
    >  安装 Distributed Replay 之后，您必须在控制器和客户端计算机上创建防火墙规则，并授予每台客户端计算机对目标服务器的权限。 有关详细信息，请参阅 [完成安装后步骤](../../tools/distributed-replay/complete-the-post-installation-steps.md)。  
  
### <a name="net-framework-security"></a>.NET Framework 安全性  
 您必须具有管理权限才能安装任何 Distributed Replay 功能。 只有拥有 sysadmin 权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名才可以将客户端服务帐户添加到测试服务器的 sysadmin 服务器角色中。 有关分布式重播的安全注意事项的详细信息，请参阅 [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md)。  
  
##  <a name="install-distributed-replay-from-the-command-prompt"></a><a name="bkmk_command_prompt"></a> 从命令提示符安装 Distributed Replay  
 通过从命令提示符安装 Distributed Replay 的新实例，您可以指定要安装的功能以及如何配置这些功能。 在命令提示符下安装支持对 Distributed Replay 组件进行安装、修复、升级和卸载。 通过命令提示符安装时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持完全静默模式（通过使用 /Q 参数）。  
  
> [!NOTE]  
>  对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。  
  
### <a name="installation-parameters"></a>安装参数  
 顶级功能列表包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]、 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]和工具。 工具功能将安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理工具、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书、 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]和其他共享组件。 若要安装 Distributed Replay 组件，请指定以下参数：  
  
|组件|参数|  
|---------------|---------------|  
|“Distributed Replay 控制器”|**DREPLAY_CTLR**|  
|Distributed Replay 客户端|**DREPLAY_CLT**|  
|管理工具|**工具**|  
  
> [!IMPORTANT]  
>  安装 Distributed Replay 之后，您必须在控制器和客户端计算机上创建防火墙规则，并授予每台客户端计算机对目标服务器的权限。 有关详细信息，请参阅 [完成安装后步骤](../../tools/distributed-replay/complete-the-post-installation-steps.md)。  
  
 使用下表中的参数可开发用于安装的命令行脚本。  
  
|参数|说明|支持的值|  
|---------------|-----------------|----------------------|  
|/CTLRSVCACCOUNT<br /><br /> **可选**|Distributed Replay 控制器服务的服务帐户。|检查帐户和密码|  
|/CTLRSVCPASSWORD<br /><br /> **可选**|Distributed Replay 控制器服务帐户的密码。|检查帐户和密码|  
|/CTLRSTARTUPTYPE<br /><br /> **可选**|Distributed Replay 控制器服务的启动类型。|自动<br /><br /> 已禁用<br /><br /> 手动|  
|/CTLRUSERS<br /><br /> **可选**|指定哪些用户对 Distributed Replay 控制器服务具有权限。|一组使用“ ”（空格）作为分隔符的用户帐户字符串<br /><br /> **重要说明**：在配置 Distributed Replay 控制器服务时，可以指定一个或多个用于运行 Distributed Replay 客户端服务的用户帐户。 下面是支持的帐户的列表：<br /><br /> 域用户帐户<br /><br /> 用户创建的本地用户帐户<br /><br /> 管理员<br /><br /> 管理员<br /><br /> 虚拟帐户和 MSA（托管服务帐户）<br /><br /> Network Services、Local Services 和 System<br /><br /> <br /><br /> 注意：不接受组帐户（本地或域）和其他内置帐户（如 Everyone）。|  
|/CLTSVCACCOUNT<br /><br /> **可选**|Distributed Replay 客户端服务的服务帐户。|检查帐户和密码|  
|/CLTSVCPASSWORD<br /><br /> **可选**|Distributed Replay 客户端服务帐户的密码。|检查帐户和密码|  
|/CLTSTARTUPTYPE<br /><br /> **可选**|Distributed Replay 客户端服务的启动类型。|自动<br /><br /> 已禁用<br /><br /> 手动|  
|/CLTCTLRNAME<br /><br /> **可选**|客户端就 Distributed Replay 控制器服务与之通信的计算机的名称。||  
|/CLTWORKINGDIR<br /><br /> **可选**|Distributed Replay 客户端服务的工作目录。|有效的路径|  
|/CLTRESULTDIR<br /><br /> **可选**|Distributed Replay 客户端服务的结果目录。|有效的路径|  
  
### <a name="sample-syntax"></a>示例语法：  
 **安装 Distributed Replay 控制器组件**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CTLR /IAcceptSQLServerLicenseTerms /CTLRUSERS="domain\user1" "domain\user2" /CTLRSVCACCOUNT="domain\svcuser" /CTLRSVCPASSWORD="password" /CTLRSTARTUPTYPE=Automatic  
```  
  
 **安装 Distributed Replay 客户端组件**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CLT /IAcceptSQLServerLicenseTerms /CLTSVCACCOUNT="domain\svcuser" /CLTSVCPASSWORD="password" /CLTSTARTUPTYPE=Automatic /CLTCTLRNAME=ControllerMachineName /CLTWORKINGDIR="C:\WorkingDir" /CLTRESULTDIR="C:\ResultDir  
```  
  
##  <a name="install-distributed-replay-using-a-configuration-file"></a><a name="bkmk_configuration_file"></a> 使用配置文件安装 Distributed Replay  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序提供基于用户输入和系统默认值生成配置文件的功能。 如果您指定要安装管理工具，则可以使用配置文件来部署三个 Distributed Replay 组件（管理工具、Distributed Replay 控制器和 Distributed Replay 客户端）。 它支持安装、修复和卸载 Distributed Replay 组件。  
  
 安装程序仅支持通过命令行使用配置文件。 下面列出了在使用配置文件时参数的处理顺序：  
  
-   配置文件覆盖包中的默认值  
  
-   命令行的值覆盖配置文件中的值  
  
 有关如何使用配置文件的详细信息，请参阅 [使用配置文件安装 SQL Server 2016](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)。  
  
> [!IMPORTANT]  
>  安装 Distributed Replay 之后，您必须在控制器和客户端计算机上创建防火墙规则，并授予每台客户端计算机对目标服务器的权限。 有关详细信息，请参阅 [完成安装后步骤](../../tools/distributed-replay/complete-the-post-installation-steps.md)。  
  
#### <a name="to-generate-a-configuration-file"></a>生成配置文件  
  
1.  按照安装向导的说明操作，直到出现 **“准备安装”** 页面。 配置文件的路径是在 **“准备安装”** 页的配置文件路径部分中指定的。  
  
2.  取消安装并且不要真正完成安装，以便生成 INI 文件。  
  
#### <a name="to-install-distributed-replay-using-the-configuration-file"></a>使用配置文件安装 Distributed Replay  
  
-   通过命令提示符运行安装并使用 ConfigurationFile 参数提供 ConfigurationFile.ini。  
  
 **示例语法**  
  
 下面是如何在命令提示符处指定配置文件的示例：  
  
```
Setup.exe /CTLRSVCPASSWORD="ctlrsvcpswd" /CLTSVCPASSWORD="cltsvcpswd" / ConfigurationFile=ConfigurationFile.INI\  
```

> [!NOTE]
> 您必须在命令行中指定这两个密码，因为您无法在配置文件中配置这些密码。  

## <a name="see-also"></a>另请参阅

- [SQL Server 2016 版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)
- [SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Distributed Replay 要求](../../tools/distributed-replay/distributed-replay-requirements.md)
- [管理工具命令行选项（Distributed Replay 实用工具）](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)
- [配置 Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)