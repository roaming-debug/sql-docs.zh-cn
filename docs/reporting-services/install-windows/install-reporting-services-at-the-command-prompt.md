---
description: 在命令提示符处安装 Reporting Services 2016 - SSRS
title: 在命令提示符处安装 Reporting Services 2016 - SSRS | Microsoft Docs
ms.date: 01/09/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- command line
ms.assetid: 048169b3-512c-41e4-895a-0416eff41268
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: c7ca2a221b40d636e3dda791bff7ec1d19c402ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425107"
---
# <a name="install-reporting-services-2016-at-the-command-prompt"></a>在命令提示符处安装 Reporting Services 2016

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 支持从 SQL Server 安装程序进行命令行安装。 本主题包含特定于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]的若干命令行安装示例。 有关可用于所有 SQL Server 组件的命令行选项的完整说明，请参阅[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。 本主题不介绍用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序的命令行选项。 有关该外接程序的命令行安装的信息，请参阅 [使用安装文件 rsSharePoint.msi 安装外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md#bkmk_install_rssharepoint)。

##  <a name="native-mode-reporting-services"></a><a name="bkmk_native_mode"></a> 本机模式 Reporting Services

### <a name="rsinstallmode-native-mode"></a>RSINSTALLMODE（本机模式）
 用于安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的主要输入设置是 **/RSINSTALLMODE** 输入设置。 该设置具有两个选项： **DefaultNativeMode** 和 **FilesOnlyMode**  
  
 如果安装包括 SQL Server 数据库引擎，则默认 RSINSTALLMODE 为 DefaultNativeMode。如果安装不包括 SQL Server 数据库引擎，则默认 RSINSTALLMODE 为 FilesOnlyMode。如果选择了 DefaultNativeMode 但安装不包括 SQL Server 数据库引擎，则安装会自动将 RSINSTALLMODE 更改为 FilesOnlyMode。 有关输入设置的详细信息，请参阅 [从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。

### <a name="examples-of-native-mode-installation"></a>本机模式安装示例

 下面的示例将安装以下项并配置其帐户：  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 。  
  
-   [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅功能所需的 SQL Server 代理。  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
```  
Setup.exe /q /IACCEPTSQLSERVERLICENSETERMS /ACTION="install" /ERRORREPORTING=1 /UPDATEENABLED="False" /INSTANCENAME="MSSQLSERVER" /FEATURES="SQLEngine,Adv_SSMS,RS" /RSINSTALLMODE="DefaultNativeMode" /SQLSVCACCOUNT="[DOMAIN\ACCOUNT]" /SQLSVCPASSWORD="[PASSWORD]" /AGTSVCACCOUNT="[DOMAIN\ACCOUNT]" /AGTSVCPASSWORD="[PASSWORD]" /SQLSYSADMINACCOUNTS="[DOMAIN\ACCOUNT]"  
```  
  
##  <a name="sharepoint-mode-reporting-services"></a><a name="bkmk_sharepoint_mode"></a> SharePoint 模式 Reporting Services  
  
### <a name="rsshpinstallmode-sharepoint-mode"></a>RSSHPINSTALLMODE（SharePoint 模式）  
 用于在 SharePoint 模式下安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的输入设置是 **/RSSHPINSTALLMODE**。 该输入设置具有一个选项：SharePointFilesOnlyMode。 此选项将安装 SharePoint 模式所需的所有文件，但以下安装需要进行配置。 使用 SharePoint 管理中心完成其他配置步骤。 有关详细信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)。  
  
### <a name="examples-of-sharepoint-mode-installation"></a>SharePoint 模式安装示例  
 下面的示例将在 SharePoint 模式中安装 SQL Server 数据库引擎服务和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 以及 SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序 (RS_SHPWFE)。  
  
```  
setup /q /ACTION=install /FEATURES=SQL, RS_SHP, RS_SHPWFE,TOOLS /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="BUILTIN\ADMINISTRATORS" /RSSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE" /SQLSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE" /AGTSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE"  
```  
  
 下面的示例仅安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式。  
  
```  
Setup.exe /q /ACTION="Install" /IACCEPTSQLSERVERLICENSETERMS /FEATURES="RS_SHP" /INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /INSTALLSHAREDDIR="C:\Program Files\Microsoft SQL Server" /INSTALLSHAREDWOWDIR="C:\Program Files (x86)\Microsoft SQL Server" /INSTALLSQLDATADIR="C:\Program Files\Microsoft SQL Server" /SECURITYMODE="SQL" /SAPWD="[PASSWORD]" /PID="[Your PID Value]" /SQLSYSADMINACCOUNTS="[Account Name]" "AutoSql Admin Group" /ASSYSADMINACCOUNTS="[Account Name]" /UPDATEENABLED="False"  
  
```  
  
### <a name="examples-of-sharepoint-mode-upgrade"></a>SharePoint 模式升级示例  
 下面的示例将升级 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式。 **RSUPGRADEPASSWORD** 是现有 Report Server 服务帐户的密码。 RSUPGRADEPASSWORD 是升级方案中必需的字段，除非 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户是内置帐户。  
  
```  
Setup.exe /q /ACTION="Upgrade" /INSTANCENAME="MSSQLSERVER" /PID="[PID value]" /FTSVCACCOUNT="[DOMAIN\ACCOUNT]" /FTSVCPASSWORD="[PASSWORD]" /UPDATEENABLED="False" /IACCEPTSQLSERVERLICENSETERMS /RSUPGRADEPASSWORD="[PASSWORD]"  
```  
  
 下面的示例可用于升级基于 SharePoint 共享服务体系结构的 SharePoint 模式安装。 该示例使用 ALLOWUPGRADEFORSSRSSHAREPOINTMODE 开关。 升级不基于共享服务体系结构的旧版本不需要该开关：  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
```  
Setup.exe /q /ACTION="Upgrade" /INSTANCENAME="MSSQLSERVER" /PID="[Your PID Value]" /FTSVCACCOUNT="[ACCOUNT Name]" /FTSVCPASSWORD="[PASSWORD]" /UPDATEENABLED="False" /IACCEPTSQLSERVERLICENSETERMS /ALLOWUPGRADEFORSSRSSHAREPOINTMODE="True"  
```

## <a name="next-steps"></a>后续步骤

[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[SysPrep 参数](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#SysPrep)   
[从命令提示符安装 Power Pivot](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013#bkmk_install)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)