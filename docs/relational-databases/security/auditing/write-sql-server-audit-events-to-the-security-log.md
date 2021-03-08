---
title: 将 SQL Server 审核事件写入安全日志 | Microsoft Docs
description: 了解如何将 SQL Server 审核事件写入 Windows 安全日志。 了解使用该日志的限制和局限。
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], Security Log
- server audit [SQL Server]
- audits [SQL Server], writing to Security Log
- security logs [SQL Server]
ms.assetid: 6fabeea3-7a42-4769-a0f3-7e04daada314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abdd25c6166b40436584a1260d4b1b09d40900c4
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186047"
---
# <a name="write-sql-server-audit-events-to-the-security-log"></a>将 SQL Server 审核事件写入安全日志  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

在高度安全环境中，Windows 安全日志是写入记录对象访问的事件的合适位置。 其他审核位置也受支持，但是更易被篡改。  
  
 将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务器审核写入 Windows 安全日志有三个关键要求：  
  
-   必须配置审核对象访问设置以捕获事件。 审核策略工具 (`auditpol.exe`) 公开了 **审核对象访问** 类别中的多种子策略设置。 若要允许 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核对象访问，请配置 **应用程序生成的** 设置。  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务正在其下运行的帐户必须拥有 **生成安全审核** 权限才能写入 Windows 安全日志。 默认情况下，LOCAL SERVICE 和 NETWORK SERVICE 帐户拥有此权限。 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 正在其中一个帐户下运行，则不需要此步骤。  
-   为[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]服务帐户访问注册表配置单元`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security`提供完整权限。  

  > [!IMPORTANT]  
  > [!INCLUDE[ssnoteregistry-md](../../../includes/ssnoteregistry-md.md)]   
  
将 Windows 审核策略配置为写入 Windows 安全日志时，可能会影响 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核，此时若该审核策略配置不正确，就可能导致事件丢失。 通常，将 Windows 安全日志设置为覆盖较旧的事件。 这样可保留最新的事件。 但如果 Windows 安全日志未设置为覆盖较旧的事件，则当安全日志已满时，系统将发出 Windows 事件 1104（日志已满）。 此时：  
-   不再记录其他安全事件  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将无法检测系统是否能够在安全日志中记录事件，从而导致可能丢失审核事件  
-   Box 管理员修复安全日志后，日志记录行为将恢复正常。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 计算机的管理员应了解安全日志的本地设置可能会被域策略覆盖。 在这种情况下，域策略可能会覆盖子类别设置 (**auditpol /get /subcategory:"application generated"** )。 这可能会影响 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在无法检测 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 尝试审核的事件是否将不被记录的情况下记录事件的能力。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 您必须是 Windows 管理员，才能配置这些设置。  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-auditpol"></a><a name="auditpolAccess"></a> 在 Windows 中使用 auditpol 配置审核对象访问设置  
  
1.  使用管理权限打开命令提示符。  
  
    1.  在“开始”菜单中，依次指向“所有程序”、“附件”，右键单击“命令提示符”，然后单击“以管理员身份运行”。  
  
    2.  在 **“用户帐户控制”** 对话框打开时，单击 **“继续”** 。  
  
2.  执行以下语句以从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]启用审核。  
  
    ```  
    auditpol /set /subcategory:"application generated" /success:enable /failure:enable  
    ```  
  
3.  关闭命令提示符窗口。  
  
##  <a name="to-grant-the-generate-security-audits-permission-to-an-account-using-secpol"></a><a name="secpolAccess"></a> 使用 secpol 将生成安全审核权限授予帐户  
  
1.  对于任何 Windows 操作系统，在 **“开始”** 菜单上单击 **“运行”** 。  
  
2.  键入 **secpol.msc** ，然后单击 **“确定”** 。 在显示 **“用户访问控制”** 对话框时，单击 **“继续”** 。  
  
3.  在“本地安全策略”工具中，依次展开 **“安全设置”** 、 **“本地策略”** ，然后单击 **“用户权限分配”** 。  
  
4.  在结果窗格中，双击“生成安全审核”。  
  
5.  在 **“本地安全设置”** 选项卡上，单击 **“添加用户或组”** 。  
  
6.  在“选择用户、计算机或组”对话框中，键入用户帐户的名称，例如 **domain1\user1**，然后单击“确定”，或单击“高级”并搜索帐户。  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
8.  关闭安全策略工具。  
  
9. 重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 以便启用此设置。  
  
##  <a name="to-configure-the-audit-object-access-setting-in-windows-using-secpol"></a><a name="secpolPermission"></a> 在 Windows 中使用 secpol 配置审核对象访问设置  
  
1.  如果操作系统的版本早于 [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] 或 Windows Server 2008，则在 **“开始”** 菜单上单击 **“运行”** 。  
  
2.  键入 **secpol.msc** ，然后单击 **“确定”** 。 在显示 **“用户访问控制”** 对话框时，单击 **“继续”** 。  
  
3.  在“本地安全策略”工具中，依次展开 **“安全设置”** 、 **“本地策略”** ，然后单击 **“审核策略”** 。  
  
4.  在结果窗格中，双击“审核对象访问”。  
  
5.  在 **“本地安全设置”** 选项卡上的 **“审核这些操作”** 区域中，选择 **“成功”** 和 **“失败”** 。  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  关闭安全策略工具。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Audit（数据库引擎）](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
