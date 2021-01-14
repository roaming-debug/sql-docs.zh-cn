---
title: SQL Server 安装 - 命令提示符参数
description: 本文介绍 SQL Server 安装的命令参数。 你可以指定要安装和配置的功能。
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, command prompt
- installation scripts [SQL Server]
- maintenance scripts [SQL Server]
- REMOVENODE property
- components [SQL Server], removing
- command prompt [SQL Server], SQL Server installations
- ASACCOUNT parameter
- failover clustering [SQL Server], installing
- master database [SQL Server], rebuilding
- SQLCOLLATION parameter
- clusters [SQL Server], installing
- unattended installations [SQL Server]
- modifying collations
- AGTPASSWORD parameter
- USESYSDB parameter
- RSPASSWORD parameter
- AUTOSTART parameter
- ASPASSWORD parameter
- stand-alone installations [SQL Server]
- SAMPLEDATABASESERVER parameter
- adding components
- SAPWD parameter
- scripts [SQL Server], uninstallations
- remote installations [SQL Server]
- components [SQL Server], installing
- TARGETCOMPUTER parameter
- REMOVENODE parameter
- REINSTALLMODE parameter
- scripts [SQL Server], maintenance
- rebuilding registry
- SQLPASSWORD parameter
- rebuilding databases
- IP property
- PIDKEY parameter
- RSCONFIGURATION parameter
- ADDLOCAL parameter
- Setup [SQL Server], command prompt
- REBUILDDATABASE parameter
- SECURITYMODE parameter
- REMOVE property
- DISABLENETWORKPROTOCOLS parameter
- INSTALLDATADIR parameter
- REMOVE parameter
- removing components
- SQLACCOUNT parameter
- parameters [SQL Server], SQL Server installations
- UPGRADE parameter
- shortcuts [SQL Server]
- updating components
- removing SQL Server
- clustered instance of SQL Server
- INSTALLSQLDATADIR parameter
- RSACCOUNT parameter
- ADMINPASSWORD parameter
- GROUP property
- ERRORREPORTING property
- uninstallation scripts [SQL Server]
- AGTACCOUNT parameter
- SAVESYSDB parameter
- INSTALLVS parameter
- INSTANCENAME parameter
- scripts [SQL Server], installations
- rebuilding database, master
- uninstalling SQL Server
- ASCOLLATION parameter
- .ini files
- ADDNODE parameter
- command line installations [SQL Server]
- VS parameter
- INSTALLASDATADIR parameter
- INSTALLSQLDIR parameter
- nodes [Faillover Clustering], command prompt
- INSTALLSQLSHAREDDIR parameter
ms.assetid: df40c888-691c-4962-a420-78a57852364d
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 01/07/2020
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f3d0be0d607a8f6a810e9d1f24fbbbff1dc11a69
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005363"
---
# <a name="install-sql-server-from-the-command-prompt"></a>从命令提示符安装 SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

在运行 SQL 安装程序之前，请查阅 [计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)。  

通过从命令提示符安装 SQL Server 的新实例，可以指定要安装的功能以及如何配置这些功能。 还可以指定与安装用户界面是进行静默交互、基本交互还是完全交互。  

若要通过命令提示符进行安装，请打开管理命令提示符，然后导航到 setup.exe 在 [SQL Server 安装介质](https://www.microsoft.com/sql-server/sql-server-downloads)中所处的位置。 运行 `setup.exe` 命令，以及完成尝试执行的操作所必需的和可选的参数：

`C:\SQLMedia\SQLServer2019> setup.exe /[Option] /[Option] = {value}`

以下示例以安静模式安装 SQL Server 数据库引擎、SQL Server Analysis Services、SQL Server Integration Services 和 SQL Server工具：

```console
C:\SQLMedia\SQLServer2019> setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION="install" /PID="AAAAA-BBBBB-CCCCC-DDDDD-EEEEE" /FEATURES=SQL,AS,IS,Tools
/INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="MyDomain\MyAccount"
/SQLSVCPASSWORD="************" /SQLSYSADMINACCOUNTS="MyDomain\MyAccount "
/AGTSVCACCOUNT="MyDomain\MyAccount" /AGTSVCPASSWORD="************"
/ASSVCACCOUNT="MyDomain\MyAccount" /ASSVCPASSWORD="************"
/ISSVCAccount="MyDomain\MyAccount" /ISSVCPASSWORD="************"
/ASSYSADMINACCOUNTS="MyDomain\MyAccount"
```

若要查看控制台内所有可能命令的列表，请使用 /help 标志运行可执行文件： 

```console
C:\SQLMedia\SQLServer2019> setup.exe /help
```

本文其余部分提供可用参数的详细说明。 

  
> [!NOTE]
> 通过命令提示符安装时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持完全静默模式（通过使用 /Q 参数）或简单静默模式（通过使用 /QS 参数）。 /QS 开关仅显示进度，不接受任何输入，也不显示错误消息（如果遇到）。 仅当指定 /Action=install 时才支持 /QS 参数。  
  
 无论使用哪种安装方法，你都需要作为个人或代表实体确认接受软件许可条款，除非你对于软件的使用受单独的协议（如 Microsoft 批量许可协议或与 ISV 或 OEM 之间的第三方协议）管辖。  
  
 将在安装程序用户界面中显示许可条款，供您审核审阅和接受。 使用 /Q 或 /QS 参数进行无人参与安装时，必须包含 /IACCEPTSQLSERVERLICENSETERMS 参数。 可以通过 [Microsoft Software License Terms](https://go.microsoft.com/fwlink/?LinkId=148209)（Microsoft 软件许可条款）单独查看许可条款。  
  
> [!NOTE] 
> 根据你接收软件的方式（例如，通过 Microsoft 批量许可），你对软件的使用可能受其他条款和条件约束。  
  
 在以下情况下支持命令提示符安装：  
  
-   在命令提示符下使用指定的语法和参数，在本地计算机上安装、升级或删除 SQL Server 的实例和共享组件。    
-   安装、升级或删除故障转移群集实例。   
-   从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的一个版本升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的另一个版本。    
-   在配置文件中使用指定的语法和参数，在本地计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 可以使用此方法将安装配置复制到多台计算机，或者安装故障转移群集系统的多个节点。  
  
 
> [!NOTE]
> 对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。 对于故障转移群集安装，您必须是本地管理员，并且有权作为服务登录并有权在所有故障转移群集节点上作为操作系统的一部分工作。  
  
##  <a name="proper-use-of-setup-parameters"></a><a name="ProperUse"></a>正确使用安装参数  
若要编写语法正确的安装命令，请遵循以下准则：  
  
-   /PARAMETER（示例：`/INDICATEPROGRESS`）
-   /PARAMETER=true/false（示例：`/SQLSVCINSTANTFILEINIT=True`）
-   /PARAMETER=1/0（适用于布尔类型，例如：`/TCPENABLED=1`）
-   对于所有单值参数，/PARAMETER="value"。 （示例：`/PID="PID" /SQLSVCSTARTUPTYPE="Automatic"`）
    - 对于需要路径的参数：支持 `/INSTANCEDIR=c:\Path` 或 `/INSTANCEDIR="c:\Path"`。  
-   对于所有多值参数，/PARAMETER="value1" "value2" "value3"。 （示例：`/SQLSYSADMINACCOUNTS="Contoso\John" "Contoso\Mary"`）
    - **例外**：`/FEATURES`，这是一个多值参数，但其格式为 `/FEATURES=AS,RS,IS`（无空格，使用逗号分隔） 

  
> [!IMPORTANT]  
> 在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，如果为 INSTANCEDIR 和 SQLUSERDBDIR 指定相同的目录路径，SQL Server 代理和全文搜索则会由于缺少权限而无法启动。  
  
> [!NOTE]  
> 关系服务器值支持路径的其他终止反斜杠格式（反斜杠或两个反斜杠字符）。  

以下各部分提供用于为安装、更新和修复方案开发命令行安装脚本的参数。  
  
列出的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件的参数特定于该组件。 安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]时，SQL Server 代理和 SQL Server Browser 参数适用。  


##  <a name="installation-parameters"></a><a name="Install"></a> 安装参数  
 使用下表中的参数开发用于安装的命令行脚本。  
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示安装工作流。<br /><br /> 支持的值：Install。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SUPPRESSPRIVACYSTATEMENTNOTICE<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|禁止显示隐私声明。 使用此标记即表示同意[隐私声明](../../sql-server/sql-server-privacy.md)。  |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Python 安装程序控件|/IACCEPTPYTHONLICENSETERMS <br /><br /> 仅在为包含 Anaconda Python 包的无人参与安装指定了 /Q 或 /QS 参数时才是必需的。|必需，用于确认接受许可条款。| 
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] R 安装程序控件|/IACCEPTROPENLICENSETERMS <br /><br /> 仅在为包含 Microsoft R Open 包的无人参与安装指定了 /Q 或 /QS 参数时才是必需的。|必需，用于确认接受许可条款。| 
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/UpdateEnabled<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将包含找到的更新。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/UpdateSource<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 `.\MyUpdates` 或一个 UNC 共享）。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将通过 Windows Server Update Services 搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update 或 Windows Update 服务。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和更高版本中无效。 <br/><br/> 要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 SQL Server 2016 和更高版本以将反馈发送到 Microsoft](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。<br /><br /> 支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> - 或 -<br /><br /> /ROLE<br /><br /> **必需**|指定要安装的组件。<br /><br /> 选择 **/FEATURES** 可指定要安装的各个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件。 有关详细信息，请参阅 [功能参数](#Feature) 。<br /><br /> 选择 **/ROLE** 可指定安装程序角色。 安装角色在预先确定的配置中安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示安装参数的用法选项。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTALLSHAREDDIR<br /><br /> **可选**|为 64 位共享组件指定一个非默认安装目录。<br /><br /> 默认为 `%Program Files%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files(x86)%\Microsoft SQL Server`|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTALLSHAREDWOWDIR<br /><br /> **可选**|为 32 位共享组件指定一个非默认安装目录。 仅在 64 位系统上受支持。<br /><br /> 默认为 `%Program Files(x86)%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files%\Microsoft SQL Server`|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为特定于实例的组件指定一个非默认安装目录。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> **可选**|为 [InstanceID](#InstanceID)指定一个非默认值。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 如果要配置包含头节点的 PolyBase 扩展计算组，请使用此选项。 支持的值：True、False |  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/Q 或 /QUIET<br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE <br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/UIMODE<br /><br /> **可选**|指定在安装过程中是否只提供最少数量的对话框。<br /><br /> **/UIMode** 只能与 **/ACTION=INSTALL** 和 **UPGRADE** 参数一起使用。 支持的值：<br /><br /> **/UIMODE=Normal** 对于非 Express 版本是默认值，它为所选功能提供所有安装程序对话框。<br /><br /> **/UIMODE=AutoAdvance** 对于 Express 版本是默认值，它跳过不重要的对话框。<br /><br /> <br /><br /> 请注意，当与其他参数组合时，将覆盖 **UIMODE** 。 例如，当同时提供了 **/UIMODE=AutoAdvance** 和 **/ADDCURRENTUSERASSQLADMIN=FALSE** 时，当前用户将不会自动填充设置对话框。<br /><br /> **UIMode** 设置不能与 **/Q** 或 **/QS** 参数结合使用。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理|/AGTSVCACCOUNT<br /><br /> **必需**|为 SQL Server 代理服务指定帐户。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理|/AGTSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQL Server 代理服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理|/AGTSVCSTARTUPTYPE<br /><br /> **可选**|为 SQL Server 代理服务指定 [启动](#Accounts) 模式。<br /><br /> 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASBACKUPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 备份文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的排序规则设置。<br /><br /> 默认值：Latin1_General_CI_AS|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCONFIGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASDATADIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASLOGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 日志文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSERVERMODE<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例的服务器模式。 有效值为 MULTIDIMENSIONAL、POWERPIVOT 或 TABULAR。 **ASSERVERMODE** 区分大小写。 所有值必须以大写形式表示。 有关有效值的详细信息，请参阅 [安装 Analysis Services](/analysis-services/instances/install-windows/install-analysis-services)。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的帐户。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCSTARTUPTYPE<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSYSADMINACCOUNTS<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的管理员凭据。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASTEMPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 临时文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server \<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASPROVIDERMSOLAP<br /><br /> **可选**|指定 MSOLAP 提供程序是否可以在进程中运行。<br /><br /> 默认值：1=启用|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/FARMACCOUNT<br /><br /> **对于 SPI_AS_NewFarm 是必需的**|指定用于在场中运行 SharePoint 管理中心服务和其他重要服务的域用户帐户。<br /><br /> 此参数仅用于通过 /ROLE = SPI_AS_NEWFARM 安装的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/FARMPASSWORD<br /><br /> **对于 SPI_AS_NewFarm 是必需的**|指定场帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/PASSPHRASE<br /><br /> **对于 SPI_AS_NewFarm 是必需的**|指定用于向 SharePoint 场添加其他应用程序服务器或 Web 前端服务器的通行短语。<br /><br /> 此参数仅用于通过 /ROLE = SPI_AS_NEWFARM 安装的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/FARMADMINIPORT<br /><br /> **对于 SPI_AS_NewFarm 是必需的**|指定用于连接 SharePoint 管理中心 Web 应用程序的端口。<br /><br /> 此参数仅用于通过 /ROLE = SPI_AS_NEWFARM 安装的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。|  
|SQL Server Browser|/BROWSERSVCSTARTUPTYPE<br /><br /> **可选**|指定 SQL Server Browser 服务的 [启动](#Accounts) 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/ENABLERANU<br /><br /> **可选**|为 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 安装启用运行身份凭据。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/INSTALLSQLDATADIR<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件的数据目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SAPWD<br /><br /> **/SECURITYMODE=SQL 时是必需的**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SA 帐户的密码。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SECURITYMODE<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安全模式。<br /><br /> 如果未提供此参数，则支持仅 Windows 身份验证模式。<br /><br /> 支持的值：**SQL**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLBACKUPDIR<br /><br /> **可选**|指定备份文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Backup`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的排序规则设置。<br /><br /> 默认安装设置由操作系统 (OS) 区域设置确定。 服务器级排序规则可以在安装期间更改，也可以在安装前通过更改 OS 区域设置进行更改。 默认排序规则设置为与每个特定区域设置关联的最早可用版本。 这是出于向后兼容的原因。 因此，不推荐总是使用默认排序规则。 更改 Windows 排序规则的默认安装设置可充分利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 例如，对于 OS 区域设置“英语(美国)”（代码页 1252），安装过程中的默认排序规则是 SQL_Latin1_General_CP1_CI_AS，可将其更改为最接近的 Windows 对等排序规则 Latin1_General_100_CI_AS_SC  。 <br /><br />有关详细信息，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/ADDCURRENTUSERASSQLADMIN<br /><br /> **可选**|将当前用户添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 固定服务器角色。 当安装 Express 版本或使用 /Role=ALLFeatures_WithDefaults 时，可以使用 /ADDCURRENTUSERASSQLADMIN 参数。 有关详细信息，请参阅下面的 /ROLE。<br /><br /> 使用 /ADDCURRENTUSERASSQLADMIN 是可选的，但 /ADDCURRENTUSERASSQLADMIN 或 /SQLSYSADMINACCOUNTS 是必需的。 默认值：<br /><br /> **True** True [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]<br /><br /> 对于所有其他版本则为 **False**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQLSVCACCOUNT 的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。 |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCSTARTUPTYPE<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSYSADMINACCOUNTS<br /><br /> **必需**|使用此参数可将登录帐户设置为 sysadmin 角色的成员。<br /><br /> 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之外的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 版本，/SQLSYSADMINACCOUNTS 是必需的。 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]的各版本，使用 /SQLSYSADMINACCOUNTS 是可选的，但 /SQLSYSADMINACCOUNTS 或 /ADDCURRENTUSERASSQLADMIN 是必需的。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBDIR<br /><br /> **可选**|指定 tempdb 数据文件的目录。 指定多个目录时，请用空格将目录隔开。 如果指定了多个目录，则 tempdb 数据文件将以轮循机制的方式分布在目录中。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> **注意：** 此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGDIR<br /><br /> **可选**|指定 tempdb 日志文件的目录。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILECOUNT<br /><br /> **可选**|指定要由安装程序添加的 tempdb 数据文件的数量。 此值可以增加至内核的数量。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 1<br /><br /> 8 或内核的数量，对所有其他版本来说都较低<br /><br /> 重要提示：tempdb 的主数据库文件依然为 tempdb.mdf。 将其他 tempdb 文件命名为 tempdb_mssql_#.ndf，其中 # 代表在安装期间创建的每个其他 tempdb 数据文件的唯一编号。 此命名约定的目的是使它们具有唯一性。 卸载 SQL Server 的实例将删除具有命名约定 tempdb_mssql_ #.ndf 的文件。 不要对用户数据库文件使用 tempdb_mssql_\*.ndf 命名约定。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 数据文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64。 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 日志文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64。 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBDIR<br /><br /> **可选**|指定用户数据库的数据文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCINSTANTFILEINIT<br /><br /> **可选**|启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的即时文件初始化。 有关安全性和性能注意事项，请参阅 [数据库实例文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。<br /><br /> 默认值：“False”<br /><br /> 可选值：“True”|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBLOGDIR<br /><br /> **可选**|指定用户数据库的日志文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLMAXDOP=parameter <br /><br /> **可选** <br /> 如果在无人参与（无提示）安装中被省略，MAXDOP 遵循[“最大并行度”准则](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)。 |指定最大并行度，它决定了在执行一个语句期间一个语句可以使用的处理器数。 仅从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始可用。 <br /><br /> 默认值遵循[最大并行度准则](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)|
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/USESQLRECOMMENDEDMEMORYLIMITS<br /><br /> **可选** <br /> 如果 /USESQLRECOMMENDEDMEMORYLIMITS、/SQLMINMEMORY 和 /SQLMAXMEMORY 在无人参与（无提示）安装中被省略，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存配置。|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将使用计算出的建议值，建议值遵循独立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的[服务器内存配置准则](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually)。 仅从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始可用。<br /><br /> **注意：** 此参数无法与 /SQLMINMEMORY 和 /SQLMAXMEMORY 结合使用。 |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLMINMEMORY<br /><br /> **可选** <br /> 如果 /USESQLRECOMMENDEDMEMORYLIMITS、/SQLMINMEMORY 和 /SQLMAXMEMORY 在无人参与（无提示）安装中被省略，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存配置。|以 MB 为单位指定最小服务器内存配置。 仅从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始可用。<br /><br /> 默认值：0。<br /><br /> **注意：** 此参数无法与 /USESQLRECOMMENDEDMEMORYLIMITS 结合使用。 |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLMAXMEMORY<br /><br /> **可选** <br /> 如果 /USESQLRECOMMENDEDMEMORYLIMITS、/SQLMINMEMORY 和 /SQLMAXMEMORY 在无人参与（无提示）安装中被省略，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存配置。|以 MB 为单位指定最大服务器内存配置。 仅从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始可用。<br /><br /> 默认值：计算出的建议值，遵循独立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的[服务器内存配置准则](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually)。<br /><br /> **注意：** 此参数无法与 /USESQLRECOMMENDEDMEMORYLIMITS 结合使用。 |  
|FILESTREAM|/FILESTREAMLEVEL<br /><br /> **可选**|指定 FILESTREAM 功能的访问级别。 支持的值：<br /><br /> 0＝禁用此实例的 FILESTREAM 支持。 （默认值）<br /><br /> 1=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。<br /><br /> 2=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和文件 I/O 流访问启用 FILESTREAM。 （对于群集方案无效）<br /><br /> 3＝允许远程客户端针对 FILESTREAM 数据启用流访问。|  
|FILESTREAM|/FILESTREAMSHARENAME<br /><br /> **可选**<br /><br /> **当 FILESTREAMLEVEL 大于 1 时是必需的。**|指定用来存储 FILESTREAM 数据的 Windows 共享的名称。|  
|SQL Server 全文|/FTSVCACCOUNT<br /><br /> **可选**|指定全文筛选器启动器服务的帐户。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 ServiceSID 用于保护 SQL Server 与全文筛选器后台程序之间的通信。 如果未提供这些值，则将禁用全文筛选器启动器服务。 您必须使用 SQL Server 控制管理器来更改服务帐户和启用全文功能。<br /><br /> 默认值：Local Service 帐户|  
|SQL Server 全文|/FTSVCPASSWORD<br /><br /> **可选**|指定全文筛选器启动器服务的密码。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的帐户。<br /><br /> 默认值：NT AUTHORITY\NETWORK SERVICE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCStartupType<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 模式。|  
|SQL Server 网络配置|/NPENABLED<br /><br /> **可选**|指定 SQL Server 服务的 Named Pipes 协议的状态。 支持的值：<br /><br /> 0＝禁用 Named Pipes 协议<br /><br /> 1＝启用 Named Pipes 协议|  
|SQL Server 网络配置|/TCPENABLED<br /><br /> **可选**|指定 SQL Server 服务的 TCP 协议的状态。 支持的值：<br /><br /> 0＝禁用 TCP 协议<br /><br /> 1＝启用 TCP 协议|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **可选**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。 支持的值：<br /><br /> **SharePointFilesOnlyMode**<br /><br /> **DefaultNativeMode**<br /><br /> **FilesOnlyMode**<br /><br /> <br /><br /> 注意：如果安装包括 SQL Server[!INCLUDE[ssDE](../../includes/ssde-md.md)]，则默认的 RSINSTALLMODE 为 DefaultNativeMode。<br /><br /> 如果安装不包括 SQL Server[!INCLUDE[ssDE](../../includes/ssde-md.md)]，则默认的 RSINSTALLMODE 为 FilesOnlyMode。<br /><br /> 如果选择 DefaultNativeMode，但安装不包括 SQL Server[!INCLUDE[ssDE](../../includes/ssde-md.md)]，则安装会自动将 RSINSTALLMODE 更改为 FilesOnlyMode。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCACCOUNT<br /><br /> **必需**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的启动帐户。 |  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCPASSWORD<br /><br /> [必需](#Accounts)|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的启动帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCStartupType<br /><br /> **可选**|从 SQL Server 2017 开始不再适用。  指定 [的](#Accounts) 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|Python/机器学习服务（数据库内）|/MPYCACHEDIRECTORY|保留供将来使用。 使用 %TEMP% 存储 Python .CAB 文件，以便在没有 Internet 连接的计算机上安装。 |  
|R/机器学习服务（数据库内）|/MRCACHEDIRECTORY|使用此参数为 Microsoft R Open、SQL Server 2016 R Services、SQL Server 2016 R Server（独立版）或者 SQL Server 机器学习服务或机器学习服务器（独立版）中的 R 功能支持指定缓存目录。 从[没有 Internet 访问的计算机上的命令行](../../machine-learning/install/sql-ml-component-install-without-internet-access.md)安装 R 组件时，通常使用此设置。|  
|Java/语言扩展| /SQL_INST_JAVA,<br /> /SQLJAVADIR = "path"<br /><br /> **可选** | 从 SQL Server 2019 开始，指定通过语言扩展安装 Java。 如果提供的 /SQL_INST_JAVA 不带 /SQLJAVADIR 参数，则表示假设你要安装由安装介质提供的 Zulu Open JRE。 <br /><br /> 为 /SQLJAVADIR 提供路径表示你要使用已安装的 JRE 或 JDK。 |
  
###### <a name="sample-syntax"></a>示例语法：  
 安装新的具有 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]、复制和全文搜索组件的独立实例并启用 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的即时文件初始化。 
  
```  
setup.exe /q /ACTION=Install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSVCINSTANTFILEINIT="True" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
##  <a name="sysprep-parameters"></a><a name="SysPrep"></a> 参数  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 的详细信息，请参阅  
  
 [使用 SysPrep 安装 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]](../../database-engine/install-windows/install-sql-server-using-sysprep.md)。 
  
#### <a name="prepare-image-parameters"></a>准备映像参数  
 使用下表中的参数可开发用于准备但不配置 SQL Server 实例的命令行脚本。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示安装工作流。<br /><br /> 支持的值：PrepareImage|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/UpdateEnabled<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/UpdateSource<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 `.\MyUpdates` 或一个 UNC 共享）。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将通过 Windows Server Update Services 搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update 或 Windows Update 服务。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> **必需**|指定要安装的 [组件](#Feature) 。<br /><br /> 支持的值有：SQLEngine、Replication、FullText、DQ、AS、AS_SPI、RS、RS_SHP、RS_SHPWFE、DQC、Conn、IS、BC、SDK、DREPLAY_CTLR、DREPLAY_CLT、SNAC_SDK、SQLODBC、SQLODBC_SDK、LocalDB、MDS、POLYBASE|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示安装参数的用法选项。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTALLSHAREDDIR<br /><br /> **可选**|为 64 位共享组件指定一个非默认安装目录。<br /><br /> 默认为 `%Program Files%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files(x86)%\Microsoft SQL Server`|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为特定于实例的组件指定一个非默认安装目录。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2（2013 年 1 月）之前，**必需**<br /><br /> 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 开始，**必需** 此参数才可获得实例功能。|指定正在准备的实例的 InstanceID。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE<br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
  
###### <a name="sample-syntax"></a>示例语法：  
 准备新的具有 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]、复制和全文搜索组件以及 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的独立实例。 
  
```  
setup.exe /q /ACTION=PrepareImage /FEATURES=SQL,RS /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
```  
  
#### <a name="complete-image-parameters"></a>完成映像参数  
 使用下表中的参数可开发用于完成和配置已准备好的 SQL Server 实例的命令行脚本。 
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示安装工作流。<br /><br /> 支持的值：CompleteImage|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示安装参数的用法选项。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2（2013 年 1 月）之前，**必需**<br /><br /> 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 起，**可选**|使用在准备映像步骤中指定的实例 ID。<br /><br /> 支持的值：已准备实例的 InstanceID。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2（2013 年 1 月）之前，**必需**<br /><br /> 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 起，**可选**|为正在完成的实例指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本的产品密钥。 如果未指定此参数，则使用 Evaluation。<br /><br /> **注意：** 如果安装的是 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]、[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with tools 或 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services，则会预定义 PID。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。 |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE<br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|SQL Server 代理|/AGTSVCACCOUNT<br /><br /> **必需**|为 SQL Server 代理服务指定帐户。|  
|SQL Server 代理|/AGTSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQL Server 代理服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|SQL Server 代理|/AGTSVCSTARTUPTYPE<br /><br /> **可选**|为 SQL Server 代理服务指定 [启动](#Accounts) 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|SQL Server Browser|/BROWSERSVCSTARTUPTYPE<br /><br /> **可选**|指定 SQL Server Browser 服务的 [启动](#Accounts) 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/ENABLERANU<br /><br /> **可选**|为 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 安装启用运行身份凭据。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/INSTALLSQLDATADIR<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件的数据目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SAPWD<br /><br /> **/SECURITYMODE=SQL 时是必需的**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SA 帐户的密码。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SECURITYMODE<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安全模式。<br /><br /> 如果未提供此参数，则支持仅 Windows 身份验证模式。<br /><br /> 支持的值：**SQL**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLBACKUPDIR<br /><br /> **可选**|指定备份文件的目录。<br /><br /> 默认值：<br /><br /> `<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Backup`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的排序规则设置。<br /><br /> 默认值基于您的 Windows 操作系统的区域设置。 有关详细信息，请参阅 [Collation Settings in Setup](../../relational-databases/collations/collation-and-unicode-support.md)（安装程序中的排序规则设置）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQLSVCACCOUNT 的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCSTARTUPTYPE<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSYSADMINACCOUNTS<br /><br /> **必需**|使用此参数可将登录帐户设置为 sysadmin 角色的成员。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBDIR<br /><br /> **可选**|指定 tempdb 数据文件的目录。 指定多个目录时，请用空格将目录隔开。 如果指定了多个目录，则 tempdb 数据文件将以轮循机制的方式分布在目录中。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGDIR<br /><br /> **可选**|指定 tempdb 日志文件的目录。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 数据文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64<br /><br /> 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILESIZE<br /><br /> **可选**|指定 tempdb 日志文件的初始大小 (MB)。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 4<br /><br /> 所有其他版本为 8<br /><br /> 允许的范围：最小 = 默认值（4 或 8），最大 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILEGROWTH<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 日志文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILECOUNT<br /><br /> **可选**|指定要由安装程序添加的 tempdb 数据文件的数量。 此值可以增加至内核的数量。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 1<br /><br /> 8 或内核的数量，对所有其他版本来说都较低<br /><br /> **重要提示：** tempdb 的主数据库文件依然为 tempdb.mdf。 将其他 tempdb 文件命名为 tempdb_mssql_#.ndf，其中 # 代表在安装期间创建的每个其他 tempdb 数据文件的唯一编号。 此命名约定的目的是使它们具有唯一性。 卸载 SQL Server 的实例将删除具有命名约定 tempdb_mssql_ #.ndf 的文件。 不要对用户数据库文件使用 tempdb_mssql_\*.ndf 命名约定。<br /><br /> 警告：[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 不支持配置此参数。 安装程序仅安装 1 个 tempdb 数据文件。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBDIR<br /><br /> **可选**|指定用户数据库的数据文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBLOGDIR<br /><br /> **可选**|指定用户数据库的日志文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|FILESTREAM|/FILESTREAMLEVEL<br /><br /> **可选**|指定 FILESTREAM 功能的访问级别。 支持的值：<br /><br /> 0＝禁用此实例的 FILESTREAM 支持。 （默认值）<br /><br /> 1=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。<br /><br /> 2=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和文件 I/O 流访问启用 FILESTREAM。 （对于群集方案无效）<br /><br /> 3＝允许远程客户端针对 FILESTREAM 数据启用流访问。|  
|FILESTREAM|/FILESTREAMSHARENAME<br /><br /> **可选**<br /><br /> **当 FILESTREAMLEVEL 大于 1 时是必需的。**|指定用来存储 FILESTREAM 数据的 Windows 共享的名称。|  
|SQL Server 全文|/FTSVCACCOUNT<br /><br /> **可选**|指定全文筛选器启动器服务的帐户。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 ServiceSID 用于保护 SQL Server 与全文筛选器后台程序之间的通信。 如果未提供这些值，则将禁用全文筛选器启动器服务。 您必须使用 SQL Server 控制管理器来更改服务帐户和启用全文功能。<br /><br /> 默认值：Local Service 帐户|  
|SQL Server 全文|/FTSVCPASSWORD<br /><br /> **可选**|指定全文筛选器启动器服务的密码。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。 |  
|SQL Server 网络配置|/NPENABLED<br /><br /> **可选**|指定 SQL Server 服务的 Named Pipes 协议的状态。 支持的值：<br /><br /> 0＝禁用 Named Pipes 协议<br /><br /> 1＝启用 Named Pipes 协议|  
|SQL Server 网络配置|/TCPENABLED<br /><br /> **可选**|指定 SQL Server 服务的 TCP 协议的状态。 支持的值：<br /><br /> 0＝禁用 TCP 协议<br /><br /> 1＝启用 TCP 协议|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **可选**|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCACCOUNT<br /><br /> **必需**|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的启动帐户。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCPASSWORD<br /><br /> [必需](#Accounts)| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的启动帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCStartupType<br /><br /> **可选**|从 SQL Server 2017 开始不再适用。  指定 [的](#Accounts) 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
  
###### <a name="sample-syntax"></a>示例语法：  
 完成已准备的、包含 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]、复制和全文搜索组件的独立实例。 
  
```  
setup.exe /q /ACTION=CompleteImage /INSTANCENAME=MYNEWINST /INSTANCEID=<MYINST> /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
##  <a name="upgrade-parameters"></a><a name="Upgrade"></a> 升级参数  
 使用下表中的参数可开发用于升级的命令行脚本。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示安装工作流。 支持的值：<br /><br /> **升级**<br /><br /> **EditionUpgrade**<br /><br /> <br /><br /> **EditionUpgrade** 值用于将现有版本的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 升级到另一版本。 有关支持的版本升级的详细信息，请参阅 [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateEnabled*<br /><br /> **可选**|指定 SQL Server 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateSource*<br /><br /> **可选**|指定 SQL Server 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 .\MyUpdates）或一个 UNC 共享。 默认情况下，SQL Server 安装程序将通过 Windows Server 更新服务搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新或 Windows 更新服务。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为共享组件指定一个非默认安装目录。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> **在从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]** 或更高版本进行升级时为必需项<br /><br /> **从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级时为可选项**|为 [InstanceID](#InstanceID)指定一个非默认值。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q<br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/UIMODE<br /><br /> **可选**|指定在安装过程中是否只提供最少数量的对话框。 <br />                **/UIMode** 只能与 **/ACTION=INSTALL** 和 **UPGRADE** 参数一起使用。 支持的值：<br /><br /> **/UIMODE=Normal** 对于非 Express 版本是默认值，它为所选功能提供所有安装程序对话框。<br /><br /> **/UIMODE=AutoAdvance** 对于 Express 版本是默认值，它跳过不重要的对话框。<br /><br /> 请注意， **UIMode** 设置不能与 **/Q** 或 **/QS** 参数结合使用。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口将隐藏或关闭。|  
|SQL Server Browser 服务|/BROWSERSVCSTARTUPTYPE<br /><br /> **可选**|指定 SQL Server Browser 服务的 [启动](#Accounts) 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|SQL Server 全文|/FTUPGRADEOPTION<br /><br /> **可选**|指定全文目录升级选项。 支持的值：<br /><br /> **REBUILD**<br /><br /> **RESET**<br /><br /> **IMPORT**|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的帐户。<br /><br /> 默认值：NT AUTHORITY\NETWORK SERVICE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCStartupType<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSUPGRADEDATABASEACCOUNT<br /><br /> **可选**|仅当升级版本为 2008 R2 或更低版本的 SharePoint 模式报表服务器时才使用此属性。 对于使用较旧 SharePoint 模式体系结构（在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中已更改）的报表服务器，将执行额外的升级操作。 如果命令行安装中未附随此选项，则使用针对旧报表服务器实例的默认服务帐户。 如果使用此属性，则使用 **/RSUPGRADEPASSWORD** 属性提供帐户密码。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSUPGRADEPASSWORD<br /><br /> **可选**|现有 Report Server 服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/ALLOWUPGRADEFORSSRSSHAREPOINTMODE|升级基于 SharePoint 共享服务体系结构的 SharePoint 模式安装时需要该开关。 升级 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的非共享服务版本不需要该开关。|  
  
###### <a name="sample-syntax"></a>示例语法：  
 从以前的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 版本升级现有实例或故障转移群集节点，  
  
```  
setup.exe /q /ACTION=upgrade /INSTANCEID = <INSTANCEID>/INSTANCENAME=MSSQLSERVER /RSUPGRADEDATABASEACCOUNT="<Provide a SQL Server logon account that can connect to the report server during upgrade>" /RSUPGRADEPASSWORD="<Provide a password for the report server upgrade account>" /ISSVCAccount="NT Authority\Network Service" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
##  <a name="repair-parameters"></a><a name="Repair"></a> 修复参数  
 使用下表中的参数可开发用于修复的命令行脚本。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示修复工作流。<br /><br /> 支持的值：Repair|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> **必需**|指定要修复的 [组件](#Feature) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
  
###### <a name="sample-syntax"></a>示例语法：  
 修复实例和共享组件。 
  
```  
setup.exe /q /ACTION=Repair /INSTANCENAME=<instancename>  
```  
  
##  <a name="rebuild-system-database-parameters"></a><a name="Rebuild"></a> 重新生成系统数据库参数  
 使用下表中的参数可开发命令行脚本来重新生成 master、model、msdb 和 tempdb 系统数据库。 有关详细信息，请参阅 [重新生成系统数据库](../../relational-databases/databases/rebuild-system-databases.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示重新生成数据库工作流。<br /><br /> 支持的值：Rebuilddatabase|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLCOLLATION<br /><br /> **可选**|指定新的服务器级排序规则。<br /><br /> 默认值基于您的 Windows 操作系统的区域设置。 有关详细信息，请参阅 [Collation Settings in Setup](../../relational-databases/collations/collation-and-unicode-support.md)（安装程序中的排序规则设置）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SAPWD<br /><br /> **在安装实例的过程中指定了 /SECURITYMODE=SQL 时是必需的。**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SA 帐户的密码。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSYSADMINACCOUNTS<br /><br /> **必需**|使用此参数可将登录帐户设置为 sysadmin 角色的成员。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBDIR<br /><br /> **可选**|指定 tempdb 数据文件的目录。 指定多个目录时，请用空格将目录隔开。 如果指定了多个目录，则 tempdb 数据文件将以轮循机制的方式分布在目录中。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGDIR<br /><br /> **可选**|指定 tempdb 日志文件的目录。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> **注意：** 此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILECOUNT<br /><br /> **可选**|指定要由安装程序添加的 tempdb 数据文件的数量。 此值可以增加至内核的数量。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 1<br /><br /> 8 或内核的数量，对所有其他版本来说都较低<br /><br /> **重要提示：** tempdb 的主数据库文件依然为 tempdb.mdf。 将其他 tempdb 文件命名为 tempdb_mssql_#.ndf，其中 # 代表在安装期间创建的每个其他 tempdb 数据文件的唯一编号。 此命名约定的目的是使它们具有唯一性。 卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例会删除具有命名约定 tempdb_mssql_ #.ndf 的文件。 不要对用户数据库文件使用 tempdb_mssql_\*.ndf 命名约定。<br /><br /> 警告：[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 不支持配置此参数。 安装程序仅安装 1 个 tempdb 数据文件。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 数据文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64<br /><br /> 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILESIZE<br /><br /> **可选**|指定 tempdb 日志文件的初始大小 (MB)。 安装程序允许的大小最大为 1024。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 4<br /><br /> 所有其他版本为 8<br /><br /> 允许的范围：最小 = 默认值（4 或 8），最大 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILEGROWTH<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 日志文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
  
##  <a name="uninstall-parameters"></a><a name="Uninstall"></a> 卸载参数  
 使用下表中的参数可开发用于卸载的命令行脚本。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示卸载工作流。<br /><br /> 支持的值：**卸载**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> **必需**|指定要卸载的 [组件](#Feature) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, H,?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
  
###### <a name="sample-syntax"></a>示例语法：  
 卸载现有 SQL Server 实例。 
  
```  
setup.exe /Action=Uninstall /FEATURES=SQL,AS,RS,IS,Tools /INSTANCENAME=MSSQLSERVER  
```  
  
 要删除命名实例，请指定实例名称，而不是本文前面提到的示例中的“MSSQLSERVER”。 
  
##  <a name="failover-cluster-parameters"></a><a name="ClusterInstall"></a> 故障转移群集参数  
 安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集实例之前，请先查看以下文章：  
  
-   [安装 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
-   [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)
  
-   [安装故障转移群集前的准备工作](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
-   [AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
    > [!IMPORTANT]  
    > 所有故障转移群集安装命令都需要使用一个基础 Windows 群集。 所有将要加入 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集的节点都必须属于同一 Windows 群集的一部分。 
  
 请根据您所在单位的需要对以下故障转移群集安装脚本进行测试和修改。 
  
#### <a name="integrated-install-failover-cluster-parameters"></a>集成安装故障转移群集参数  
 使用下表中的参数可开发用于故障转移群集安装的命令行脚本。 
  
 有关集成安装的详细信息，请参阅[AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。 
  
> [!NOTE]
> 若要在安装完成后添加更多节点，请使用[添加节点](#AddNode)操作。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|详细信息|  
|-----------------------------------------|---------------|-------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示故障转移群集安装工作流：<br /><br /> 支持的值：InstallFailoverCluster|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERGROUP<br /><br /> **可选**|指定要用于 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集的资源组的名称。 可以是现有群集组的名称，也可以是新资源组的名称。<br /><br /> 默认值：<br /><br /> SQL Server (\<InstanceName>)|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。 |  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateEnabled*<br /><br /> **可选**|指定 SQL Server 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateSource*<br /><br /> **可选**|指定 SQL Server 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 .\MyUpdates）或一个 UNC 共享。 默认情况下，SQL Server 安装程序将通过 Windows Server 更新服务搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新或 Windows 更新服务。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> **必需**|指定要安装的 [组件](#Feature) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTALLSHAREDDIR<br /><br /> **可选**|为 64 位共享组件指定一个非默认安装目录。<br /><br /> 默认为 `%Program Files%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files(x86)%\Microsoft SQL Server`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTALLSHAREDWOWDIR<br /><br /> **可选**|为 32 位共享组件指定一个非默认安装目录。 仅在 64 位系统上受支持。<br /><br /> 默认为 `%Program Files(x86)%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files%\Microsoft SQL Server`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为特定于实例的组件指定一个非默认安装目录。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> **可选**|为 [InstanceID](#InstanceID)指定一个非默认值。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE <br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口将隐藏或关闭。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERDISKS<br /><br /> **可选**|指定要包含在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集资源组中的共享磁盘的列表。<br /><br /> 默认值：第一个驱动器用作所有数据库的默认驱动器。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERIPADDRESSES<br /><br /> **必需**|指定编码的 IP 地址。 编码以分号 (;) 分隔并遵循格式 \<IP Type>;\<address>;\<network name>;\<subnet mask>。 支持的 IP 类型包括 DHCP、IPv4 和 IPv6。<br />可以指定多个故障转移群集 IP 地址，地址之间用空格分隔。 请看以下示例：<br /><br /> `FAILOVERCLUSTERIPADDRESSES=DEFAULT`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;172.16.0.0;ClusterNetwork1;172.31.255.255`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;2001:db8:23:1002:20f:1fff:feff:b3a3;ClusterNetwork1`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERNETWORKNAME<br /><br /> **必需**|指定新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集的网络名称。 此名称用于在网络中标识新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集实例。|  
|SQL Server 代理|/AGTSVCACCOUNT<br /><br /> **必需**|为 SQL Server 代理服务指定帐户。|  
|SQL Server 代理|/AGTSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQL Server 代理服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASBACKUPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 备份文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的排序规则设置。<br /><br /> 默认值：Latin1_General_CI_AS|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCONFIGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASDATADIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASLOGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 日志文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSYSADMINACCOUNTS<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的管理员凭据。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASTEMPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 临时文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASPROVIDERMSOLAP<br /><br /> **可选**|指定 MSOLAP 提供程序是否可以在进程中运行。<br /><br /> 默认值：1=启用|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSERVERMODE<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例的服务器模式。 群集方案中的有效值为 MULTIDIMENSIONAL 或 TABULAR。 **ASSERVERMODE** 区分大小写。 所有值必须以大写形式表示。 有关有效值的详细信息，请参阅“在表格模式下安装 Analysis Services”。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/INSTALLSQLDATADIR<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件的数据目录。<br /><br /> 必须指定数据目录且该目录必须位于共享群集磁盘上。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SAPWD<br /><br /> **/SECURITYMODE=SQL 时是必需的**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SA 帐户的密码。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SECURITYMODE<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安全模式。<br /><br /> 如果未提供此参数，则支持仅 Windows 身份验证模式。<br /><br /> 支持的值：**SQL**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLBACKUPDIR<br /><br /> **可选**|指定备份文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Backup`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的排序规则设置。<br /><br /> 默认值基于您的 Windows 操作系统的区域设置。 有关详细信息，请参阅 [Collation Settings in Setup](../../relational-databases/collations/collation-and-unicode-support.md)（安装程序中的排序规则设置）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQLSVCACCOUNT 的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSYSADMINACCOUNTS<br /><br /> **必需**|使用此参数可将登录帐户设置为 sysadmin 角色的成员。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBDIR<br /><br /> **可选**|指定用户数据库的数据文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBDIR<br /><br /> **可选**|指定 tempdb 数据文件的目录。 指定多个目录时，请用空格将目录隔开。 如果指定了多个目录，则 tempdb 数据文件将以轮循机制的方式分布在目录中。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGDIR<br /><br /> **可选**|指定 tempdb 日志文件的目录。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILECOUNT<br /><br /> **可选**|指定要由安装程序添加的 tempdb 数据文件的数量。 此值可以增加至内核的数量。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 1<br /><br /> 8 或内核的数量，对所有其他版本来说都较低<br /><br /> **重要提示：** tempdb 的主数据库文件依然为 tempdb.mdf。 将其他 tempdb 文件命名为 tempdb_mssql_#.ndf，其中 # 代表在安装期间创建的每个其他 tempdb 数据文件的唯一编号。 此命名约定的目的是使它们具有唯一性。 卸载 SQL Server 的实例将删除具有命名约定 tempdb_mssql_ #.ndf 的文件。 不要对用户数据库文件使用 tempdb_mssql_\*.ndf 命名约定。<br /><br /> 警告：[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 不支持配置此参数。 安装程序仅安装 1 个 tempdb 数据文件。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 数据文件的初始大小。<br/><br/>默认值 = 8 MB。<br/><br/>最小值 = 8 MB。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64<br /><br /> 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILESIZE<br /><br /> **可选**|指定 tempdb 日志文件的初始大小 (MB)。 安装程序允许的大小最大为 1024。 <br /> 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 4<br /><br /> 所有其他版本为 8<br /><br /> 允许的范围：最小 = 默认值（4 或 8），最大 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILEGROWTH<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 日志文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBLOGDIR<br /><br /> **可选**|指定用户数据库的日志文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|FILESTREAM|/FILESTREAMLEVEL<br /><br /> **可选**|指定 FILESTREAM 功能的访问级别。 支持的值：<br /><br /> 0＝禁用此实例的 FILESTREAM 支持。 （默认值）<br /><br /> 1=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。<br /><br /> 2=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和文件 I/O 流访问启用 FILESTREAM。 （对于群集方案无效）<br /><br /> 3＝允许远程客户端针对 FILESTREAM 数据启用流访问。|  
|FILESTREAM|/FILESTREAMSHARENAME<br /><br /> **可选**<br /><br /> **当 FILESTREAMLEVEL 大于 1 时是必需的。**|指定用来存储 FILESTREAM 数据的 Windows 共享的名称。|  
|SQL Server 全文|/FTSVCACCOUNT<br /><br /> **可选**|指定全文筛选器启动器服务的帐户。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 ServiceSID 是用来帮助保护 SQL Server 和全文筛选器后台程序之间的通信。<br /><br /> 如果未提供这些值，则将禁用全文筛选器启动器服务。 您必须使用 SQL Server 控制管理器来更改服务帐户和启用全文功能。<br /><br /> 默认值：Local Service 帐户|  
|SQL Server 全文|/FTSVCPASSWORD<br /><br /> **可选**|指定全文筛选器启动器服务的密码。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的帐户。<br /><br /> 默认值：NT AUTHORITY\NETWORK SERVICE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCStartupType<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **可选**|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCACCOUNT<br /><br /> **必需**|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的启动帐户。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCPASSWORD<br /><br /> [必需](#Accounts)| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的启动帐户密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCStartupType<br /><br /> **可选**|从 SQL Server 2017 开始不再适用。  指定 [的](#Accounts) 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
  
 我们建议你使用服务 SID 来代替域组。 
  
##### <a name="additional-notes"></a>其他说明：  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 是唯一识别群集的组件。 其他功能不能识别群集，且不具有故障转移的高可用性。 
  
###### <a name="sample-syntax"></a>示例语法：  
 安装具有[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的单节点 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 故障转移群集实例（默认实例）。 
  
```  
setup.exe /q /ACTION=InstallFailoverCluster /InstanceName=MSSQLSERVER /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\UserName>" /ASDATADIR=<Drive>:\OLAP\Data /ASLOGDIR=<Drive>:\OLAP\Log /ASBACKUPDIR=<Drive>:\OLAP\Backup /ASCONFIGDIR=<Drive>:\OLAP\Config /ASTEMPDIR=<Drive>:\OLAP\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'" /FAILOVERCLUSTERNETWORKNAME="<Insert Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /Features=AS,SQL /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /SQLSYSADMINACCOUNTS="<DomainName\UserName> /IACCEPTSQLSERVERLICENSETERMS  
```  
  
#### <a name="prepare-failover-cluster-parameters"></a>准备故障转移群集参数  
 使用下表中的参数可开发用于故障转移群集准备的命令行脚本。 这是高级群集安装的第一步，在此步骤中您必须在故障转移群集的所有节点上准备故障转移群集实例。 有关详细信息，请参阅 [AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示故障转移群集准备工作流。<br /><br /> 支持的值：PrepareFailoverCluster|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateEnabled*<br /><br /> **可选**|指定 SQL Server 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateSource*<br /><br /> **可选**|指定 SQL Server 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 `.\MyUpdates` 或一个 UNC 共享）。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将通过 Windows Server Update Services 搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update 或 Windows Update 服务。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FEATURES<br /><br /> **必需**|指定要安装的 [组件](#Feature) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTALLSHAREDDIR<br /><br /> **可选**|为 64 位共享组件指定一个非默认安装目录。<br /><br /> 默认为 `%Program Files%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files(x86)%\Microsoft SQL Server`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTALLSHAREDWOWDIR<br /><br /> **可选**|为 32 位共享组件指定一个非默认安装目录。 仅在 64 位系统上受支持。<br /><br /> 默认为 `%Program Files(x86)%\Microsoft SQL Server`<br /><br /> 不能设置为 `%Program Files%\Microsoft SQL Server`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为特定于实例的组件指定一个非默认安装目录。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> **可选**|为 [InstanceID](#InstanceID)指定一个非默认值。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，<br /><br /> 则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE <br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|SQL Server 代理|/AGTSVCACCOUNT<br /><br /> **必需**|为 SQL Server 代理服务指定帐户。|  
|SQL Server 代理|/AGTSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQL Server 代理服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。 |  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的帐户。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT<br /><br /> **必需**|指定 SQL Server 服务的启动帐户。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQLSVCACCOUNT 的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|FILESTREAM|/FILESTREAMLEVEL<br /><br /> **可选**|指定 FILESTREAM 功能的访问级别。 支持的值：<br /><br /> 0＝禁用此实例的 FILESTREAM 支持。 （默认值）<br /><br /> 1=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。<br /><br /> 2=针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和文件 I/O 流访问启用 FILESTREAM。 （对于群集方案无效）<br /><br /> 3＝允许远程客户端针对 FILESTREAM 数据启用流访问。|  
|FILESTREAM|/FILESTREAMSHARENAME<br /><br /> **可选**<br /><br /> 当 FILESTREAMLEVEL 大于 1 时是 **必需** 的。|指定用来存储 FILESTREAM 数据的 Windows 共享的名称。|  
|SQL Server 全文|/FTSVCACCOUNT<br /><br /> **可选**|指定全文筛选器启动器服务的帐户。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 ServiceSID 是用来帮助保护 SQL Server 和全文筛选器后台程序之间的通信。<br /><br /> 如果未提供这些值，则将禁用全文筛选器启动器服务。 您必须使用 SQL Server 控制管理器来更改服务帐户和启用全文功能。<br /><br /> 默认值：Local Service 帐户|  
|SQL Server 全文|/FTSVCPASSWORD<br /><br /> **可选**|指定全文筛选器启动器服务的密码。<br /><br /> [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中忽略了此参数。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的帐户。<br /><br /> 默认值：NT AUTHORITY\NETWORK SERVICE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCStartupType<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **仅在“仅文件”模式下可用。**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCACCOUNT<br /><br /> **必需**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的启动帐户。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCPASSWORD<br /><br /> [必需](#Accounts)| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的启动帐户密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCStartupType<br /><br /> **可选**| 从 SQL Server 2017 开始不再适用。 指定 [的](#Accounts) 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
  
 我们建议你使用服务 SID 来代替域组。 
  
###### <a name="sample-syntax"></a>示例语法：  
 针对 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]执行故障转移群集高级安装方案的“准备”步骤。 
  
 在命令提示符下运行以下命令以准备默认实例：  
  
```  
setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=MSSQLSERVER /Features=AS,SQL /INDICATEPROGRESS /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
 在命令提示符下运行以下命令以准备命名实例：  
  
```  
setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName="<Insert Instance name>" /Features=AS,SQL /INDICATEPROGRESS /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
#### <a name="complete-failover-cluster-parameters"></a>完成故障转移群集参数  
 使用下表中的参数可开发用于执行故障转移群集完成操作的命令行脚本。 这是高级故障转移群集安装选项的第二步。 在所有的故障转移群集节点上运行了 prepare 后，在拥有共享磁盘的节点上运行此命令。 有关详细信息，请参阅 [AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示故障转移群集完成工作流。<br /><br /> 支持的值：CompleteFailoverCluster|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERGROUP<br /><br /> **可选**|指定要用于 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集的资源组的名称。 可以是现有群集组的名称，也可以是新资源组的名称。<br /><br /> 默认值：<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (\<InstanceName>)|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅[实例配置](../../sql-server/install/instance-configuration.md)  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE <br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 1=启用<br /><br /> 0=禁用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERDISKS<br /><br /> **可选**|指定要包含在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集资源组中的共享磁盘的列表。<br /><br /> 默认值：<br /><br /> 第一个驱动器用作所有数据库的默认驱动器。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERIPADDRESSES<br /><br /> **必需**|指定编码的 IP 地址。 编码以分号 (;) 分隔并遵循格式 \<IP Type>;\<address>;\<network name>;\<subnet mask>。 支持的 IP 类型包括 DHCP、IPv4 和 IPv6。<br />可以指定多个故障转移群集 IP 地址，地址之间用空格分隔。 请看以下示例：<br /><br /> `FAILOVERCLUSTERIPADDRESSES=DEFAULT`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;172.16.0.0;ClusterNetwork1;172.31.255.255`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;2001:db8:23:1002:20f:1fff:feff:b3a3;ClusterNetwork1`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERNETWORKNAME<br /><br /> **必需**|指定新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集的网络名称。 此名称用于在网络中标识新的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集实例。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIRMIPDEPENDENCYCHANGE|指示对于多子网故障转移群集，同意将 IP 地址资源依赖关系设置为 OR。 有关详细信息，请参阅[创建新的 SQL Server 故障转移群集（安装程序）](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)。 支持的值：<br /><br /> 0 = False（默认值）<br /><br /> 1 = True|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASBACKUPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 备份文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Backup`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的排序规则设置。<br /><br /> 默认值：Latin1_General_CI_AS|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASCONFIGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Config`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASDATADIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Data`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASLOGDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 日志文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Log`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSERVERMODE<br /><br /> **可选**|指定 Analysis Services 实例的服务器模式。 群集方案中的有效值为 MULTIDIMENSIONAL 或 TABULAR。 **ASSERVERMODE** 区分大小写。 所有值必须以大写形式表示。 有关有效值的详细信息，请参阅“在表格模式下安装 Analysis Services”。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSYSADMINACCOUNTS<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的管理员凭据。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASTEMPDIR<br /><br /> **可选**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 临时文件的目录。 默认值：<br /><br /> 对于 64 位上的 WOW 模式：`%Program Files(x86)%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`<br /><br /> 对于所有其他安装：`%Program Files%\Microsoft SQL Server\<INSTANCEDIR>\<ASInstanceID>\OLAP\Temp`|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASPROVIDERMSOLAP<br /><br /> **可选**|指定 MSOLAP 提供程序是否可以在进程中运行。<br /><br /> 默认值：1=启用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/INSTALLSQLDATADIR<br /><br /> **必需**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件的数据目录。<br /><br /> 数据目录必须指定且必须位于共享群集磁盘上。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SAPWD<br /><br /> **/SECURITYMODE=SQL 时是必需的**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SA 帐户的密码。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SECURITYMODE<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安全模式。<br /><br /> 如果未提供此参数，则支持仅 Windows 身份验证模式<br /><br /> 支持的值：**SQL**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLBACKUPDIR<br /><br /> **可选**|指定备份文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Backup`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLCOLLATION<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的排序规则设置。<br /><br /> 默认值基于您的 Windows 操作系统的区域设置。 有关详细信息，请参阅 [Collation Settings in Setup](../../relational-databases/collations/collation-and-unicode-support.md)（安装程序中的排序规则设置）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSYSADMINACCOUNTS<br /><br /> **必需**|使用此参数可将登录帐户设置为 sysadmin 角色的成员。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBDIR<br /><br /> **可选**|指定用户数据库的数据文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLUSERDBLOGDIR<br /><br /> **可选**|指定用户数据库的日志文件的目录。<br /><br /> 默认值：30`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **在“仅文件”模式下可用。**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBDIR<br /><br /> **可选**|指定 tempdb 数据文件的目录。 指定多个目录时，请用空格将目录隔开。 如果指定了多个目录，则 tempdb 数据文件将以轮循机制的方式分布在目录中。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGDIR<br /><br /> **可选**|指定 tempdb 日志文件的目录。<br /><br /> 默认值：`<InstallSQLDataDir>\<SQLInstanceID>\MSSQL\Data`（系统数据目录）<br /><br /> 注意：此参数也被添加到了 RebuildDatabase 方案。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILECOUNT<br /><br /> **可选**|指定要由安装程序添加的 tempdb 数据文件的数量。 此值可以增加至内核的数量。 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 1<br /><br /> 8 或内核的数量，无论哪个对所有其他版本来说较低。<br /><br /> **重要提示：** tempdb 的主数据库文件依然为 tempdb.mdf。 将其他 tempdb 文件命名为 tempdb_mssql_#.ndf，其中 # 代表在安装期间创建的每个其他 tempdb 数据文件的唯一编号。 此命名约定的目的是使它们具有唯一性。 卸载 SQL Server 的实例将删除具有命名约定 tempdb_mssql_ #.ndf 的文件。 不要对用户数据库文件使用 tempdb_mssql_\*.ndf 命名约定。<br /><br /> 警告：[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 不支持配置此参数。 安装程序仅安装 1 个 tempdb 数据文件。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILESIZE<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 数据文件的初始大小。<br/><br/>默认值 = 8 MB。<br/><br/>最小值 = 8 MB。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262144 MB）。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBFILEGROWTH<br /><br /> **可选**|指定每个 tempdb 数据文件的文件增长增量 (MB)。 值为 0 时表明自动增长被设置为关闭，不允许增加空间。 安装程序允许的大小最大为 1024。<br /><br /> 默认值：64<br /><br /> 允许的范围：最小值 = 0，最大值 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILESIZE<br /><br /> **可选**|指定 tempdb 日志文件的初始大小 (MB)。 安装程序允许的大小最大为 1024。 <br /> 默认值：<br /><br /> 对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]为 4<br /><br /> 所有其他版本为 8<br /><br /> 允许的范围：最小 = 默认值（4 或 8），最大 = 1024|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLTEMPDBLOGFILEGROWTH<br /><br /> **可选**|已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中引入。 指定每个 tempdb 日志文件的初始大小。<br/><br/>[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认值为 4 MB，所有其他版本的默认值为 8 MB。<br/><br/>最小值 =（4 MB 或 8 MB）。<br/><br/>最大值 = 1024 MB（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的最大值为 262,144 MB）|  
  
###### <a name="sample-syntax"></a>示例语法：  
 针对 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]执行故障转移群集高级安装方案的“完成”步骤。 在将成为故障转移群集活动节点的计算机上运行以下命令，以使其可用。 必须在 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 故障转移群集中拥有共享磁盘的节点上运行“CompleteFailoverCluster”操作。 
  
 在命令提示符下运行以下命令以完成默认实例的故障转移群集安装：  
  
```  
setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=MSSQLSERVER /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\Username>" /ASDATADIR=<Drive>:\OLAP\Data /ASLOGDIR=<Drive>:\OLAP\Log /ASBACKUPDIR=<Drive>:\OLAP\Backup /ASCONFIGDIR=<Drive>:\OLAP\Config /ASTEMPDIR=<Drive>:\OLAP\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
```  
  
 在命令提示符下运行以下命令以完成命名实例的故障转移群集安装：  
  
```  
setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName="<Insert Instance Name>" /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\UserName>" /ASDATADIR=<Drive>:\KATMAI\Data /ASLOGDIR=<drive>:\KATMAI\Log /ASBACKUPDIR=<Drive>:\KATMAI\Backup /ASCONFIGDIR=<Drive>:\KATMAI\Config /ASTEMPDIR=<Drive>:\KATMAI\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>" /FAILOVERCLUSTERNETWORKNAME="CompNamedFOI" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;ClusterNetwork1;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="<Insert New Group Name>" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER_KATMAI" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\Username>"  
```  
  
#### <a name="upgrade-failover-cluster-parameters"></a>升级故障转移群集参数  
 使用下表中的参数可开发用于故障转移群集升级的命令行脚本。 有关详细信息，请参阅[升级 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集实例（安装程序）](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance-setup.md)和 [Always On 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示安装工作流。<br /><br /> 支持的值：**升级**|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 SQL Server。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateEnabled*<br /><br /> **可选**|指定 SQL Server 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateSource*<br /><br /> **可选**|指定 SQL Server 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 `.\MyUpdates` 或一个 UNC 共享）。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将通过 Windows Server Update Services 搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update 或 Windows Update 服务。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ERRORREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 <br/><br/>要管理如何将错误反馈发送到 Microsoft，请参阅[如何配置 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)。 <br/><br/>在旧版本中，它指定 SQL Server 的错误报告。<br /><br /> 有关详细信息，请参阅 [Privacy Statement for the Microsoft Error Reporting Service](../../sql-server/sql-server-privacy.md)（Microsoft 错误报告服务的隐私声明）。 支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ INSTANCEDIR<br /><br /> **可选**|为共享组件指定一个非默认安装目录。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCEID<br /><br /> **从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本升级时是必需的。**<br /><br /> **从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级时为可选项**|为 [InstanceID](#InstanceID)指定一个非默认值。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/SQMREPORTING<br /><br /> **可选**|在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中无效。 在旧版本中，它指定 SQL Server 的功能使用情况报告。<br /><br />支持的值：<br /><br /> 0=禁用<br /><br /> 1=启用|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERROLLOWNERSHIP|指定升级过程中的 [故障转移行为](#RollOwnership) 。|  
|SQL Server Browser 服务|/BROWSERSVCSTARTUPTYPE<br /><br /> **可选**|指定 SQL Server Browser 服务的 [启动](#Accounts) 模式。 支持的值：<br /><br /> **自动**<br /><br /> **已禁用**<br /><br /> **手动**|  
|SQL Server 全文|/FTUPGRADEOPTION<br /><br /> **可选**|指定全文目录升级选项。 支持的值：<br /><br /> **REBUILD**<br /><br /> **RESET**<br /><br /> **IMPORT**|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的帐户。<br /><br /> 默认值：NT AUTHORITY\NETWORK SERVICE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCStartupType<br /><br /> **可选**|指定 [服务的](#Accounts) 启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSUPGRADEDATABASEACCOUNT<br /><br /> **可选**|仅当升级版本为 2008 R2 或更低版本的 SharePoint 模式报表服务器时才使用此属性。 对于使用较旧 SharePoint 模式体系结构（在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中已更改）的报表服务器，将执行额外的升级操作。 如果命令行安装中未附随此选项，则使用针对旧报表服务器实例的默认服务帐户。 如果使用此属性，则使用 **/RSUPGRADEPASSWORD** 属性提供帐户密码。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSUPGRADEPASSWORD<br /><br /> **可选**|现有 Report Server 服务帐户的密码。|  
  
####  <a name="add-node-parameters"></a><a name="AddNode"></a>添加节点参数  
 使用下表中的参数可开发用于 AddNode 的命令行脚本。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示 AddNode 工作流。<br /><br /> 支持的值：AddNode|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/IACCEPTSQLSERVERLICENSETERMS<br /><br /> **仅在为无人参与安装指定了 /Q 或 /QS 参数时才是必需的。**|必需，用于确认接受许可条款。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ENU<br /><br /> **可选**|当安装介质包括针对英文以及与操作系统相对应的语言的语言包时，使用此参数可以在已本地化的操作系统上安装英文版的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateEnabled*<br /><br /> **可选**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序是否应发现和包含产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下，SQL Server 安装程序将包含找到的更新。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/*UpdateSource*<br /><br /> **可选**|指定 SQL Server 安装程序将获取产品更新的位置。 有效值为可用于搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新的“MU”，这是一个有效的文件夹路径、一个相对路径（例如 `.\MyUpdates` 或一个 UNC 共享）。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将通过 Windows Server Update Services 搜索 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update 或 Windows Update 服务。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|PolyBase|/PBENGSVCACCOUNT<br /><br /> **可选**|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|PolyBase|/PBDMSSVCPASSWORD<br /><br /> **可选**|指定引擎服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|PolyBase|/PBENGSVCSTARTUPTYPE<br /><br /> **可选**|指定 PolyBase 引擎服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|/PBPORTRANGE<br /><br /> **可选**|使用最少 6 个端口为 PolyBase 服务指定端口范围。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  
|PolyBase|/PBSCALEOUT<br /><br /> **可选**|指定是否将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例用作 PolyBase 扩展计算组的一部分。 支持的值：True、False |  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/PID<br /><br /> **可选**|指定 SQL Server 版本的产品密钥。 如果未指定此参数，则使用 Evaluation。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE <br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/FAILOVERCLUSTERIPADDRESSES<br /><br /> **必需**|指定编码的 IP 地址。 编码以分号 (;) 分隔并遵循格式 \<IP Type>;\<address>;\<network name>;\<subnet mask>。 支持的 IP 类型包括 DHCP、IPv4 和 IPv6。<br />可以指定多个故障转移群集 IP 地址，地址之间用空格分隔。 请看以下示例：<br /><br /> `FAILOVERCLUSTERIPADDRESSES=DEFAULT`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv4;172.16.0.0;ClusterNetwork1;172.31.255.255`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;DHCP;ClusterNetwork1`<br /><br /> `FAILOVERCLUSTERIPADDRESSES=IPv6;2001:db8:23:1002:20f:1fff:feff:b3a3;ClusterNetwork1`<br /><br /> <br /><br /> 有关详细信息，请参阅[在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集中添加或删除节点（安装程序）](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIRMIPDEPENDENCYCHANGE<br /><br /> **必需**|指示对于多子网故障转移群集，同意将 IP 地址资源依赖关系设置为 OR。 有关详细信息，请参阅[在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集中添加或删除节点（安装程序）](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。 支持的值：<br /><br /> 0 = False（默认值）<br /><br /> 1 = True|  
|SQL Server 代理|/AGTSVCACCOUNT<br /><br /> **必需**|为 SQL Server 代理服务指定帐户。|  
|SQL Server 代理|/AGTSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQL Server 代理服务帐户的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCACCOUNT<br /><br /> **必需**|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的帐户。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT<br /><br /> **必需**|指定 SQL Server 服务的启动帐户。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 SQLSVCACCOUNT 的密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCPASSWORD<br /><br /> [必需](#Accounts)|指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSINSTALLMODE<br /><br /> **在“仅文件”模式下可用**| 从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的安装模式。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCPASSWORD<br /><br /> [必需](#Accounts)|从 SQL Server 2017 开始不再适用。  指定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的启动帐户密码。 使用托管服务帐户、虚拟帐户或内置帐户时，可以省略此参数。|  
  
##### <a name="additional-notes"></a>其他说明：  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 是唯一识别群集的组件。 其他功能不能识别群集，且不具有故障转移的高可用性。 
  
###### <a name="sample-syntax"></a>示例语法：  
 将节点添加到具有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的现有故障转移群集实例。 
  
```  
setup.exe /q /ACTION=AddNode /INSTANCENAME="<Insert Instance Name>" /SQLSVCACCOUNT="<SQL account that is used on other nodes>" /SQLSVCPASSWORD="<password for SQL account>" /AGTSVCACCOUNT="<SQL Server Agent account that is used on other nodes>", /AGTSVCPASSWORD="<SQL Server Agent account password>" /ASSVCACCOUNT="<AS account that is used on other nodes>" /ASSVCPASSWORD="<password for AS account>" /INDICATEPROGRESS /IACCEPTSQLSERVERLICENSETERMS /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;ClusterNetwork1;xxx.xxx.xxx.x" /CONFIRMIPDEPENDENCYCHANGE=0  
```  
  
#### <a name="remove-node-parameters"></a>删除节点参数  
 使用下表中的参数可开发用于 RemoveNode 的命令行脚本。 若要卸载故障转移群集，必须在每个故障转移群集节点上运行 RemoveNode。 有关详细信息，请参阅 [AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|参数|说明|  
|-----------------------------------------|---------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/ACTION<br /><br /> **必需**|需要它来指示 RemoveNode 工作流。<br /><br /> 支持的值：RemoveNode|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIGURATIONFILE<br /><br /> **可选**|指定要使用的 [ConfigurationFile](./install-sql-server-using-a-configuration-file.md) 。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HELP, ?<br /><br /> **可选**|显示这些参数的用法选项。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INDICATEPROGRESS<br /><br /> **可选**|指定应将详细的安装日志文件传送到控制台。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/INSTANCENAME<br /><br /> **必需**|指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例名称。<br /><br /> 有关详细信息，请参阅 [Instance Configuration](../../sql-server/install/instance-configuration.md)。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/Q 或 /QUIET <br /><br /> **可选**|指定在没有任何用户界面的情况下以静默模式运行安装程序。 这适用于无人参与的安装。 /Q 参数会覆盖 /QS 参数的输入。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/QS 或 /QUIETSIMPLE<br /><br /> **可选**|指定安装程序通过 UI 运行并显示进度，但是不接受任何输入或显示任何错误消息。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/HIDECONSOLE<br /><br /> **可选**|指定控制台窗口隐藏或关闭。|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 安装程序控件|/CONFIRMIPDEPENDENCYCHANGE<br /><br /> **必需**|指示对于多子网故障转移群集，同意将 IP 地址资源依赖关系从 OR 设置为 AND。 有关详细信息，请参阅[在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集中添加或删除节点（安装程序）](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。 支持的值：<br /><br /> 0 = False（默认值）<br /><br /> 1 = True|  
  
###### <a name="sample-syntax"></a>示例语法：  
 从具有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的现有故障转移群集实例中删除节点。 
  
```  
setup.exe /q /ACTION=RemoveNode /INSTANCENAME="<Insert Instance Name>" [/INDICATEPROGRESS] /CONFIRMIPDEPENDENCYCHANGE=0  
```  
  
##  <a name="service-account-parameters"></a><a name="Accounts"></a>服务帐户参数  
 可以使用内置帐户、本地帐户或域帐户配置 SQL Server 服务。 
  
> [!NOTE] 
> 使用托管服务帐户、虚拟帐户或内置帐户时，不应指定相应的密码参数。 有关这些服务帐户的详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)中的 **可用于 [!INCLUDE[win7](../../includes/win7-md.md)] 和 [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] 的新帐户类型** 部分。 
  
 有关服务帐户配置的详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。 
  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 组件|帐户参数|密码参数|启动类型|  
|-----------------------------------------|-----------------------|------------------------|------------------|  
|SQL Server 代理|/AGTSVCACCOUNT|/AGTSVCPASSWORD|/AGTSVCSTARTUPTYPE|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|/ASSVCACCOUNT|/ASSVCPASSWORD|/ASSVCSTARTUPTYPE|  
|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|/SQLSVCACCOUNT|/SQLSVCPASSWORD|/SQLSVCSTARTUPTYPE|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|/ISSVCACCOUNT|/ISSVCPASSWORD|/ISSVCStartupType|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|/RSSVCACCOUNT|/RSSVCPASSWORD|/RSSVCStartupType|  
  
  > [!NOTE]
  > Reporting Services 功能已从 SQL Server 2017 中删除。 SQL Server Reporting Services 的帐户参数仅适用于 SQL Server 2017 之前的版本。 


##  <a name="feature-parameters"></a><a name="Feature"></a> 功能参数  
 若要安装特定功能，请使用 /FEATURES 参数并指定下表中的父功能或功能值： 有关 SQL Server 各版本支持的功能的列表，请参阅 [[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 各个版本及其支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)、[[!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)] 各个版本及其支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)或 [[!INCLUDE[ssSQLv15](../../includes/sssqlv15-md.md)] 各个版本及其支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)。
  
|父功能参数|功能参数|说明|  
|:---|:---|:---|  
|SQL||安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]、复制、全文组件和 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]。|  
||SQLEngine|仅安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]。|  
||复制|将复制组件与 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]一起安装。|  
||FullText|将全文组件与 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]一起安装。|  
||DQ|复制完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装所需的文件。 在完成 SQL Server 安装后，必须运行 DQSInstaller.exe 文件来完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装。 有关详细信息，请参阅 [运行 DQSInstaller.exe 以便完成数据质量服务器安装](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md)。 这也将安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]。|  
||PolyBase |安装 PolyBase 组件。|
||PolyBaseCore | 从 SQL Server 2019 开始，与 **PolyBase** 配对以安装 Polybase 技术，该技术支持使用标准 T-SQL 语句实现跨 Oracle、Teradata、SQL Server 以及其他关系和非关系数据的真正集成查询。 |
|| PolyBaseJava | 从 SQL Server 2019 开始，与 **PolyBase** 配对以安装 PolyBase Java 连接器，该连接器支持使用标准 T-SQL 语句实现对 HDFS 数据的真正集成查询。
||AdvancedAnalytics |安装 [SQL Server 机器学习服务](../../machine-learning/install/sql-machine-learning-services-windows-install.md)或 [SQL Server 2016 R Services（数据库内）](../../machine-learning/install/sql-r-services-windows-install.md)。|  
||SQL_INST_MR |适用于 [SQL Server 机器学习服务](../../machine-learning/install/sql-machine-learning-services-windows-install.md)2017 版或更高版本）和 [SQL Server 2016 R Services（数据库内）](../../machine-learning/install/sql-r-services-windows-install.md)。 与 AdvancedAnalytics 配对以安装 R Open 和专有 R 包。|  
||SQL_INST_MPY|适用于 [SQL Server 机器学习服务](../../machine-learning/install/sql-machine-learning-services-windows-install.md)（2017 版或更高版本）。 与 AdvancedAnalytics 配对以安装 Anaconda 和专有 Python 包。|  
||SQL_INST_JAVA |适用于 [SQL Server Java 语言扩展](../../language-extensions/install/windows-java.md)（2019 版及更高版本）。 与 **AdvancedAnalytics** 配对以安装扩展，这些扩展支持使用标准 T-SQL 语句实现与 Java 的集成。|  
|AS||安装所有的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 组件。|  
|RS||安装所有的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件。 从 SQL Server 2017 开始已删除。 |  
|RS_SHP||安装用于 SharePoint 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件。 从 SQL Server 2017 开始已删除。|  
|RS_SHPWFE||安装用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。 从 SQL Server 2017 开始已删除。 |  
|DQC||安装 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]。|  
|IS||安装所有的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件。|  
||IS_Master|包括适用于 Integration Services Scale Out 的 Scale Out 主要角色。| 
||IS_Worker|包括适用于 Integration Services Scale Out 的 Scale Out 辅助角色。| 
|MDS||安装 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]。|  
|SQL_SHARED_MPY||为 [SQL Server 2017 机器学习服务器（独立版）](../../machine-learning/install/sql-machine-learning-standalone-windows-install.md)安装 Python 包 |  
|SQL_SHARED_MR||为 [SQL Server 2016 R Server（独立版）](../../machine-learning/install/sql-machine-learning-standalone-windows-install.md)或 [SQL Server 机器学习服务器（独立版）](../../machine-learning/install/sql-machine-learning-standalone-windows-install.md)安装 R 包 |  
|工具*||安装客户端工具和 SQL Server 联机丛书组件。|  
||BC|安装向后兼容组件。|  
||Conn|安装连接组件。|
||DREPLAY_CTLR|安装 Distributed Replay 控制器|  
||DREPLAY_CLT|安装 Distributed Replay 客户端|  
||SNAC_SDK|安装用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server Native Client 的 SDK|  
||SDK 中 IsInRole 中的声明|安装软件开发工具包。|  
||LocalDB**|安装 LocalDB，它是面向程序开发人员的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 执行模式。|  

*[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 现在是独立于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 安装程序的独立安装程序。 有关详细信息，请参阅[安装 SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。

### <a name="feature-parameter-examples"></a>功能参数示例：
  
|参数和值|说明| 
|---------------|-----------------|  
|/FEATURES=SQLEngine|安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，不带复制和全文组件。|  
|/FEATURES=SQLEngine,FullText|安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和全文组件。|  
|/FEATURES=SQL,Tools|安装完整的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和所有工具。|  
|/FEATURES=BOL|安装 SQL Server 联机丛书组件以便查看和管理帮助内容。|  
|/FEATURES=SQLEngine,PolyBase|安装 PolyBase 引擎。|  
  
##  <a name="role-parameters"></a><a name="RoleParameters"></a> 角色参数

安装角色或 /Role 参数用于安装预配置的所选功能。 SSAS 角色在现有 SharePoint 场或未配置的新场中安装 SSAS 实例。 对于每种方案，分别提供了两个安装角色来支持它们。 一次只能选择一个安全角色来进行安装。 如果您选择了安装角色，安装程序将安装属于此角色的功能和组件。 您不能改变为该角色指定的功能和组件。 有关如何使用功能角色参数的详细信息，请参阅 [从命令提示符安装 Power Pivot](/analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode)。

AllFeatures_WithDefaults 角色是各版本 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的默认行为，可减少向用户提供的对话框数量。 当安装的 SQL Server 版本不是 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]时，可以从命令行指定此角色。

|角色|说明|安装…|  
|----------|-----------------|---------------|  
|SPI_AS_ExistingFarm|将 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 作为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 命名实例安装在现有 [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] 场或独立服务器上。|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 计算引擎，为内存中数据存储和处理而预先配置的。<br /><br /> [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案包<br /><br /> [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)]<br /><br /> SQL Server 联机丛书|  
|SPI_AS_NewFarm|将 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 作为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 命名实例安装在新的、未配置的 Office [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] 场或独立服务器上。 SQL Server 安装程序将在功能角色安装过程中配置场。|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 计算引擎，为内存中数据存储和处理而预先配置的。<br /><br /> [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案包<br /><br /> SQL Server 联机丛书<br /><br /> [!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> 配置工具<br /><br /> [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]|  
|AllFeatures_WithDefaults|安装当前版本中提供的所有功能。<br /><br /> 将当前用户添加到 SQL Server **sysadmin** 固定服务器角色。<br /><br /> 在 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或更高版本中，当操作系统不是域控制器时， [!INCLUDE[ssDE](../../includes/ssde-md.md)]和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 默认为使用 NTAUTHORITY\NETWORK SERVICE 帐户，而 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 默认为使用 NTAUTHORITY\NETWORK SERVICE 帐户。<br /><br /> 默认情况下在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]的版本中启用此角色。 对于所有其他版本，不启用此角色，但可以通过 UI 或使用命令行参数指定此角色。|对于 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]的各版本，只安装相应版本中提供的那些功能。 对于其他版本，安装所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。<br /><br /> **AllFeatures_WithDefaults** 参数可以与其他替代 **AllFeatures_WithDefaults** 参数设置的参数结合使用。 例如，使用 **AllFeatures_WithDefaults** 参数和 **/Features=RS** 参数会覆盖用于安装所有功能的命令，而只安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，但建议选择 **AllFeatures_WithDefaults** 参数以便将默认服务帐户用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。<br /><br /> 当将 **AllFeatures_WithDefaults** 参数与 **/ADDCURRENTUSERASSQLADMIN=FALSE** 结合使用时，当前用户不会自动填充设置对话框。 添加 **/AGTSVCACCOUNT** 和 **/AGTSVCPASSWORD** ，以便为 SQL Server 代理指定服务帐户和密码。|

## <a name="controlling-failover-behavior-using-the-failoverclusterrollownership-parameter"></a><a name="RollOwnership"></a> 使用 /FAILOVERCLUSTERROLLOWNERSHIP 参数控制故障转移行为  
若要将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 故障转移群集升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]，则必须从被动节点开始逐个在每个故障转移群集节点上运行安装程序。 安装程序根据故障转移群集实例中的节点总数以及已经升级的节点数来确定何时故障转移到已升级的节点。 如果有一半或更多节点已经升级，则默认情况下，安装程序将导致故障转移到已升级的节点。

若要控制升级过程中群集节点的故障转移行为，请从命令提示符运行升级操作，并使用 /FAILOVERCLUSTERROLLOWNERSHIP 参数控制升级操作使节点脱机之前的故障转移行为。 此参数的用法如下所示：

- /FAILOVERCLUSTERROLLOWNERSHIP=0 不会将群集所有权（移动组）滚动到已升级的节点，并且在升级结束时不会将此节点添加到 SQL Server 群集的可能所有者列表中。 

- /FAILOVERCLUSTERROLLOWNERSHIP=1 会将群集所有权（移动组）滚动到已升级的节点，并且在升级结束时会将此节点添加到 SQL Server 群集的可能所有者列表中。 

- /FAILOVERCLUSTERROLLOWNERSHIP=2 是默认设置。 如果未指定此参数，将使用该默认设置。 此设置指示 SQL Server 安装程序将根据需要管理群集所有权（移动组）。 

##  <a name="instance-id-or-instanceid-configuration"></a><a name="InstanceID"></a> 实例 ID 或 InstanceID 配置

实例 ID 或 /InstanceID 参数用于指定实例组件的安装位置以及实例的注册表路径。 INSTANCEID 的值为字符串且必须唯一。 

- SQL 实例 ID：`MSSQLxx.<INSTANCEID>`

- AS 实例 ID：`MSASxx.<INSTANCEID>`

- RS 实例 ID：`MSRSxx.<INSTANCEID>`

识别实例的组件安装在以下位置：  

`%Program Files%\Microsoft SQL Server\<SQLInstanceID>`

`%Program Files%\Microsoft SQL Server\<ASInstanceID>`

`%Program Files%\Microsoft SQL Server\<RSInstanceID>`

> [!NOTE]
> 如果在命令行中未指定 INSTANCEID，则默认情况下安装程序用 \<INSTANCENAME> 替代 \<INSTANCEID>。 

## <a name="see-also"></a>另请参阅
- [使用安装向导安装 SQL Server 2016](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
- [SQL Server 故障转移群集安装](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)
- [安装 SQL Server 2016 商业智能功能](../../sql-server/install/install-sql-server-business-intelligence-features.md)
