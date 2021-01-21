---
title: 注册标准 .NET Framework 数据提供程序 | Microsoft Docs
description: 注册标准 .NET Framework 数据提供程序，从而使用第三方 .NET Framework 数据提供程序检索 Reporting Services 报表数据集的数据。
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], data
- .NET Framework data providers for Reporting Services
- data processing extensions [Reporting Services]
- data providers [Reporting Services]
- data retrieval [Reporting Services]
- Reporting Services, data sources
ms.assetid: d92add64-e93c-4598-8508-55d1bc46acf6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6539c9552206893f5799fbeb30ff628355dd137d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596943"
---
# <a name="register-a-standard-net-framework-data-provider-ssrs"></a>注册标准 .NET Framework 数据访问接口 (SSRS)
  若要使用第三方 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序检索 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表数据集的数据，需要在以下两个位置部署和注册 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序程序集：报表创作客户端和报表服务器。 在报表创作客户端上，必须将数据访问接口注册为数据源类型并将其与查询设计器相关联。 然后，可以在创建报表数据集时选择此数据访问接口作为数据源类型。 关联的查询设计器会打开，帮助您为此数据源类型创建查询。 在报表服务器上，必须将该数据访问接口注册为数据源类型。 然后，可以处理使用此数据访问接口从数据源检索数据的已发布报表。  
  
 第三方数据提供程序不一定提供 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件提供的所有功能。 有关详细信息，请参阅 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。 若要了解扩展 . [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序的功能，请参阅 [实现数据处理扩展插件](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)。  
  
 安装和注册数据访问接口需要管理员凭据。  
  
## <a name="registering-a-net-framework-data-provider-on-the-report-server"></a>在报表服务器上注册 .NET Framework 数据访问接口  
 若要在报表服务器上处理使用此 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的已发布报表，需要在报表服务器上安装其程序集。 必须修改两个配置文件。 修改 rsreportserver.config 以注册数据访问接口。 修改 rssrvpolicy.config 以授予对程序集的代码访问安全权限。  
  
#### <a name="to-install-a-data-provider-assembly-on-the-report-server"></a>在报表服务器上安装数据访问接口程序集  
  
1.  在要在其上使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的报表服务器上，导航到 bin 目录的默认位置。 报表服务器 bin 目录的默认位置为 \<drive>:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin。  
  
2.  将程序集从临时位置复制到报表服务器的 bin 目录中。 也可以选择将程序集加载到全局程序集缓存 (GAC) 中。 有关详细信息，请参阅 MSDN 上的 [SDK 文档中的](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac) Working with Assemblies and the Global Assembly Cache [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] （使用程序集和全局程序集缓存）。  
  
#### <a name="to-register-a-net-data-provider-on-the-report-server"></a>在报表服务器上注册 .NET 数据访问接口  
  
1.  在 bin 目录的 ReportServer 父目录中备份 RSReportServer.config 文件。  
  
2.  打开 RSReportServer.config。您可以使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或诸如记事本之类的简单文本编辑器打开该配置文件。  
  
3.  在 RSReportServer.config 文件中找到 **Data** 元素。 应当在以下位置为 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口生成一个条目：  
  
    ```  
    <Extensions>  
       <Data>  
          <Extension Your data provider configuration information goes here />  
       </Data>  
    </Extensions>  
    ```  
  
4.  添加 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的条目。  
  
    |特性|描述|  
    |---------------|-----------------|  
    |**Name**|提供数据访问接口的唯一名称，例如 **MyNETDataProvider**。 **Name** 属性的最大长度是 255 个字符。 该名称在配置文件的 **Extension** 元素内的所有条目中必须唯一。 创建新数据源时，此处包含的值显示在数据源类型下拉列表中。|  
    |类型|输入包括实现 <xref:System.Data.IDbConnection> 接口的类的完全限定命名空间在内的逗号分隔的列表，后跟 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序程序集的名称（不包含 .dll 文件扩展名）。|  
  
     例如，对于部署到报表服务器的 bin 目录中的 DLL，该条目应如下所示：  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly" />   
    ```  
  
     如果将程序集加载到全局程序集缓存 (GAC) 中，必须提供强名称属性。 例如：  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly,Version=1.0.0.0, Culture=neutral, PublicKeyToken=MyPublicToken"/>  
    ```  
  
#### <a name="to-set-the-code-group-policy-for-a-net-data-provider"></a>设置 .NET 数据访问接口的代码组策略  
  
1.  在 bin 目录的 ReportServer 父目录中创建 rssrvpolicy.config 文件的备份副本。  
  
2.  打开 rssrvpolicy.config。您可以使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或诸如记事本之类的简单文本编辑器打开该配置文件。  
  
3.  在 rssrvpolicy.config 文件中找到 **CodeGroup** 元素。  
  
4.  为数据访问接口程序集添加授予 **FullTrust** 权限的代码组。 该代码组应如下所示：  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="ThisDataProviderCodeGroup"  
       Description="Code group for the .NET data provider">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url=  
    "C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\DataProviderAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 URL 成员身份仅是您可能为数据访问接口选择的多个成员身份条件之一。  
  
### <a name="verifying-the-deployment-and-registration"></a>验证部署和注册  
 打开 Web 门户，然后验证可用数据源列表中是否包含该数据访问接口，从而验证是否已将该数据访问接口成功部署到报表服务器上。 有关 Web 门户和数据源的详细信息，请参阅[创建、修改和删除共享数据源 (SSRS)](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)。  
  
## <a name="registering-a-net-framework-data-provider-on-the-report-designer-client"></a>在报表设计器客户端上注册 .NET Framework 数据访问接口  
 若要创作使用此 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口作为数据源的报表，必须在运行报表设计器的客户端计算机上安装该程序集。 必须修改两个配置文件。 修改 RSReportDesigner.config 以将该数据访问接口注册为数据源，并使用通用查询设计器。 修改 RSPreviewPolicy.config 以授予对数据访问接口程序集的代码访问安全权限。  
  
#### <a name="to-install-a-data-provider-assembly-on-the-report-designer-client"></a>在报表设计器客户端上安装数据访问接口程序集  
  
1.  在要在其上使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的报表设计器客户端上，导航到 PrivateAssemblies 目录的默认位置。 PrivateAssemblies 目录的默认位置为 \<drive>:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies。  
  
2.  将程序集从临时位置复制到报表设计器客户端的 PrivateAssemblies 目录中。 也可以选择将程序集加载到全局程序集缓存 (GAC) 中。 有关详细信息，请参阅 MSDN 上的 [SDK 文档中的](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac) Working with Assemblies and the Global Assembly Cache [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] （使用程序集和全局程序集缓存）。  
  
#### <a name="to-register-a-net-data-provider-on-the-report-designer-client"></a>在报表设计器客户端上注册 .NET 数据访问接口  
  
1.  在 PrivateAssemblies 目录中创建 RSReportDesigner.config 文件的备份副本。  
  
2.  使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或诸如记事本之类的简单文本编辑器打开 RSReportDesigner.config。  
  
3.  在 RSReportDesigner.config 文件中找到 **Data** 元素。 应当在以下位置为该数据访问接口生成一个条目：  
  
    ```  
    <Extensions>  
       <Data>  
          <Extension Your data provider configuration information goes here />  
       </Data>  
    </Extensions>  
    ```  
  
4.  添加该数据访问接口的条目。  
  
    |特性|描述|  
    |---------------|-----------------|  
    |**Name**|提供数据访问接口的唯一名称，例如 **MyNETDataProvider**。 **Name** 属性的最大长度是 255 个字符。 该名称在配置文件的 **Extension** 元素内的所有条目中必须唯一。 创建新数据源时，在此处包含的值显示在数据源类型下拉列表中。|  
    |类型|输入包括实现 <xref:System.Data.IDbConnection> 接口的类的完全限定命名空间在内的逗号分隔的列表，后跟 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序程序集的名称（不包含 .dll 文件扩展名）。|  
  
     例如，对于部署到 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 的 PrivateAssemblies 目录中的 DLL，该条目应如下所示：  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly" />   
    ```  
  
     如果将程序集加载到 GAC 中，必须提供强名称属性。 例如：  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly, Version=1.0.0.0, Culture=neutral, PublicKeyToken=MyPublicToken"/>  
    ```  
  
5.  在 RSReportDesigner.config 文件中找到 **Designer** 元素。 应当在以下位置为 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口生成一个条目：  
  
    ```  
    <Extensions>  
       <Designer>  
          <Your data provider configuration information goes here>  
       </Designer>  
    </Extensions>  
    ```  
  
6.  将以下条目添加到 RSReportDesigner.config 文件中的 **Designer** 元素下。 您只需要使用在以前的条目中提供的名称替换 **Name** 属性。  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="Microsoft.ReportingServices.QueryDesigners.GenericQueryDesigner,Microsoft.ReportingServices.QueryDesigners"/>  
    ```  
  
#### <a name="to-set-the-code-group-policy-for-a-net-data-provider-on-the-report-designer-client"></a>在报表设计器客户端上设置 .NET 数据访问接口的代码组策略  
  
1.  在 PrivateAssemblies 目录中创建 RSPreviewPolicy.config 文件的备份副本。  
  
2.  使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或诸如记事本之类的简单的文本编辑器打开 RSPreviewPolicy.config。  
  
3.  在 RSPreviewPolicy.config 文件中找到 **CodeGroup** 元素。  
  
4.  为 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序程序集添加授予 **FullTrust** 权限的代码组。 该代码组应如下所示：  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="ThisDataProviderCodeGroup"  
       Description="Code group for the .NET data provider">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url=  
    " C:\Program Files\Microsoft Visual Studio 9\Common7\IDE\PrivateAssemblies\DataProviderAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 URL 成员身份仅是您可能为数据访问接口选择的多个成员身份条件之一。  
  
### <a name="verifying-the-deployment-and-registration-on-the-report-designer-client"></a>在报表设计器客户端上验证部署和注册  
 必须先关闭本地计算机上的所有 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 实例，然后才能验证部署。 结束所有当前会话之后，可以在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]中创建一个新报表项目，以验证数据访问接口是否已成功部署到报表设计器。 为报表创建新的数据集时，该数据访问接口应当包含在可用数据源类型列表中。  
  
## <a name="platform-considerations"></a>平台注意事项  
 在 64 位 (x64) 平台上， [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 在 32 位 WOW 模式下运行。 在 x64 平台上创作报表时，需要在报表创作客户端上安装 32 位数据访问接口，以便预览报表。 如果在同一系统上发布报表，则需要 x64 数据提供程序，以便在 Web 门户中查看报表。  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 不受基于 [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)]的平台支持。  
  
 必须在每个平台上对随 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装的数据处理扩展插件进行本机编译，然后将其安装在正确位置。 如果注册自定义数据访问接口或标准 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口，则需要在对应平台上对其进行本机编译，然后将其安装在相应位置。 如果在 32 位平台上运行，则必须为此 32 位平台编译数据访问接口。 如果在 64 位平台上运行，则必须为此 64 位平台编译数据访问接口。 不能在 64 位平台上使用采用 64 位接口包装的 32 位数据访问接口。 有关数据访问接口是否可在所安装平台上工作的信息，请查看您的第三方软件。 有关数据提供程序和平台支持的详细信息，请参阅 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [配置和管理报表服务器（SSRS 本机模式）](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [实现数据处理扩展插件](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 配置文件](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Reporting Services 中的代码访问安全性](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)  
  
