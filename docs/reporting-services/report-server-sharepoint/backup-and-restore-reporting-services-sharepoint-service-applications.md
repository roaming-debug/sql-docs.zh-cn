---
title: 备份和还原 Reporting Services SharePoint 服务应用程序 | Microsoft Docs
description: 了解如何使用 SharePoint 管理中心或 PowerShell 备份和还原 SQL Server Reporting Services 服务应用程序。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 559b88b776c92abfd98929f5b73cabb04a7f6d80
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474518"
---
# <a name="back-up-and-restore-reporting-services-sharepoint-service-applications"></a>备份和还原 Reporting Services SharePoint 服务应用程序

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

本主题说明如何使用 SharePoint 管理中心或 PowerShell 备份和还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="before-you-begin"></a>开始之前

### <a name="limitations-and-restrictions"></a>限制和局限

> [!NOTE]
>  通过使用 SharePoint 备份和还原功能，可以部分地备份和还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。 **需要执行其他步骤** ，本主题中介绍了这些步骤。 备份过程当前不备份无人参与的执行帐户 (UEA) 或对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据库的 Windows 身份验证的加密密钥和凭据。

### <a name="recommendations"></a>建议
  
-   启动 SharePoint 备份之前备份加密密钥。 如果你没有备份加密密钥，则在还原服务应用程序之后将无法访问加密的数据。 您将需要删除加密数据。  
  
-   验证您的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序是正在使用 UEA 还是 Windows 身份验证来访问数据库。 如果正在使用其中一个，请验证正确的凭据，以便在还原过程之后可以正确地配置该服务应用程序。  
  
-   查看在与备份文件相同的文件夹中创建 SharePoint 备份日志。 该文件通常名为 **spbackup.log**  
  
## <a name="back-up-the-service-application"></a>备份服务应用程序

 请按顺序完成下列步骤：  
  
1.  备份加密密钥  
  
2.  备份服务应用程序  
  
3.  验证您的服务应用程序是使用 UEA 还是 Windows 身份验证来访问数据库。 如果确实使用这两种方法，请记下凭据，以便在还原服务应用程序之后，可以使用这些凭据配置服务应用程序。  

### <a name="back-up-the-encryption-keys-using-sharepoint-central-administration"></a>使用 SharePoint 管理中心备份加密密钥

有关备份 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加密密钥的信息，请参阅[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)的“加密密钥”部分。  

### <a name="back-up-the-service-application-using-sharepoint-central-administration"></a>使用 SharePoint 管理中心备份服务应用程序

若要备份服务应用程序，请完成以下步骤：  
  
1.  在 SharePoint 管理中心中，选择“备份和还原”组中的“执行备份”。  
  
2.  在 **“共享服务”** 节点下，展开 **“共享服务应用程序”** ，然后选择您的服务应用程序。 该应用程序将具有 **“SQL Server Reporting Services 服务应用程序”** 类型。  
  
3.  选择“**下一页**”。  
  
4.  键入“备份位置:”的路径，然后选择“开始备份”  
  
5.  重复上述过程，而不是选择服务应用程序，展开 **“共享服务代理”** 节点，然后选择服务应用程序代理。 该应用程序将具有 **“SQL Server Reporting Services 服务应用程序代理”** 类型。  
  
 有关详细信息，请参阅 SharePoint 文档中的以下主题：  
  
 [SharePoint 文档中的备份服务应用程序 (SharePoint Foundation 2010)](/previous-versions/office/sharepoint-foundation-2010/ee748601(v=office.14))。  
  
 [备份服务应用程序 (SharePoint Server 2010)](/SharePoint/administration/back-up-a-service-application)  
  
### <a name="verify-execution-account-and-database-authentication"></a>验证执行帐户和数据库身份验证

 **执行帐户**：验证你的服务应用程序是否正在使用执行帐户：  
  
1.  在 SharePoint 管理中心的“应用程序管理”组中，选择“管理服务应用程序”。  
  
2.  选择你的服务应用程序的名称，然后选择 SharePoint 功能区中的“管理”。  
  
3.  选择“执行帐户”。  
  
4.  如果配置了执行帐户，则当需要还原服务应用程序备份时，您需要知道凭据。 在知道正确的凭据之前，请不要继续执行备份和还原过程。  
  
 **数据库身份验证**：验证你的服务应用程序是否正在使用 Windows 身份验证来进行数据库身份验证：  
  
1.  在 SharePoint 管理中心的“应用程序管理”组中，选择“管理服务应用程序”。  
  
2.  选择你的服务应用程序的名称，然后选择 SharePoint 功能区中的“属性”。  
  
3.  查看“Reporting Services (SSRS) 服务数据库”部分。  
  
4.  如果配置了 Windows 身份验证，则您需要知道凭据，以便您可以在还原服务应用程序后对其进行配置。 在知道正确的凭据之前，请不要继续执行备份和还原过程。  
  
## <a name="restore-the-service-application"></a>还原服务应用程序

 请按顺序完成下列步骤：  
  
1.  还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。  
  
2.  还原加密密钥  
  
3.  如果您的服务应用程序正在使用执行帐户或 Windows 身份验证来访问数据库，请配置凭据。  
  
### <a name="restore-the-service-application-using-sharepoint-central-administration"></a>使用 SharePoint 管理中心还原服务应用程序
  
1.  在 SharePoint 管理中心中，选择“备份和还原”组中的“从备份中还原”。  
  
2.  在“备份目录位置”框中键入指向备份文件的路径，然后选择“刷新”。  
  
3.  从“顶部组件”列表中选择服务应用程序备份，然后选择“下一步”。  
  
4.  选择你的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序，然后选择“下一步”。  
  
5.  在 **“登录名和密码”** 部分中，键入登录名的密码。 登录名框中应填充服务应用程序在备份之前使用的登录名。  
  
6.  选择“开始还原”。  
  
7.  重复上述过程，而不是还原服务应用程序，展开 **“共享服务”** 节点，然后展开 **“共享服务应用程序”** 节点。  
  
 有关详细信息，请参阅 SharePoint 文档中的以下主题：  
  
 [还原服务应用程序 (SharePoint Foundation 2010)](/previous-versions/office/sharepoint-foundation-2010/ee748615(v=office.14))。  
  
 [还原服务应用程序 (SharePoint Server 2010)](/SharePoint/administration/restore-a-service-application)。  

### <a name="restore-the-encryption-keys-using-sharepoint-central-administration"></a>使用 SharePoint 管理中心还原加密密钥

 有关还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加密密钥的信息，请参阅[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)的“加密密钥”部分。  

### <a name="configure-the-execution-account-and-database-authentication"></a>配置执行帐户和数据库身份验证

 **执行帐户**：如果你的服务应用程序正在使用执行账户，请完成下列步骤对它进行配置：  
  
1.  在 SharePoint 管理中心的“应用程序管理”组中，选择“管理服务应用程序”。  
  
2.  选择你的服务应用程序的名称，然后选择 SharePoint 功能区中的“管理”。  
  
3.  选择“执行帐户”。  
  
4.  键入帐户和密码，然后选中 **“指定执行帐户”** 框。  
  
5.  选择“确定”。  
  
 **数据库身份验证**：如果你的服务应用程序正在使用 Windows 身份验证来进行数据库身份验证，请完成以下步骤：  
  
1.  在 SharePoint 管理中心的“应用程序管理”组中，选择“管理服务应用程序”。  
  
2.  选择你的服务应用程序的名称，然后选择 SharePoint 功能区中的“属性”。  
  
3.  查看“Reporting Services (SSRS) 服务数据库”部分。  
  
4.  选择 **“Windows 身份验证”** 。  
  
5.  键入帐户和密码。 如果适当，请选择 **“用作 Windows 凭据”** 。  
  
6.  选择“确定”

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)