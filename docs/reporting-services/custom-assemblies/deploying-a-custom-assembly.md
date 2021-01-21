---
title: 部署自定义程序集 | Microsoft Docs
description: 了解如何在 SQL Server Reporting Services 中部署自定义程序集。 此外，了解如何授予自定义程序集执行权限以外的特权。
ms.date: 11/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- deploying custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], deploying
- custom assemblies [Reporting Services], updating
- updating custom assemblies
ms.assetid: c6674cd8-0de7-4a5a-9e7c-12ffa49f6fd2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9aa3a01e88b699f184ea05dfcb3650b4653837cb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595505"
---
# <a name="deploying-a-custom-assembly"></a>部署自定义程序集
  为了在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中部署自定义程序集，请将程序集同时放到报表设计器和报表服务器的应用程序文件夹中。 默认情况下，将在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中向自定义程序集授予 Execution 权限  。 若要向自定义程序集授予超过执行权限的特权，需要对报表服务器编辑 rssrvpolicy.config 配置文件，并对报表设计器预览窗口编辑 rspreviewpolicy.config 配置文件。 也可以选择将自定义程序集安装到全局程序集缓存 (GAC) 中。  
  
> [!NOTE]  
>  报表设计器有两种预览模式：预览选项卡以及在以 DebugLocal 模式启动报表项目时启动的弹出式预览窗口  。 “预览”选项卡使用 FullTrust 权限集执行所有报表表达式，它不应用安全策略设置  。 弹出式预览窗口旨在模拟报表服务器的功能，因此具有策略配置文件，您或管理员必须修改该文件才能在报表设计器中使用自定义程序集。 此弹出式预览还锁定自定义程序集。 因此，您需要关闭预览窗口，才能修改或更新自定义程序集代码。  
  
## <a name="to-deploy-a-custom-assembly-in-reporting-services"></a>在 Reporting Services 中部署自定义程序集  
  
1.  将自定义程序集从生成位置复制到报表服务器的 bin 文件夹或报表设计器文件夹中。 

     如果将自定义程序集置于报表服务器的 bin 文件夹中，则可以发布引用这些自定义程序集的报表。 报表服务器的 bin 文件夹的默认位置为：

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin
     ```
     Reporting Services 2017 及更高版本
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer\bin
     ```
     如果将其置于报表设计器文件夹中，则可以在报表设计器中运行并调试引用自定义程序集的报表。 报表设计器的默认位置为：

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```  
2.  打开相应的配置文件。 报表服务器的 rssrvpolicy.config 的默认位置为：

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer
     ```
     Reporting Services 2017 及更高版本
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer
     ```
     要为报表设计器更新的文件包括：

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportHost11.exe.config
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```    
3.  为自定义程序集添加代码组。 有关详细信息，请参阅[安全开发 (Reporting Services)](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)。  
  
## <a name="updating-custom-assemblies"></a>更新自定义程序集  
 有时，您可能需要更新当前正在由多个已发布报表引用的自定义程序集的版本。 如果该程序集已位于报表服务器的 bin 目录或报表服务器中，并且程序集的版本号以某种方式递增或更改，则当前发布的报表将不再正常工作。 需要在报表定义的 CodeModules 元素中更新所引用的程序集的版本，并重新发布报表。 如果您知道您将频繁地更新自定义程序集，并且您当前发布的报表需要引用新程序集，则您可能需要考虑在特定程序集的所有更新中使用同一个版本号。  
  
 如果您不需要当前发布的报表引用程序集的新版本，则可以将自定义程序集部署到全局程序集缓存。 全局程序集缓存可以保持同一个程序集的多个版本，这样，您当前的报表可以引用程序集的前一个版本，而新发布的报表可以引用更新后的程序集。 此外，另一个方法是设置报表服务器的绑定重定向，以强制将旧程序集的所有请求重定向到新程序集。 你需要修改报表服务器 Web.config 文件和报表服务器 ReportingServicesService.exe.config 文件。 该条目可能如下所示：  
  
```  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅  
 [将自定义程序集用于报表](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [使用程序集和全局程序集缓存](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac)  
  
