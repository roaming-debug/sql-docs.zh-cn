---
description: 执行包实用工具 (dtexecui)
title: 执行包实用工具 (dtexecui) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtexecui.setvalues.f1
- sql13.dts.dtexecui.reporting.f1
- sql13.dts.dtexecui.datasources.f1
- sql13.dts.dtexecui.commandfiles.f1
- sql13.dts.dtexecui.logging.f1
- sql13.dts.dtexecui.general.f1
- sql13.dts.dtexecui.verification.f1
- sql13.dts.dtexecui.executionoptions.f1
- sql13.dts.dtexecui.commandline.f1
- sql13.dts.dtexecui.configuration.f1
helpviewer_keywords:
- DTExecUI utility
ms.assetid: 3d71df39-126b-4c8e-bd77-128bbd5b0887
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14392191ab3d11165f75f1637c2fb3330aa886ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346914"
---
# <a name="execute-package-utility-dtexecui"></a>执行包实用工具 (dtexecui)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“执行包实用工具”** 来运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。 该实用工具运行存储在以下三个位置之一的包：[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库、[!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区和文件系统。 此用户界面是使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] DTExec **命令提示工具运行包的替代方法，可从** 打开，或者通过在命令提示符下键入 **dtexecui** 打开。  
  
 包与 **dtexecui.exe** 实用工具在同一个进程中执行。 由于此实用工具为 32 位工具，因此，在 64 位环境中使用 **dtexecui.exe** 运行的包是在 Windows on Win32 (WOW) 中运行的。 当在 64 位计算机上使用 dtexecui.exe 实用工具开发和测试命令时，应该首先在 64 位模式下使用 64 位版本的 **dtexec.exe** 测试该命令，然后在生产服务器中部署或安排这些命令。  
  
 “执行包实用工具”  是用于 **DTExec** 命令提示工具的图形用户界面。 通过指定的选项运行包时，此用户界面可使配置选项的工作变得更轻松，并会自动汇集传递到 **DTExec** 命令提示工具的命令行。  
  
 “执行包实用工具”  还可用于汇集在直接运行 **DTExec** 时所用的命令行。  
  
### <a name="to-open-execute-package-utility-in-sql-server-management-studio"></a>打开 SQL Server Management Studio 中的执行包实用工具  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的 **“视图”** 菜单中，单击 **“对象资源管理器”**。  
  
2.  在对象资源管理器中，单击 **“连接”**，然后单击 **“Integration Services”**。  
  
3.  在 **“连接到服务器”** 对话框中的 **“服务器名称”** 列表中输入服务器名称，然后单击 **“连接”**。  
  
4.  展开“已存储的包”文件夹和子文件夹，右键单击要运行的包，然后单击“运行包”。  
  
### <a name="to-open-the-execute-package-utility-at-the-command-prompt"></a>在命令提示符下打开执行包实用工具  
  
-   在命令提示符窗口中，运行 **dtexecui**。  
  
 以下各节描述了 **“执行包实用工具”** 对话框的各页。  
  
## <a name="general-page"></a>“常规”页  
 使用 **“执行包实用工具”** 对话框的 **“常规”** 页，可以指定包的名称和位置。  
  
 执行包实用工具 (dtexecui.exe) 始终在本地计算机上运行包，即使包保存在远程服务器上也是如此。 如果远程包使用同样保存在远程服务器上的配置文件，那么执行包实用工具可能找不到配置，包将失败。 若要避免此问题，必须使用通用命名约定 (UNC) 共享名称（如 \\\myserver\myfile）引用配置。  
  
### <a name="static-options"></a>静态选项  
 **包源**  
 使用以下选项指定要运行的包的位置：  
  
|值|说明|  
|-|-|  
|**SQL Server**|当包驻留在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时选择此选项。 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证提供用户名和密码。 每个用户名和密码会将 /USER username 和 /PASSWORD password 选项添加到命令提示符  。|  
|**文件系统**|当包驻留在文件系统时选择此选项。|  
|**SSIS 包存储区**|当包驻留在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区时选择此选项。|  
  
 上述选择的每一项都包括以下一组选项：  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
### <a name="dynamic-options"></a>动态选项  
  
#### <a name="package-source--sql-server"></a>包源 = SQL Server  
 **Server**  
 输入包驻留的服务器的名称，或者从列表中选择服务器。  
  
 **登录到服务器**  
 指定包使用 Windows 身份验证还是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 为了实现更好的安全性，建议使用 Windows 身份验证。 使用 Windows 身份验证时无需指定用户名和密码。  
  
 **Use Windows Authentication**  
 选择此选项，可以使用 Windows 身份验证，并使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户帐户登录。  
  
 **Use SQL Server Authentication**  
 选择此选项，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 当用户使用指定的登录名和密码从不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将通过检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码匹配，来进行身份验证。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 找不到登录帐户，则身份验证会失败，用户将收到错误消息。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。  
  
 **包**  
 键入包的名称或者单击省略号按钮 (…)，使用“选择 SSIS 包”对话框定位包。  
  
#### <a name="package-source--file-system"></a>包源 = 文件系统  
 **包**  
 键入包的名称或者单击省略号按钮 (…) ，使用“打开”对话框定位包。 默认情况下，该对话框仅列出扩展名为 .dtsx 的文件。  
  
#### <a name="package-source--ssis-package-store"></a>包源 = SSIS 包存储区  
 **Server**  
 输入包驻留的计算机的名称，或者从列表中选择计算机。  
  
 **登录到服务器**  
 指定包是否使用 Microsoft Windows 身份验证连接到包源。 为了实现更好的安全性，建议使用 Windows 身份验证。 使用 Windows 身份验证时无需指定用户名和密码。  
  
 **Use Windows Authentication**  
 选择此选项可以使用 Windows 身份验证，并使用 Microsoft Windows 用户帐户登录。  
  
 **Use SQL Server Authentication**  
 在运行存储于“SSIS 包存储区”的包时，此选项不可用。  
  
 **包**  
 键入包的名称或者单击省略号按钮 (…)，使用“选择 SSIS 包”对话框定位包。  
  
## <a name="configurations-page"></a>配置页  
 可以使用 **“执行包实用工具”** 对话框的 **“配置”** 页，选择在运行时加载的配置文件并指定它们的加载顺序。  
  
### <a name="options"></a>选项  
 **配置文件**  
 列出包使用的配置。 每个配置文件都会向命令提示符中添加 **/CONFIGFILE filename** 选项。  
  
 **箭头键**  
 在列表中选择配置文件，然后使用右侧的箭头键更改加载顺序。 将从列表顶部开始按顺序加载配置。  
  
> [!NOTE]  
>  如果多个配置修改了同一个属性，则使用最后加载的配置。  
  
 **添加**  
 单击此项可以使用“打开”对话框添加配置。 默认情况下，该对话框只列出具有 .dtsconfig 扩展名的文件。  
  
 **删除**  
 在列表中选择配置文件，再单击“删除”。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="command-files-page"></a>“命令文件”页  
 可以使用 **“执行包实用工具”** 对话框的 **“命令文件”** 页选择在运行时加载的命令文件。  
  
### <a name="options"></a>选项  
 **命令文件**  
 列出包使用的命令文件。 一个包可以使用多个文件来设置命令行选项。  
  
 **箭头键**  
 在列表中选择命令文件，然后使用右侧的箭头键更改加载顺序。 将从列表顶部开始按顺序加载命令文件。  
  
 **添加**  
 单击此项可以使用“打开”对话框添加命令文件。  
  
 **删除**  
 在文本框中选择命令文件，然后使用“删除”按钮删除该文件。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="connection-managers-page"></a>“连接管理器”页  
 可以使用 **“执行包实用工具”** 对话框的 **“连接管理器”** 页，编辑包使用的连接管理器的连接字符串。  
  
### <a name="options"></a>选项  
 **连接管理器**  
 选中其复选框后，“连接字符串”列即会变为可编辑状态。  
  
 **说明**  
 查看每个连接管理器的说明。 无法编辑说明。  
  
 **连接字符串**  
 编辑连接管理器的连接字符串。 只有选中 **“连接管理器”** 复选框时，此字段才是可编辑的。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="execution-options-page"></a>“执行选项”页  
 可以使用“执行包实用工具”对话框的“执行选项”页指定包的运行时选项。  
  
### <a name="options"></a>选项  
 **发生验证警告时包失败**  
 指示如果发生验证警告包是否失败。  
  
 **验证但不执行包**  
 指示是否只验证包。  
  
 **最大并发可执行文件数**  
 指示是否要指定包中可以同时运行的可执行文件的最大数量。 选中此复选框后，可以使用数字调整框指定可执行文件的最大数量。  
  
 **启用包检查点**  
 指示是否启用包检查点。  
  
 **检查点文件**  
 如果启用包检查点，则列出包所使用的检查点文件。  
  
 **“浏览”**  
 如果启用了包检查点，则单击浏览按钮 (…) 可以通过“打开”对话框查找检查点文件。 如果已经指定了检查点文件，将用所选文件替换该文件。  
  
 **覆盖重新启动选项**  
 指示是否覆盖重新启动选项（如果启用了包检查点）。  
  
 **重新启动选项**  
 选择如何使用检查点（如果覆盖了重新启动选项）。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="reporting-page"></a>“报告”页  
 可以使用 **“执行包实用工具”** 对话框的 **“报告”** 页指定与包有关的事件和信息，以便在包运行时记录到控制台。  
  
### <a name="options"></a>选项  
 **控制台事件**  
 指示要报告的事件和消息类型。  
  
 **无**  
 选择此选项将不进行报告。  
  
 **错误**  
 选择此选项将报告错误消息。  
  
 **警告**  
 选择此选项将报告警告消息。  
  
 **自定义事件**  
 选择此选项将报告自定义事件消息。  
  
 **管道事件**  
 选择此选项将报告数据流事件消息。  
  
 **信息**  
 选择此选项将报告信息性消息。  
  
 **Verbose**  
 选择此选项将使用详细报告。  
  
 **控制台日志记录**  
 指定在发生所选事件时要写入日志的信息。  
  
 **名称**  
 选择此选项将报告创建包的人员的姓名。  
  
 **计算机**  
 选择此选项将报告运行包的计算机的名称。  
  
 **“运算符”**  
 选择此选项将报告启动包的人员的姓名。  
  
 **源名称**  
 选择此选项将报告包名称。  
  
 **源 GUID**  
 选择此选项将报告包 GUID。  
  
 **执行 GUID**  
 选择此选项将报告包执行实例的 GUID。  
  
 **消息**  
 选择此选项将报告消息。  
  
 **开始时间和结束时间**  
 选择此选项将报告包开始和完成的时间。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="logging-page"></a>“日志记录”页  
 可以使用 **“执行包实用工具”** 对话框的 **“日志记录”** 页，将包设置为可在运行时使用日志提供程序。 提供包日志提供程序类型和连接到日志的连接字符串。 对于每个日志提供程序项，在命令提示符下都会添加一个 **/LOGGER**_classid_ 选项。  
  
### <a name="options"></a>选项  
 **日志提供程序**  
 从该列表中选择日志提供程序。  
  
 **配置字符串**  
 从指向日志位置的包中选择连接管理器的名称，或键入连接到日志提供程序的连接字符串。  
  
 **删除**  
 选择一个日志提供程序，再单击此项将删除该日志提供程序。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="set-values-page"></a>“设置值”页  
 可以使用 **“执行包实用工具”** 对话框的 **“设置值”** 页，通过键入属性路径和属性值来设置包、可执行文件、连接、变量和日志提供程序的属性值。 对于每个路径项，在命令提示符下都会添加一个 **/SET**_propertypath;value_ 选项。  
  
### <a name="options"></a>选项  
 **属性路径**  
 键入属性的路径。 在路径语法中，反斜杠 (\\) 用于指示其后面为容器项，句点 (.) 用于指示其后面为属性项，而括号用于指示集合成员。 成员可以通过其索引或其名称进行标识。 例如，包变量的属性路径可以是 \Package.Variables[MyVariable].Value。  
  
 **值**  
 键入属性的值。  
  
 **删除**  
 在选择属性路径后单击此项将删除相应的属性路径。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="verification-page"></a>“验证”页  
 可以使用 **“执行包”** 对话框的 **“验证”** 页设置对包进行验证的条件。  
  
### <a name="options"></a>选项  
 **仅执行已签名的包**  
 选择此项将仅执行已签名的包。  
  
 **验证包内部版本**  
 选择此项可以验证包内部版本。  
  
 生成  
 指定与内部版本相关联的内部版本序号。  
  
 **验证包 ID**  
 选择此项可以验证包 ID。  
  
 包 ID  
 指定包标识号。  
  
 **验证版本 ID**  
 选择此项可以验证版本 ID。  
  
 版本 ID  
 指定版本标识号。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="command-line-page"></a>“命令行”页  
 可以使用 **“执行包实用工具”** 对话框的 **“命令行”** 节点，编辑由不同对话框创建的选项生成的命令行。  
  
### <a name="options"></a>选项  
 **还原原始选项**  
 单击此项可将命令行还原为其原始状态。 如果你使用“手动编辑命令行”选项进行了修改，然后要还原原始命令行选项，则可以使用此选项。  
  
 **手动编辑命令行**  
 单击此项可在“命令行”文本框中编辑命令行。  
  
 **命令行**  
 显示当前的命令行。 如果您选择了手动编辑命令行的选项，则可编辑该命令行。  
  
 **执行**  
 单击此项可运行包。  
  
 **关闭**  
 单击此项可关闭“执行包实用工具”对话框。  
  
## <a name="see-also"></a>另请参阅  
 [dtexec 实用工具](../../integration-services/packages/dtexec-utility.md)  
  
  
