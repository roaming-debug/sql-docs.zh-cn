---
description: SSRS 加密密钥 - 备份和还原加密密钥
title: 备份和还原 Reporting Services 加密密钥 | Microsoft Docs
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- backing up encryption keys [Reporting Services]
- restoring encryption keys [Reporting Services]
- encryption keys [Reporting Services]
- symmetric keys [Reporting Services]
ms.assetid: 6773d5df-03ef-4781-beb7-9f6825bac979
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: eb35ae0143b2146d5d5d38ebf0f2ab6d51a94049
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439322"
---
# <a name="ssrs-encryption-keys---back-up-and-restore-encryption-keys"></a>SSRS 加密密钥 - 备份和还原加密密钥
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  报表服务器配置的一个重要部分是为用于加密敏感信息的对称密钥创建备份副本。 该密钥的备份副本对许多例程操作来说是必需的，通过使用备份副本，您可以在新的安装中重用现有报表服务器数据库。  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式 | [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
 在发生以下任何事件时，必须还原加密密钥的备份副本：  
  
-   更改报表服务器 Windows 服务帐户名或重置密码。 使用报表服务器配置管理器时，对密钥进行备份是服务帐户名称更改操作的一部分。  
  
    > [!NOTE]
    > 重置密码不同于更改密码。 密码重置需要相应的权限以覆盖域控制器上的帐户信息。 密码重置在您忘记或不知道特定密码时由系统管理员执行。 只有密码重置才需要还原对称密钥。 定期更改帐户密码不需要重置对称密钥。  
  
-   重命名承载报表服务器的计算机或实例（报表服务器实例基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名）。  
  
-   迁移报表服务器安装或配置报表服务器以使用其他报表服务器数据库。  
  
-   由于硬件故障而恢复报表服务器安装。  
  
 只需要备份对称密钥的一个副本。 报表服务器数据库和对称密钥之间存在一对一的对应关系。 尽管只需要备份一个副本，但如果在扩展部署模型中运行多个报表服务器，则可能需要多次还原密钥。 每个报表服务器实例都需要具有自己的对称密钥副本，才能对报表服务器数据库中的数据进行锁定和解锁。

 备份对称密钥是将密钥写入所指定的文件，然后使用所提供的密码对密钥进行加密的过程。 对称密钥绝对不能在未加密状态下存储，所以在将它保存到磁盘时，必须提供密码对密钥进行加密。 创建文件后，必须将其存储在安全的位置， **并记住文件的解锁密码** 。 若要备份对称密钥，可以使用以下工具：  
  
 **本机模式：** 报表服务器配置管理器或 rskeymgmt 实用工具。  

::: moniker range="=sql-server-2016"
  
 **SharePoint 模式：** SharePoint 管理中心页或 PowerShell。  
  
##  <a name="backup-sharepoint-mode-report-servers"></a><a name="bkmk_backup_sharepoint"></a> 备份 SharePoint 模式报表服务器  
 对于 SharePoint 模式报表服务器，您可以使用 PowerShell 命令或使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的管理页。 有关详细信息，请参阅 [管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)的“密钥管理”部分  

::: moniker-end
  
##  <a name="back-up-encryption-keys--report-server-configuration-manager-native-mode"></a><a name="bkmk_backup_configuration_manager"></a> 备份加密密钥 - 报表服务器配置管理器（本机模式）  
  
1.  启动报表服务器配置管理器，然后连接到要配置的报表服务器实例。  
  
2.  单击“加密密钥”，再选择“备份”。  
  
3.  键入强密码。  
  
4.  指定用来包含所存储密钥的文件。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将向该文件追加 .snk 文件扩展名。 请考虑将文件存储在独立于报表服务器的磁盘上。  
  
5.  选择“确定”  。  
  
###  <a name="back-up-encryption-keys--rskeymgmt-native-mode"></a><a name="bkmk_backup_rskeymgmt"></a> 备份加密密钥 - rskeymgmt（本机模式）  
  
1.  在承载报表服务器的计算机上本地运行 **rskeymgmt.exe** 。 必须使用 **-e** 提取参数来复制密钥、提供文件名并指定密码。 下面的示例演示必须指定的参数：  
  
    ```  
    rskeymgmt -e -f d:\rsdbkey.snk -p<password>  
    ```  
  
## <a name="restore-encryption-keys"></a>还原加密密钥  
 还原对称密钥将覆盖存储在报表服务器数据库中的现有对称密钥。 还原加密密钥将用以前保存到磁盘上的副本来替换无法使用的密钥。 还原加密密钥将导致以下操作：  
  
-   从密码保护的备份文件中打开对称密钥。  
  
-   使用报表服务器 Windows 服务的公钥加密对称密钥。  
  
-   将加密的对称密钥存储到报表服务器数据库中。  
  
-   删除以前存储的对称密钥数据（例如，在以前部署的报表服务器数据库中已存在的密钥信息）。  
  
 若要还原加密密钥，必须具有保存在文件中的加密密钥副本。 还必须知道用来对存储的副本进行解锁的密码。 如果您具有密钥和密码，则可以运行 Reporting Services 配置工具或 **rskeymgmt** 实用工具来还原密钥。 该对称密钥必须与用来对报表服务器数据库中当前存储的加密数据进行锁定和解锁的密钥相同。 如果还原的副本无效，则报表服务器将无法访问报表服务器数据库中当前存储的加密数据。 在这种情况下，如果无法恢复有效的密钥，则最好删除所有加密值。 如果由于某些原因（例如，如果没有备份副本）而无法还原加密密钥，则必须删除现有密钥和加密内容。 有关详细信息，请参阅[删除和重新创建加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)。 有关如何创建对称密钥的详细信息，请参阅[初始化报表服务器（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)。  
  
###  <a name="restore-encryption-keys--report-server-configuration-manager-native-mode"></a><a name="bkmk_restore_configuration_manager"></a>还原加密密钥 - 报表服务器配置管理器（本机模式）  
  
1.  启动报表服务器配置管理器，然后连接到要配置的报表服务器实例。  
  
2.  在“加密密钥”页上，选择“还原”。  
  
3.  选择包含备份副本的 .snk 文件。  
  
4.  键入用于解锁该文件的密码。  
  
5.  选择“确定”  。 
  
###  <a name="restore-encryption-keys---rskeymgmt-native-mode"></a><a name="bkmk_restore_rskeymgmt"></a> 还原加密密钥 - rskeymgmt（本机模式）  
  
1.  在承载报表服务器的计算机上本地运行 **rskeymgmt.exe** 。 使用 **-a** 参数还原密钥。 必须提供完全限定的文件名，并指定密码。 下面的示例演示必须指定的参数：  
  
    ```  
    rskeymgmt -a -f d:\rsdbkey.snk -p<password>  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [配置和管理加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
