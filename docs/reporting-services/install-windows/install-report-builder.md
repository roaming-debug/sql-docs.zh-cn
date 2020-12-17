---
title: 安装报表生成器 | Microsoft Docs
description: 报表生成器是一个独立应用，由你或管理员安装到你的电脑上。
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 6b2291bb-1d20-4d08-81cb-a16dd8e01faf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ca0071416491700254047b43056d32fc64ed1d9d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425121"
---
# <a name="install-report-builder"></a>安装报表生成器

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 是一个独立应用，由你或管理员安装到你的电脑上。 可以从 Microsoft 下载中心、 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 报表服务器或者与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]集成的 SharePoint 站点进行安装。  

> [!NOTE]
> 想要查找 Power BI 报表生成器的安装信息？ 转到“下载中心”上的 [Microsoft Power BI 报表生成器](https://www.microsoft.com/download/details.aspx?id=58158)页面。 

 通常，由管理员安装和配置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]、授予从 Web 门户下载 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的权限，以及管理文件夹与对报表、报表部件和保存到报表服务器的共享数据集的权限。 有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 管理的详细信息，请参阅 [Reporting Services 报表服务器（本机模式）](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)。  
  
## <a name="install-ssrbnoversion-from--a--web-portal-or-sharepoint-library"></a>从 Web 门户或 SharePoint 库安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
 可以从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户或者与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成的 SharePoint 站点启动 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]。 有关信息，请参阅 [启动报表生成器](../../reporting-services/report-builder/start-report-builder.md)。  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-site-integrated-with-ssrsnoversion"></a>与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
  
 在与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]集成的 SharePoint 站点上，如果“新建文档”  菜单未列出“报表生成器报表” 、“报表生成器模型” 和“报表数据源” ，则需要将其内容类型添加到 SharePoint 库中。 有关详细信息，请参阅 [向 SharePoint 库添加 Reporting Services 内容类型](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md)。  

::: moniker-end
 
## <a name="install-ssrbnoversion-with-microsoft-endpoint-configuration-manager"></a>使用 Microsoft Endpoint Configuration Manager 安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 
  
 管理员还可以使用 Microsoft Endpoint Configuration Manager 等软件将程序推送到你的计算机上。 若要了解如何使用特定软件安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]，请查阅该软件的文档。 有关详细信息，请参阅 [Microsoft Endpoint Configuration Manager 文档](/configmgr/)。  
  
> [!IMPORTANT]  
>  Windows Vista 和 Windows 7 安全功能需要提升权限以运行命令行操作，并将提示您提供运行命令行的权限。 安装不是静默的。 若要进行静默安装，您需要以管理员身份运行命令行。  
  
## <a name="system-requirements"></a>系统要求
  
 请参阅 Microsoft 下载中心上 [报表生成器下载页](https://go.microsoft.com/fwlink/?LinkID=734968) 的 **“系统要求”** 部分。
  
##  <a name="to-install-ssrbnoversion-from-the-download-site"></a><a name="download"></a> 从下载站点安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]  
  
1.  在 [Microsoft 下载中心的报表生成器页上](https://go.microsoft.com/fwlink/?LinkID=734968) ，单击“下载” 。  
  
2.  [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 下载完成后，单击“运行”。  
  
     此操作将启动 SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 向导。  
  
3.  接受许可协议中的条款，然后单击“下一步” 。  
  
4.  在 **“默认的目标服务器”** 页上，如果目标报表服务器的 URL 与默认 URL 不同，则可选择提供前者。 单击“下一步”。   
  
    > [!NOTE]  
    >  如果计划在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 连接到某个报表服务器时使用它，则此时提供该报表服务器的 URL 将会非常方便。 从 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 中的“选项”对话框也可以执行相同操作。  
  
5.  单击“安装”以完成 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的安装。  
  
## <a name="to-install-ssrbnoversion-from-a-share"></a>从共享安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]  
  
1.  请与管理员联系，获得在本地计算机上安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 所要运行的 ReportBuilder.msi 的位置。  
  
2.  浏览找到 ReportBuilder.msi（[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的 Windows Installer 程序包 (MSI)），然后单击它。  
  
     此操作将启动 SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 向导。  
  
3.  完成 [从下载站点安装报表生成器](#download)中的剩余步骤。  
  
## <a name="to-install-ssrbnoversion-from-the-command-line"></a>从命令行安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 

 还可以执行 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的命令行安装，并提供参数以自定义安装。 除了标准 MSI 内部参数以外，还可以使用 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 提供的自定义参数：RBINSTALLDIR 和 RBSERVERURL。 RBINSTALLDIR 指定 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的根安装文件夹。 RBSERVERURL 指定 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 用于在服务器上保存报表的默认报表服务器。  
  
 如果要进行根本没有用户界面交互的完全静默安装，请指定 **/quiet** 选项。 根据设计，quiet 选项标志会隐藏安装错误。 因此，建议在使用 quiet 选项时包括 **/l** 选项，该选项指定进行日志记录。   
  
1.  在 [Microsoft 下载中心的“报表生成器”页](https://go.microsoft.com/fwlink/?LinkID=734968)上，单击“下载”。  
  
2.  [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 下载完成后，单击“保存”。  
  
3.  在 **“开始”** 菜单上，单击 **“运行”** 。  
  
4.  在“打开”  框中，键入 **cmd.**  
  
5.  在命令提示符窗口中，导航到保存 ReportBuilder.msi 的文件夹。  
  
6.  使用以下格式键入命令：  
  
     `msiexec/i ReportBuilder.msi OPTION=OptionValue [OPTION=OptionValue]`  
  
     专门用来安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 的两个选项是：RBINSTALLDIR 和 RBSERVERURL。 不需要在命令行中包含这些参数。 以下为基准命令：  
  
     `msiexec /i ReportBuilder3_x86.msi /quiet`  
  
7.  若要运行命令，请按 ENTER。  
  
## <a name="set-ssrbnoversion-defaults"></a>设置 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 默认值  
  
-   安装 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 后，可以设置一些默认选项。 单击“文件” > “选项” 。  
  
     设置默认 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户或者 SharePoint 站点最有用。 有关详细信息，请参阅 [Set default options for Report Builder](../../reporting-services/report-builder/set-default-options-for-report-builder.md)。  
  
-   单击“报表生成器”  。  
  
     如果未在现有服务器列表中看到报表服务器，请关闭“打开报表”对话框，并单击 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 底部的“连接”，以连接到服务器。  
  
## <a name="see-also"></a>另请参阅  
 [启动报表生成器](../../reporting-services/report-builder/start-report-builder.md)   
 [卸载报表生成器](../../reporting-services/install-windows/uninstall-report-builder.md)  
  
