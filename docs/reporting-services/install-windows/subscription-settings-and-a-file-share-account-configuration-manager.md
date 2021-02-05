---
title: 订阅设置和文件共享帐户（配置管理器）| Microsoft Docs
description: 使用报表服务器配置管理器的“订阅设置”页，为本机模式报表服务器和文件共享订阅配置文件共享帐户。
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5239174f08c9922a31204b4f476f923a7e931567
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236229"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>订阅设置和文件共享帐户（报表服务器配置管理器）
  使用 **配置管理器的**[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] “订阅设置”页为本机模式报表服务器和文件共享订阅配置文件共享帐户。 文件共享帐户运行使用将报表传递给文件共享的多个订阅中的单组凭据。 需要更改凭据时，可以为文件共享帐户配置更改并且无需更新每个单独的订阅。  
  
 两个工作流与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文件共享订阅共存：  
  
-   使用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 版本中的新增功能， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 管理员可以配置单个文件共享帐户，用于一个或多个订阅。 配置  “指定文件共享帐户”，然后在单独的订阅配置页中，用户选择“使用文件共享帐户”  。  
  
-   使用目标文件共享的特定凭据配置单独的订阅。  
  
-   你还可以混合使用这两种方法，使一些文件共享订阅使用中央文件共享帐户，而其他订阅使用特定的凭据。  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式。  
  
## <a name="specify-a-file-share-account"></a>指定文件共享帐户  
 如果选择此选项，你将能够提供帐户以用于从报表服务器访问文件共享。 如果配置文件共享帐户，则所有用户均可以选择配置用于将报表提交给文件共享的任何订阅的帐户。 如果未选中此选项，则文件共享帐户在任何订阅上均 **不** 可用。  
  
 请注意，对于配置为文件共享帐户的帐户，你需要验证其是否具有对用户将用于文件共享传递的任何文件共享的读取和写入权限。  
  
 下图是配置用于文件共享传递的订阅上显示的内容。 如果尚未配置文件共享帐户，则将禁用“使用文件共享帐户”  。  
  
 ![配置管理器文件共享帐户](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "配置管理器文件共享帐户")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>阻止权限提升或提升的权限  
  
> [!IMPORTANT]
> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户可控制订阅传递，并且可以与用于文件共享订阅的帐户进行交互。 Windows 安全功能限制以下两者的组合：1) [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户和 2) 用于文件共享帐户的帐户。 例如，如果内置操作系统帐户用于文件共享帐户，则 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务帐户必须是另一个具有模拟权限的服务帐户。 如果配置了一个显式文件共享帐户和密码，则文件共享帐户需要具有登录到运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的计算机的权限。 如果文件共享帐户没有所需的权限，则使用文件共享帐户的订阅将失败，并会显示类似于以下内容的错误消息：  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>审核文件共享帐户的使用情况的 PowerShell 示例  
 运行以下 Windows PowerShell 脚本以列出所有配置为使用“文件共享帐户”的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅  。 将报表服务器的 `SERVERNAME` 更新为相应的值。  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 该脚本的输出与以下类似：  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 中的文件共享传递](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [创建和管理本机模式报表服务器的订阅](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
