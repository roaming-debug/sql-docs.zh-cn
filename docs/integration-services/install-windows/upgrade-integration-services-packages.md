---
description: 升级 Integration Services 包
title: 升级 Integration Services 包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, migrating
- migrating packages [Integration Services]
ms.assetid: 68dbdf81-032c-4a73-99f6-41420e053980
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: f064880f44b25a96ab5f93e15ee27c361182ba81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352045"
---
# <a name="upgrade-integration-services-packages"></a>升级 Integration Services 包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在将 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 实例升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本时，现有的 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 包不会自动升级到当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 所使用的包格式。 您必须选择一种升级方法并手动升级包。  
  
 有关在将项目转换为项目部署模型时升级包的信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。
  
## <a name="selecting-an-upgrade-method"></a>选择升级方法  
 可以使用各种方法来升级 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 包。 对于某些方法，升级只是临时的。 对于其他方法，升级将是永久的。 下表描述了每种升级方法以及升级是临时的还是永久的。  
  
> [!NOTE]  
>  当你使用随当前版本的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]一起安装的 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]dtexec [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]实用工具 (dtexec.exe) 运行 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 、 **、** 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]包时，临时包升级将增加执行时间。 包执行时间的增加比率将由包的大小决定。 为了避免增加执行时间，建议您在运行包之前对包进行升级。  
  
|升级方法|升级类型|  
|--------------------|---------------------|  
|使用随当前版本的 **安装的** dtexec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实用工具 (dtexec.exe) 来运行 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 包。<br /><br /> 有关详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。|包升级是临时的。<br /><br /> 无法保存所做的更改。|  
|在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中打开 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]包文件。|如果保存该包，则包升级是永久的；否则，如果不保存该包，则包升级是临时的。|  
|将 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 包添加到 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的现有项目中。|包升级是永久的。|  
|在 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 中打开 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]或更高版本的项目文件，然后使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包升级向导升级项目中的多个包。<br /><br /> 有关详细信息，请参阅 [使用 SSIS 包升级向导升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md) 和 [SSIS 包升级向导的 F1 帮助](../../integration-services/ssis-package-upgrade-wizard-f1-help.md)。|包升级是永久的。|  
|使用随当前版本的 <xref:Microsoft.SqlServer.Dts.Runtime.Application.Upgrade%2A> 方法升级一个或多个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。|包升级是永久的。|  
  
## <a name="custom-applications-and-custom-components"></a>自定义应用程序和自定义组件  
 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] 自定义组件将不与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的当前版本一起使用。  
  
 可以使用当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 工具运行和管理包含 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssIS](../../includes/ssis-md.md)] 自定义组件的包。 我们向以下文件添加了四个绑定重定向规则，以帮助将运行时程序集从版本 10.0.0.0 ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)])、版本 11.0.0.0 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) 或版本 12.0.0.0 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) 重定向到版本 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)])。  
  
-   DTExec.exe.config  
  
-   dtshost.exe.config  
  
-   DTSWizard.exe.config  
  
-   DTUtil.exe.config  
  
-   DTExecUI.exe.config  
  
 若要使用 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 设计包含 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 自定义组件的包，需要修改位于 \<drive>:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE 的 devenv.exe.config 文件。  
  
 若要将这些包用于使用 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]的运行时生成的客户应用程序，则在可执行文件的 *.exe.config 文件的配置部分中包含重定向规则。 这些规则将运行时程序集重定向到版本 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)])。 有关程序集版本重定向的详细信息，请参阅 [\<runtime> 的 \<assemblyBinding> 元素](/dotnet/framework/configure-apps/file-schema/runtime/assemblybinding-element-for-runtime)。  
  
### <a name="locating-the-assemblies"></a>定位程序集  
 在 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 程序集已升级到 .NET 4.0。 位于 \<drive>:\Windows\Microsoft.NET\assembly 中的 .NET 4 存在单独的全局程序集缓存。 您可在此路径下找到所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 程序集，一般位于 GAC_MSIL 文件夹中。  
  
 与之前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一样，核心 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 扩展性 .dll 文件也位于 \<drive>:\Program Files\Microsoft SQL Server\130\SDK\Assemblies 中。  
  
## <a name="understanding-sql-server-package-upgrade-results"></a>了解 SQL Server 包升级结果  
 在包升级过程中， [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 包中的大部分组件和功能都无缝地转换为其在当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的对应部分。 但是，有一些组件和功能，它们要么无法升级，要么升级的结果值得引起您的注意。 下表确定了这些组件和功能。  
  
> [!NOTE]  
>  若要确定哪些包具有该表中列出的问题，请运行升级顾问。  
  
|组件或功能|升级结果|  
|--------------------------|---------------------|  
|连接字符串|对于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 包，某些提供程序的名称已更改，而且需要在连接字符串中使用不同的值。 若要更新连接字符串，请使用下列过程之一：<br /><br /> 使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包升级向导升级包，并选择 **“更新连接字符串以使用新的提供程序名称”** 选项。<br /><br /> 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，在“选项”对话框的“常规”页上，选择 **“更新连接字符串以使用新的提供程序名称”** 选项。 有关此选项的详细信息，请参阅“常规页”。<br /><br /> 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开该包并手动更改 ConnectionString 属性的文本。<br /><br /> 注意：在连接字符串存储于配置文件或数据源文件中，或表达式设置了 **ConnectionString** 属性时，不能使用上述过程更新连接字符串。 若要在这两种情况下更新连接字符串，必须手动更新文件或表达式。<br /><br /> 有关数据源的详细信息，请参阅 [数据源](../../integration-services/connection-manager/data-sources.md)。|  
  
### <a name="scripts-that-depend-on-adodbdll"></a>依赖于 ADODB.dll 的脚本  
 在未安装 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的计算机上，可能不能升级或运行显式引用 ADODB.dll 的“脚本任务”和“脚本组件”脚本。 为了升级这些“脚本任务”或“脚本组件”脚本，建议你删除对 ADODB.dll 的依赖关系。  Ado.Net 是建议用于托管代码（如 VB 和 C# 脚本）的替代项。  
  
