---
description: SSIS 包升级向导的 F1 帮助
title: SSIS 包升级向导 F1 帮助 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
f1_keywords:
- sql13.is.upgradewizard.ssisupgradewizard.f1
- sql13.is.upgradewizard.selectsourcelocation.f1
- sql13.is.upgradewizard.selectdestinationlocation.f1
- sql13.is.upgradewizard.selectpackagemgtoptions.f1
- sql13.is.upgradewizard.selectpackages.f1
- sql13.is.upgradewizard.completewizard.f1
- sql13.is.upgradewizard.upgradingpackage.f1
ms.assetid: 7fe886ff-1ea5-48d5-9d20-d5da36dd1cd7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9e47bae183dc2d0e790a6f0cb6be207d30cd4834
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495501"
---
# <a name="ssis-package-upgrade-wizard-f1-help"></a>SSIS 包升级向导的 F1 帮助

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  使用“SSIS 包升级向导”可将由较早版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 创建的包升级到当前版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 的包格式。  
  
 **运行 SSIS 包升级向导**  
  
-   [使用 SSIS 包升级向导升级 Integration Services 包](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)  

## <a name="ssis-upgrade-wizard"></a>SSIS 升级向导
  
### <a name="options"></a>选项  
 **不再显示此页。**  
 下次打开向导时跳过“欢迎”页。  
 
## <a name="select-source-location-page"></a>“选择源位置”页
 使用 **“选择源位置”** 页可指定从中升级包的源。  
  
> [!NOTE]  
>  仅当通过 [!INCLUDE[ssIS](../includes/ssis-md.md)] 或命令提示符运行 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 包升级向导时，此页才可用。  
  
### <a name="static-options"></a>静态选项  
 **包源**  
 选择包含要升级包的存储位置。 此选项具有下表所列的值。  
  
|值|说明|  
|-----------|-----------------|  
|**文件系统**|指示要升级的包位于本地计算机上的文件夹中。<br /><br /> 若要使向导在升级这些包前备份原始包，必须在文件系统中必须存储原始包。 有关详细信息，请参阅操作指南主题。|  
|**SSIS 包存储区**|指示要升级的包位于包存储区中。 包存储区由一组 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务管理的系统文件夹组成。 有关详细信息，请参阅[包管理（SSIS 服务）](../integration-services/service/package-management-ssis-service.md)。<br /><br /> 选择此值将显示相应的动态选项 **Package source** 。|  
|**Microsoft SQL Server**|指示要升级的包来自 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的现有实例。<br /><br /> 选择此值将显示相应的动态选项 **Package source** 。|  
  
 **文件夹**  
 键入要升级的包所在的文件夹名称或单击“浏览”找到该文件夹。  
  
 **“浏览”**  
 浏览找到要升级的包所在的文件夹。  
  
### <a name="package-source-dynamic-options"></a>包源动态选项  
  
#### <a name="package-source--ssis-package-store"></a>包源 = SSIS 包存储区  
 **Server**  
 键入要升级的包所在的服务器的名称，或在列表中选择此服务器。  
  
#### <a name="package-source--microsoft-sql-server"></a>包源 = Microsoft SQL Server  
 **Server**  
 键入要升级的包所在的服务器的名称，或从列表中选择此服务器。  
  
 **使用 Windows 身份验证**  
 选择此选项可以使用 Windows 身份验证连接到服务器。  
  
 **使用 SQL Server 身份验证**  
 选择此选项可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证连接到服务器。 如果使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证，则必须提供用户名和密码。  
  
 **用户名**  
 键入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证用于连接到服务器的用户名。  
  
 **密码**  
 键入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证用于连接到服务器的密码。  
 
## <a name="select-destination-location-page"></a>“选择目标位置”页
 使用 **“选择目标位置”** 页可以指定要将升级包保存到的目标位置。  
  
> [!NOTE]  
>  仅当通过 [!INCLUDE[ssIS](../includes/ssis-md.md)] 或命令提示符运行 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 包升级向导时，此页才可用。  
 
### <a name="static-options"></a>静态选项  
 **保存到源位置**  
 将升级的包保存到在向导的“选择源位置”页上指定的位置。  
  
 如果原始包存储在文件系统中，并且您希望向导备份这些包，请选择 **“保存到源位置”** 选项。 有关详细信息，请参阅 [使用 SSIS 包升级向导升级 Integration Services 包](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)。  
  
 **选择新的目标位置**  
 将升级的包保存到此页上指定的目标位置。  
  
 **包源**  
 指定存储升级包的位置。 此选项具有下表所列的值。  
  
|值|说明|  
|-----------|-----------------|  
|**文件系统**|指示将升级的包将保存到本地计算机上的文件夹中。|  
|**SSIS 包存储区**|指示升级的包将保存到 Integration Services 包存储区中。 包存储区由一组 Integration Services 服务管理的文件系统文件夹组成。 有关详细信息，请参阅[包管理（SSIS 服务）](../integration-services/service/package-management-ssis-service.md)。<br /><br /> 选择此值将显示相应的动态选项 **“包源”** 。|  
|**Microsoft SQL Server**|指示升级的包将保存到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的现有实例中。<br /><br /> 选择此值将显示相应的动态选项 **“包源”** 。|  
  
 **文件夹**  
 键入要保存升级包的文件夹的名称，或单击“浏览”找到该文件夹。  
  
 **“浏览”**  
 浏览找到将保存已升级包的文件夹。  
  
### <a name="package-source-dynamic-options"></a>包源动态选项  
  
#### <a name="package-source--ssis-package-store"></a>包源 = SSIS 包存储区  
 **Server**  
 键入要保存升级包的服务器的名称，或在列表中选择服务器。  
  
#### <a name="package-source--microsoft-sql-server"></a>包源 = Microsoft SQL Server  
 **Server**  
 键入要保存升级包的服务器的名称，或在列表中选择此服务器。  
  
 **使用 Windows 身份验证**  
 选择此选项可以使用 Windows 身份验证连接到服务器。  
  
 **使用 SQL Server 身份验证**  
 选择此选项可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证连接到服务器。 如果使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证，则必须提供用户名和密码。  
  
 **用户名**  
 键入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证连接到服务器时使用的用户名。  
  
 **密码**  
 键入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 身份验证连接到服务器时使用的密码。  
 
## <a name="select-package-management-options-page"></a>“选择包管理选项”页
  使用 **“选择包管理选项”** 页指定用于对包进行升级的选项。  
  
 **运行 SSIS 包升级向导**  
  
-   [使用 SSIS 包升级向导升级 Integration Services 包](../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)  
  
### <a name="options"></a>选项  
 **更新连接字符串以使用新的提供程序名称**  
 更新连接字符串以使用当前版本的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]的下列提供程序的名称：  
  
-   用于 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 的 OLE DB 提供程序  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] 包升级向导仅更新存储在连接管理器中的连接字符串。 向导不会更新通过使用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 表达式语言或在脚本任务中使用代码动态构造的连接字符串。  
  
 **验证升级包**  
 验证升级包，并仅保存通过验证的升级包。  
  
 如果未选择此选项，则向导将不会验证升级包。 因此，向导将保存所有升级包，不论包是否有效。 向导会将升级包保存到在向导的“选择目标位置”页上指定的目标。  
  
 验证会延长升级过程的时间。 对于可能成功升级的大型包，建议不要选择此选项。  
  
 **创建新的包 ID**  
 为升级包创建新的包 ID。  
  
 **包升级失败时继续执行升级过程**  
 指定当无法升级某个包时 [!INCLUDE[ssIS](../includes/ssis-md.md)] 包升级向导继续升级剩余包。  
  
 **包名称冲突**  
 指定向导应如何处理同名的包。 此选项具有下表所列的值。  
  
 **覆盖现有包文件**  
 使用同名的升级包替换现有包。  
  
 **添加数值后缀以升级包名称**  
 向升级包名称添加数值后缀。  
  
 **不要升级包**  
 停止升级包，并在完成向导时显示错误。  
  
 当在向导的 **“选择目标位置”** 页上选择 **“保存到源位置”** 选项时，上述选项不可用。  
  
 **忽略包配置**  
 包在升级过程中未加载包配置。 选择此选项可缩短升级程序包所需的时间。  
  
 **备份原始包**  
 使向导将原始包备份到 **SSISBackupFolder** 文件夹。 向导会创建 **SSISBackupFolder** 文件夹，将其作为包含原始包和升级包的文件夹的子文件夹。  
  
> [!NOTE]  
>  仅当指定将原始包和升级的包都存储在文件系统中且在同一文件中时，此选项才可用。  

## <a name="select-packages-page"></a>“选择包”页
  可以使用 **“选择包”** 页选择要升级的包。 此页列出了在向导的 **“选择源位置”** 页上指定的位置中存储的包。  
  
### <a name="options"></a>选项  
 **现有包名称**  
 选择一个或多个要升级的包。  
  
 **升级包名称**  
 提供目标包的名称或使用向导提供的默认名称。  
  
> [!NOTE]  
>  还可以在升级包后更改目标包名称。 在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 或 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]中，打开升级的包并更改包名称。  
  
 **密码**  
 指定用于解密选定升级包的密码。  
  
 **应用于所选内容**  
 应用指定密码来解密选定的升级包。  
 
## <a name="complete-the-wizard-page"></a>“完成向导”页
  可以使用 **“完成该向导”** 页查看并确认所选的包升级选项。 这是向导的最后一页，您可以返回并更改向导此会话的选项。  
  
### <a name="options"></a>选项  
 **选项摘要**  
 查看您在向导中选择的升级选项。 若要更改任意选项，请单击 **“上一步”** 返回到向导前面的页
 
## <a name="upgrading-the-packages-page"></a>“正在升级包”页
  可以使用 **“升级包”** 页查看包升级的进度以及中断升级过程。 [!INCLUDE[ssIS](../includes/ssis-md.md)] 包升级向导会逐一升级所选包。  
  
### <a name="options"></a>选项  
 **消息窗格**  
 在升级过程中显示进度消息和摘要信息。  
  
 **操作**  
 查看升级中的操作。  
  
 **Status**  
 查看每个操作的结果。  
  
 **消息**  
 查看每个操作生成的错误消息。  
  
 **Stop**  
 停止包升级。  
  
 **Report**  
 选择希望如何处理包含包升级结果的报告：  
  
-   联机查看报告。  
  
-   将报告保存到文件中。  
  
-   将报告复制到剪贴板  
  
-   将报告作为电子邮件发送。  

## <a name="view-upgraded-packages"></a>验证升级的包
### <a name="view-upgraded-packages-that-were-saved-to-a-sql-server-database-or-to-the-package-store"></a>查看保存到 SQL Server 数据库或包存储区的升级包
  
在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]中的对象资源管理器中，连接到 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]的本地实例，然后展开 **“已存储的包”** 节点查看已升级的包。  
  
### <a name="view-upgraded-packages-that-were-upgraded-from-sql-server-data-tools"></a>查看从 SQL Server Data Tools 升级的升级包  
  
在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中的解决方案资源管理器中，打开 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目，然后展开 **“SSIS 包”** 节点查看已升级的包。  
  
## <a name="see-also"></a>另请参阅  
 [升级 Integration Services 包](../integration-services/install-windows/upgrade-integration-services-packages.md)  
  
  
