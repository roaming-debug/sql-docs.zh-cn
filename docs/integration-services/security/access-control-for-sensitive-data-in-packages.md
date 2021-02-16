---
description: 对包中敏感数据的访问控制
title: 对包中敏感数据的访问控制 | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.packageprotectionlevel.f1
- sql13.ssis.bids.projectprotectionlevel.f1
- sql13.dts.designer.packagepassword.f1
- sql13.ssis.bids.projectpassword.f1
helpviewer_keywords:
- passwords [Integration Services]
- packages [Integration Services], security
- protection levels for packages [Integration Services]
- configuration files [Integration Services]
- encryption [Integration Services]
- cryptography [Integration Services]
- security [Integration Services], protection levels
ms.assetid: d4b073c4-4238-41fc-a258-4e114216e185
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b85dc4949e363a8e62ca3d992a04365f7ce414f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353011"
---
# <a name="access-control-for-sensitive-data-in-packages"></a>对包中敏感数据的访问控制

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  为了保护 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中的数据，可以设置保护级别，以帮助仅保护包中的敏感数据或包中的所有数据。 另外，可以采用密码或用户密钥对数据加密，或依靠数据库对数据进行加密。 另外，您对包所采用的保护级别不一定是静态的，而是在包的整个生命周期内可能变化。 通常，您可以在包开发阶段设置一个保护级别，在包部署阶段设置另一个保护级别。  
  
> [!NOTE]  
>  除了本主题中所述的保护级别外，还可以使用固定数据库级角色保护保存到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包。  
  
## <a name="definition-of-sensitive-information"></a>定义敏感信息  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中，下列信息定义为“敏感” 信息：  
  
-   连接字符串的密码部分。 但是，如果选择加密所有数据的选项，则整个连接字符串都将被视为敏感信息。  
  
-   标记为敏感的任务生成的 XML 节点。 XML 节点的标记由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 控制，用户无法更改。  
  
-   标记为敏感的所有变量。 标记的变量由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]控制。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 认为属性是否敏感，主要取决于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件（连接管理器或任务）的开发人员是否将该属性指定为敏感。 用户不能向被视为敏感的属性列表添加属性，也不能从该列表删除属性。  
  
## <a name="encryption"></a>加密  
 加密（包保护级别所使用的加密）是通过使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 数据保护 API (DPAPI) 来执行的，DPAPI 是 Cryptography API (CryptoAPI) 的一部分。  
  
 使用密码加密包的包保护级别还要求您提供密码。 如果将保护级别从不使用密码的级别更改为使用密码的级别，则系统将提示您输入密码。  
  
 另外，对于使用密码的保护级别， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会使用 Triple DES 加密算法（其密钥长度为 192 位）， [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类库 (FCL) 中提供该算法。  
  
## <a name="protection-levels"></a>保护级别  
 下表介绍 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的保护级别。 括号中的值是来自 <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel> 枚举的值。 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中处理包时，这些值出现在用来配置包属性的“属性”窗口中。  
  
|保护级别|说明|  
|----------------------|-----------------|  
|不保存敏感数据 (**DontSaveSensitive**)|保存包时不保存包中敏感属性的值。 这种保护级别不进行加密，但它防止标记为敏感的属性随包一起保存，因此其他用户将无法使用这些敏感数据。 如果其他用户打开该包，敏感信息将被替换为空白，用户必须提供这些敏感信息。<br /><br /> 当与 **dtutil** 实用工具 (dtutil.exe) 一起使用时，此保护级别对应的值为 0。|  
|使用密码加密所有数据 (**EncryptAllWithPassword**)|使用密码加密整个包。 使用用户在创建包或导出包时提供的密码加密包。 用户必须提供包密码，才能在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中打开包，或使用 **dtexec** 命令提示符实用工具运行包。 如果没有密码，用户将无法访问或运行包。<br /><br /> 当与 **dtutil** 实用工具一起使用时，此保护级别对应的值为 3。|  
|使用用户密钥加密所有数据 (**EncryptAllWithUserKey**)|使用基于当前用户配置文件的密钥加密整个包。 只有创建或导出了包的用户才能在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中打开包，或使用 **dtexec** 命令提示符实用工具运行包。<br /><br /> 当与 **dtutil** 实用工具一起使用时，此保护级别对应的值为 4。<br /><br /> 注意：对于使用用户密钥的保护级别， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用 DPAPI 标准。 有关 DPAPI 的详细信息，请参阅 MSDN Library [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10))。|  
|使用密码加密敏感数据 (**EncryptSensitiveWithPassword**)|使用密码只加密包中敏感属性的值。 DPAPI 用于此加密。 敏感数据作为包的一部分保存，但数据是使用当前用户在创建包或导出包时提供的密码加密的。 若要在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中打开包，用户必须提供包密码。 如果不提供该密码，则包虽然可以打开但其中不包含敏感数据，当前用户必须为敏感数据提供新值。 如果用户试图在不提供密码的情况下执行包，则包执行将会失败。 有关密码和命令行执行的详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。<br /><br /> 当与 **dtutil** 实用工具一起使用时，此保护级别对应的值为 2。|  
|使用用户密钥加密敏感数据 (**EncryptSensitiveWithUserKey**)|使用基于当前用户配置文件的密钥只加密包中敏感属性的值。 只有使用同一配置文件的同一个用户才能加载此包。 如果其他用户打开该包，敏感信息将被替换为空白，当前用户必须为敏感数据提供新值。 如果用户试图执行该包，则包执行将会失败。 DPAPI 用于此加密。<br /><br /> 当与 **dtutil** 实用工具一起使用时，此保护级别对应的值为 1。<br /><br /> 注意：对于使用用户密钥的保护级别， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用 DPAPI 标准。 有关 DPAPI 的详细信息，请参阅 MSDN Library [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10))。|  
|依靠服务器存储进行加密 (**ServerStorage**)|使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库角色保护整个包。 将包保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb 数据库后，支持此选项。 此外，SSISDB 目录使用 **ServerStorage** 保护级别。<br /><br /> 在将包从 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]保存到文件系统时，不支持此选项。|  
  
## <a name="protection-level-setting-and-the-ssisdb-catalog"></a>保护级别设置和 SSISDB 目录  
 SSISDB 目录使用 **ServerStorage** 保护级别。 在向 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器部署 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目时，该目录会自动对包数据和敏感值加密。 该目录还会在检索数据时自动解密数据。  
  
 若将项目（.ispac 文件）从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器导出到文件系统，该系统会将保护级别自动更改为 **EncryptSensitiveWithUserKey**。 如果使用 **中的** “Integration Services 导入项目向导” [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]导入项目， **“属性”** 窗口中的 **ProtectionLevel** 属性将显示值 **EncryptSensitiveWithUserKey**。  
  
## <a name="protection-level-setting-based-on-package-life-cycle"></a>基于包的生命周期设置保护级别  
 在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中初次开发 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时，可以设置该包的保护级别。 以后当部署包时，在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中将包导入 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或从中导出包时，或者在将包从 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区或文件系统时，都可以更新包的保护级别。 例如，如果在计算机上使用某个用户密钥保护级别选项创建并保存包，则在将包提供给其他用户时，很可能需要更改保护级别，否则，他们将无法打开该包。  
  
 通常，您可以按下面列出的步骤更改保护级别：  
  
1.  在部署期间，将包的保护级别保留为默认值 **EncryptSensitiveWithUserKey**。 此设置可以保证只有开发人员可以看到包中的敏感值。 或者，您可以考虑使用 **EncryptAllWithUserKey** 或 **DontSaveSensitive**。  
  
2.  部署包时，您需要将保护级别更改为不依靠开发人员用户密钥的保护级别。 因此，通常需要选择 **EncryptSensitiveWithPassword** 或 **EncryptAllWithPassword**。 通过分配一个生产环境中运营团队也知道的临时强密码来加密包。  
  
3.  在将包部署到生产环境后，运营团队可以通过分配一个只有他们自己知道的强密码来重新加密部署的包。 他们也可以通过选择 **EncryptSensitiveWithUserKey** 或 **EncryptAllWithUserKey**，并使用要运行包的帐户的本地凭据来加密部署的包。  

## <a name="set-or-change-the-protection-level-of-packages"></a><a name="set_protection"></a> 设置或更改包的保护级别
  若要控制对包内容以及其中包含的敏感值（如密码）的访问，请设置 **ProtectionLevel** 属性的值。 项目中所含的包需要具有与项目相同的保护级别才能生成项目。 如果更改项目的 **ProtectionLevel** 属性设置，需要为包手动更新该属性设置。  
  
 有关 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中安全功能的概述，请参阅[安全性概述 (Integration Services)](../../integration-services/security/security-overview-integration-services.md)。  
  
 本主题中的过程介绍如何使用 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 或 dtutil 命令提示实用工具来更改 **ProtectionLevel** 属性的值。  
  
> [!NOTE]  
>  除了本主题中的过程外，当导入或导出包时，您通常可以设置或更改包的 **ProtectionLevel** 属性。 当使用 **ProtectionLevel** 导入和导出向导保存包时，您也可以更改包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 属性。  
  
### <a name="to-set-or-change-the-protection-level-of-a-package-in-sql-server-data-tools"></a>在 SQL Server Data Tools 中设置或更改包的保护级别  
  
1.  在[保护级别](#protection-levels)一节中查看 ProtectionLevel 属性的可用值，然后确定包的对应值。  
  
2.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含该包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中打开包。  
  
4.  如果“属性”窗口未显示包的属性，请单击设计图面。  
  
5.  在“属性”窗口中的 **“安全性”** 组，为 **ProtectionLevel** 属性选择相应的值。  
  
     如果选择的保护级别需要密码，请输入密码作为 **PackagePassword** 属性的值。  
  
6.  在 **“文件”** 菜单上，选择 **“保存选定项”** 以保存修改的包。  
  
### <a name="to-set-or-change-the-protection-level-of-packages-at-the-command-prompt"></a>在命令提示符下设置或更改包的保护级别  
  
1.  在[保护级别](#protection-levels)一节中查看 ProtectionLevel 属性的可用值，然后确定包的对应值。  
  
2.  在主题 **dtutil Utility** 中查看 [Encrypt](../../integration-services/dtutil-utility.md)选项的映射，然后确定要用作所选 **ProtectionLevel** 属性的值的相应整数。  
  
3.  打开命令提示符窗口。  
  
4.  在命令提示符下，导航到您要为其设置 **ProtectionLevel** 属性的包所在的文件夹。  
  
     下面步骤中所示的语法示例假设此文件夹是当前文件夹。  
  
5.  使用与下列示例之一相类似的命令，设置或更改包的保护级别。  
  
    -   下面的命令将文件系统中的单个包的 **ProtectionLevel** 属性设置为级别 2“使用密码加密敏感数据”，密码为“strongpassword”：  
  
         `dtutil.exe /file "C:\Package.dtsx" /encrypt file;"C:\Package.dtsx";2;strongpassword`  
  
    -   下面的命令将文件系统中特定文件夹内所有包的 **ProtectionLevel** 属性设置为级别 2“使用密码加密敏感数据”，密码为“strongpassword”：  
  
         `for %f in (*.dtsx) do dtutil.exe /file %f /encrypt file;%f;2;strongpassword`  
  
         如果您在批文件中使用类似的命令，则请输入文件占位符“%f”作为批文件中的“%%f”。  

## <a name="package-project-protection-level-dialog-box"></a><a name="protection_dialog"></a>“包项目保护级别”对话框
  可以使用 **“包保护级别”** 对话框更新包的保护级别。 保护级别决定保护包时所使用的方法、密码或用户密钥以及作用域。 可以保护所有数据，也可以只保护敏感数据。  
  
 若要了解包安全性的要求和选项，参阅[安全性概述 (Integration Services)](../../integration-services/security/security-overview-integration-services.md) 可能有所帮助。  
  
### <a name="options"></a>选项  
 **“包保护级别”**  
 从列表中选择保护级别。  
  
 **密码**  
 如果使用“使用密码加密敏感数据”或“使用密码加密所有数据”保护级别，请键入密码。  
  
 **重新键入密码**  
 再次键入该密码。  

## <a name="package-password-dialog-box"></a><a name="password_dialog"></a>“包密码”对话框
  可以使用 **“包密码”** 对话框为使用密码加密的包提供包密码。 如果包使用 **“使用密码加密敏感数据”** 或 **“使用密码加密所有数据”** 保护级别，则必须提供密码。  
  
### <a name="options"></a>选项  
 **密码**  
 输入密码。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 包](../../integration-services/integration-services-ssis-packages.md)   
 [安全性概述 (Integration Services)](../../integration-services/security/security-overview-integration-services.md)  
 [dtutil 实用工具](../../integration-services/dtutil-utility.md)  
