---
title: 配置报表生成器访问权限 | Microsoft Docs
description: 配置报表生成器，一种与 SQL Server Reporting Services 报表服务器一起使用的报表设计工具。 它使用本机或 SharePoint 集成模式。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 03/07/2021
ms.openlocfilehash: 6940a6c5b46861ebb745fb3965167a4d19173446
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833844"
---
# <a name="configure-report-builder-access"></a>配置报表生成器访问权限

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Microsoft 报表生成器是一个特别报告生成工具，该工具可与为本机模式或 SharePoint 集成模式配置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器一起使用。  

报表生成器的访问权限取决于以下因素：  

- 角色分配或权限，它们使报表生成器可用于单个用户或组。  

- 身份验证设置，它们决定用户凭据是否可以传递到报表服务器。

## <a name="prerequisites"></a>先决条件

客户端计算机必须已安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 或更高版本。

## <a name="enabling-and-disabling-report-builder"></a>启用和禁用报表生成器  

默认情况下，通过门户下载报表生成器功能已启用。 报表服务器管理员可以视需要将报表服务器系统属性 ShowDownloadMenu 设置为 false，以禁用报表生成器下载功能。 设置此属性将会禁用该报表服务器的报表生成器、移动报表发布服务器、Power BI Desktop 和 Power BI 移动版下载功能。  

 若要设置报表服务器系统属性，可以使用 Management Studio 或脚本：   

 - 若要使用 Management Studio，请连接到报表服务器，并使用“高级服务器属性”页将 ShowDownloadMenu 设置为 false。 有关如何打开该页面的详细信息，请参阅[设置报表服务器属性 (Management Studio)](../../reporting-services/tools/set-report-server-properties-management-studio.md)。      

 - 若要查看设置报表服务器属性的示例脚本，请参阅 [为部署和管理任务编写脚本](../../reporting-services/tools/script-deployment-and-administrative-tasks.md)。  

## <a name="role-assignments-granting-report-builder-access-on-a-native-mode-report-server"></a>本机模式报表服务器上用于授予报表生成器访问权限的角色分配  

在本机模式报表服务器上，创建包括使用报表生成器的任务的用户角色分配。 您必须是内容管理员和系统管理员，才能在项级和站点级创建或修改角色定义和角色分配。  

以下说明假定您使用的是预定义角色。 如果修改了角色定义，请检查角色，验证它们是否包含所需的任务。 若要详细了解如何创建角色分配，请参阅[向用户授予对报表服务器的访问权限](../../reporting-services/security/grant-user-access-to-a-report-server.md)。

创建角色分配之后，用户将拥有执行以下操作的权限：  

- 分配给系统用户角色和浏览者角色的用户可以在报表服务器上查看已发布的报表生成器报表，而不必启动报表生成器。  

- 分配给系统用户角色和报表生成者角色的用户可以生成模型，启动报表生成器和创建报表，以及将报表保存到报表服务器。  

- 分配给系统用户角色和发布者角色的用户可以将模型从模型设计器发布到报表服务器。 模型用作报表生成器中的数据源。  

- 分配给系统管理员角色和内容管理员角色的用户拥有创建、查看和管理报表生成器报表的完全权限。  

### <a name="to-verify-required-tasks-are-in-the-role-definitions"></a>验证必需的任务是否在角色定义中  

1. 启动 Management Studio 并连接到报表服务器。  

2. 打开 **“安全性”** 文件夹。  

3. 打开 **“系统角色”** 文件夹。  

4. 右键单击“系统管理员”，然后选择“属性” 。  

5. 选择 **“执行报表定义”** ，然后单击 **“确定”** 。  

6. 右键单击“系统用户”，然后选择“属性” 。  

7. 选择 **“执行报表定义”** ，然后单击 **“确定”** 。  

8. 打开 **“角色”** 文件夹。  

9. 右键单击“浏览器”，然后选择“属性” 。  

10. 选择 **“查看模型”** ，然后单击 **“确定”** 。  

11. 右键单击“内容管理器”，然后选择“属性” 。  

12. 依次选择 **“查看模型”** ， **“管理模型”** ， **“使用报表”** ，然后单击 **“确定”** 。  

13. 右键单击“发布服务器”，然后选择“属性” 。  

14. 选择 **“管理模型”** ，然后单击 **“确定”** 。  

15. 如果报表生成者角色不存在，则创建该角色：  

    1. 打开 **“安全性”** 文件夹。  

    2. 右键单击“角色”选择“新建角色” 。  

    3. 在“名称”中，键入 **报表生成者**。  

    4. 在“说明”中，输入角色说明，以便 Web 门户中的用户知道角色的用途。  

    5. 添加下列任务：“使用报表”、“查看报表”、“查看模型”、“查看资源”、“查看文件夹”和“管理单独的订阅”。  

    6. 单击 **“确定”** 保存角色。  

#### <a name="to-create-role-assignments-that-grant-access-to-report-builder"></a>创建授予对报表生成器的访问权的角色分配  

1. 启动 Web 门户。  

2. 单击 Web 门户主页右上方的齿轮图标，并从下拉菜单中选择“网站设置”。  
![Web 门户齿轮图标和菜单](../../reporting-services/report-builder/media/configure-report-builder-access/ssrswebportal-site-settings-gear-icon-and-menu.png)

3. 单击 **“安全性”** 。  

4. 如果要为其配置报表生成器访问权的用户或组已经分配有角色，则单击 **“编辑”** 。  
否则，单击 **“新建角色分配”** 。 在组或用户中，按以下格式输入 Windows 域用户或组帐户：\<domain>\\<account\>。 如果使用窗体身份验证或自定义安全性，则以适用于您的部署的格式指定该用户帐户或组帐户。  

5. 选择 **“系统用户”** ，然后单击 **“确定”** 。  

6. 单击 **“主页”** 。  

7. 单击 **“文件夹设置”** 选项卡。  

8. 单击“安全”选项卡。  

9. 如果要为其配置报表生成器访问权的用户或组已经分配有角色，则单击 **“编辑”** 。  

    否则，单击 **“新建角色分配”** 。 在组或用户中，按以下格式输入 Windows 域用户或组帐户：\<domain>\\<account\>。 如果使用窗体身份验证或自定义安全性，则以适用于您的部署的格式指定该用户帐户或组帐户。  

10. 选择 **“报表生成者”** ，然后单击 **“应用”** 。  

11. 重复这些步骤以便为其他用户或组创建或修改角色分配。  

## <a name="permissions-granting-report-builder-access-on-a-sharepoint-integrated-mode-report-server"></a>SharePoint 集成模式报表服务器上用于授予报表生成器访问权限的权限  

在 SharePoint 集成模式报表服务器上，将报表生成器访问权授予具有“参与讨论”或“完全控制”权限级别的 SharePoint 用户。  

如果使用自定义权限级别，则必须在权限级别包括“添加项”和“编辑项”。 有关通过内置权限级别访问报表生成器的详细信息，请参阅 [将 Windows SharePoint Services 中的内置安全性用于报表服务器项](../../reporting-services/security/use-built-in-security-in-windows-sharepoint-services-for-report-server-items.md)。 有关自定义权限级别的权限要求的详细信息，请参阅 [在 SharePoint Web 应用程序中设置报表服务器操作的权限](../../reporting-services/security/set-permissions-for-report-server-operations-in-a-sharepoint-web-application.md)。  

## <a name="authentication-considerations-and-credential-reuse"></a>身份验证注意事项和凭据重用  

- 报表生成器将打开其自己的与报表服务器的连接。 如果您未将 Windows 集成安全性用于单一登录，则用户必须为报表生成器与报表服务器的连接重新键入其凭据。  

## <a name="see-also"></a>另请参阅  

- [针对报表服务器的身份验证](../../reporting-services/security/authentication-with-the-report-server.md)
- [Reporting Services 和 Power View 的浏览器支持](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
- [启动报表生成器](../../reporting-services/report-builder/start-report-builder.md)
- [报表服务器的 Web 门户（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)
- [在 Management Studio 中连接到报表服务器](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)
- [报表服务器系统属性](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md)
