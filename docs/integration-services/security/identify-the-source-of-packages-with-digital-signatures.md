---
description: 使用数字签名标识包的源
title: 使用数字签名标识包的源 | Microsoft Docs
ms.custom: security
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.digitalsigning.f1
helpviewer_keywords:
- signing packages [Integration Services]
- certificates [Integration Services]
- packages [Integration Services], security
- security [Integration Services], certificates
- signing policies [Integration Services]
ms.assetid: a433fbef-1853-4740-9d5e-8a32bc4ffbb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f4607ffe77d7fe93aff86d8dc707bb86e515d67d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340791"
---
# <a name="identify-the-source-of-packages-with-digital-signatures"></a>使用数字签名标识包的源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用数字证书对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 进行签名以标识其来源。 使用数字证书对包进行签名后，可以让 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 在加载包之前先检查数字签名。 若要让 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 能够检查签名，请在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 或 **dtexec** 实用工具 (dtexec.exe) 中设置一个选项，或设置一个可选的注册表值。  
  
## <a name="sign-a-package-with-a-digital-certificate"></a>使用数字证书对包进行签名  
 必须首先获取或创建数字证书，才能使用该证书对包进行签名。 获取证书后，即可使用该证书对包进行签名。 有关如何获取证书以及如何使用该证书对包进行签名的详细信息，请参阅 [使用数字证书对包签名](#cert)。  
  
## <a name="set-an-option-to-check-the-package-signature"></a>设置选项以检查包的签名  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 和 **dtexec** 实用工具都提供了用于将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 配置为检查签名包的数字签名的选项。 使用 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 还是 **dtexec** 实用工具取决于你是想检查所有包，还是只想检查特定的包：  
  
-   若要在设计时加载包之前检查所有包的数字签名，请在 **中设置** “加载包时检查数字签名” [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]选项。 此选项是针对 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中所有包的全局设置。
  
-   若要检查个别包的数字签名，请在使用 **dtexec** 实用工具运行该包时指定 **/VerifyS[igned]** 选项。 有关详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。  
  
## <a name="set-a-registry-value-to-check-package-signature"></a>设置注册表值以检查包的签名  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 还支持可选的注册表值 **BlockedSignatureStates**。你可以使用该值来管理组织用于加载签名包和未签名包的策略。 如果包未签名、签名无效或不可信，使用该注册表值将不允许加载该包。 有关如何设置此注册表值的详细信息，请参阅 [通过设置注册表值实现签名策略](#registry)。  
  
> **注意：** 可选的 **BlockedSignatureStates** 注册表值可指定比在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中或 **dtexec** 命令行中设置的数字签名选项限制性更强的设置。 在这种情况下，限制性更强的注册表设置将覆盖其他设置。  

## <a name="implement-a-signing-policy-by-setting-a-registry-value"></a><a name="registry"></a> 通过设置注册表值实现签名策略
  使用可选的注册表值可以管理组织用于加载签名包和未签名包的策略。 如果使用此注册表值，则必须在将运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包以及将强制实施该策略的每台计算机上创建此注册表值。 设置该注册表值后， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将在加载包之前检查或验证签名。  
  
 本主题中的此过程将介绍如何将可选的 **BlockedSignatureStates** DWORD 值添加到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS 注册表项中。 **BlockedSignatureStates** 中的数据值决定当包具有不可信签名、具有无效签名或未签名时是否阻止该包。 对于用来进行包签名的签名的状态， **BlockedSignatureStates** 注册表值使用下列定义：  
  
-   “有效签名”  是一个可以成功读取的签名。  
  
-   “无效签名”是一个解密的校验和（由私钥加密的包代码的单向哈希）与在加载 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的过程中计算的解密校验和不相符的签名。  
  
-   “可信签名”  是一个使用由受信任的根证书颁发机构签名的数字证书创建的签名。 该设置不要求签名者出现在可信发布服务器的用户列表中。  
  
-   “不可信签名  ”是一个不能验证为由受信任的根证书颁发机构颁发的签名，或不是最新版本的签名。  
  
 下表列出了 DWORD 数据的有效值及其相关策略。  
  
|值|说明|  
|-----------|-----------------|  
|0|无管理限制。|  
|1|阻止无效签名。<br /><br /> 该设置不阻止未签名的包。|  
|2|阻止无效签名和不可信签名。<br /><br /> 该设置不阻止未签名的包，但会阻止自我生成的签名。|  
|3|阻止无效签名、不可信签名以及未签名的包<br /><br /> 该设置也阻止自我生成的签名。|  
  
> [!NOTE]  
>  **BlockedSignatureStates** 的建议设置为 3。 该设置提供针对未签名包或者无效或不可信签名的最强保护功能。 不过，建议的设置并非适用于所有情况。 有关如何进行数字资产签名的详细信息，请参阅 MSDN Library 中的主题“[Introduction to Code Signing](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))（代码签名简介）”。  
  
### <a name="to-implement-a-signing-policy-for-packages"></a>实现包的签名策略  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** 。  
  
2.  在“运行”对话框中，键入 **Regedit**，然后单击“确定”。   
  
3.  找到注册表项 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS。  
  
4.  右键单击“MSDTS”，指向“新建”，然后单击“DWORD 值”。  
  
5.  将新值的名称更新为 **BlockedSignatureStates**。  
  
6.  右键单击“BlockedSignatureStates”，再单击“修改”。  
  
7.  在 **“编辑 DWORD 值”** 对话框中，键入值 0、1、2 或 3。  
  
8.  单击“确定”。   
  
9. 在 **“文件”** 菜单中，单击 **“退出”** 。    

## <a name="sign-a-package-by-using-a-digital-certificate"></a><a name="cert"></a> 使用数字证书对包签名
  本主题介绍如何使用数字证书对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包进行签名。 可以使用数字签名以及其他设置来防止加载和运行无效的包。  
  
 必须先执行下列任务才能对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包进行签名：  
  
-   创建或获取私钥以便与证书关联，并将此私钥存储在本地计算机上。  
  
-   以代码签名为目的从可信证书颁发机构获取证书。 可以使用下列方法之一来获取或创建证书：  
  
    -   从颁发证书的公共商业证书颁发机构获取证书。  
  
    -   从允许组织在内部颁发证书的证书服务器获取证书。 必须将用于对证书进行签名的根证书添加到 **“受信任的根证书颁发机构”** 存储区中。 若要添加根证书，可以使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台 (MMC) 的证书管理单元。 有关详细信息，请参阅 MSDN 库中的主题“[Certificate Services](/windows/win32/seccrypto/certificate-services)（证书服务）”。  
  
    -   创建自己的证书（仅用于测试目的）。 证书创建工具 (Makecert.exe) 会生成仅用于测试目的的 X.509 证书。 有关详细信息，请参阅 MSDN Library 中的主题“[证书创建工具 (Makecert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))”。  
  
     有关证书的详细信息，请参阅证书管理单元的联机帮助。 有关如何对数字资产进行签名的详细信息，请参阅 MSDN 库中的主题“[Signing and Checking Code with Authenticode](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537364(v=vs.85))（使用 Authenticode 签名和检查代码）”。  
  
-   确保已为代码签名启用证书。 若要确定证书是否是为代码签名而启用的，请在“证书”管理单元中检查证书的属性。  
  
-   将证书存储在个人存储区中。  
  
 完成上述任务后，即可使用下列过程对包进行签名。  
  
### <a name="to-sign-a-package"></a>签署包  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开要签名的包所在的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的 **“SSIS”** 菜单上，单击 **“数字签名”**。  
  
4.  在 **“数字签名”** 对话框中，单击 **“签名”**。  
  
5.  在 **“选择证书”** 对话框中，选择一个证书。  
  
6.  也可单击“查看证书”来查看证书信息。  
  
7.  单击 **“确定”** 关闭 **“选项证书”** 对话框。  
  
8.  单击 **“确定”** 关闭 **“数字签名”** 对话框。  
  
9. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
     虽然已对包进行了签名，您现在必须配置 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ，以便在加载该包之前检查或验证数字签名。  

## <a name="digital-signing-dialog-box-ui-reference"></a><a name="signing_dialog"></a>“数字签名”对话框 UI 参考
  使用 **“数字签名”** 对话框可以使用数字签名对包进行签名或删除签名。 在 **中，** SSIS **菜单的** “数字签名” **选项中提供了** “数字签名” [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]对话框。  
  
 有关详细信息，请参阅 [使用数字证书对包签名](#cert)。  
  
### <a name="options"></a>选项  
 **签名**  
 单击此项将打开“选择证书”对话框，在其中可选择要使用的证书。  
  
 **删除**  
 单击此项将删除数字签名。  

## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 包](../../integration-services/integration-services-ssis-packages.md)   
 [安全性概述 (Integration Services)](../../integration-services/security/security-overview-integration-services.md)  
  
