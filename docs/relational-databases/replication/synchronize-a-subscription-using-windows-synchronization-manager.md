---
title: 同步订阅（Windows 同步管理器）
description: 了解如何使用 Windows 同步管理器同步订阅。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], Windows Synchronization Manager
- Windows Synchronization Manager
ms.assetid: 80f15dd6-e84d-4f96-9866-5b34ea531f1e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3b29f98ad39d34935d65530841f8e81063233791
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468718"
---
# <a name="synchronize-a-subscription-using-windows-synchronization-manager"></a>使用 Windows 同步管理器同步订阅
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  如果[!INCLUDE[msCoName](../../includes/msconame-md.md)] 与同步管理器在同一台计算机上运行，那么只能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 同步管理器同步对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布的订阅（也可用来同步脱机文件和网页）。 若要使用同步管理器：  
  
1.  在“订阅属性 - \<Subscriber>: \<SubscriptionDatabase>”对话框中，启用使用 Windows 同步管理器对请求订阅进行同步的选项。 有关访问此对话框的详细信息，请参阅[查看和修改请求订阅属性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)。  
  
2.  通过 Windows 的 **“开始”** 菜单访问同步管理器。  
  
 同步管理器允许对合并订阅使用交互式冲突解决程序。 通常，同步过程中检测到的冲突都能够自动解决，但如果启用了交互式解决，则可由用户在同步过程中解决。 如果在 Windows 同步管理器的外部执行同步（作为 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或复制监视器中的计划同步或按需同步），系统会根据为项目指定的冲突解决程序自动解决冲突，无需用户干预。  
  
> [!NOTE]  
>  从 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)]开始，64 位版本的 Windows 同步管理器无法检测 32 位的订阅。  
  
### <a name="to-enable-the-synchronization-of-pull-subscriptions-with-windows-synchronization-manager"></a>启用使用 Windows 同步管理器对请求订阅进行同步的选项  
  
1.  在“订阅属性 - \<Subscriber>: \<SubscriptionDatabase>”对话框的“常规”页上，为“使用 Windows 同步管理器”选择值“启用”。     
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-synchronize-a-pull-subscription-with-synchronization-manager"></a>使用同步管理器同步请求订阅  
  
1.  使用下列方法之一启动同步管理器：  
  
    -   在 Internet Explorer 中，单击 **“工具”** ，再单击 **“同步”** 。  
  
    -   单击 **“开始”** ，指向 **“程序”** 或 **“所有程序”** ，然后指向 **“附件”** ，再单击 **“同步”** 。  
  
    -   单击 **“开始”** ，再单击 **“运行”** 在“运行”对话框的“打开”字段中键入 **mobsync.exe**，然后单击“确定”。  
  
2.  在 **“要同步的项”** 对话框中，选择要同步的订阅。 将在计算机上安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例下列出订阅。  
  
3.  单击 **“同步”** 。  
  
### <a name="to-reinitialize-a-pull-subscription-with-synchronization-manager"></a>使用同步管理器重新初始化请求订阅  
  
1.  在 **“要同步的项”** 对话框中，选择订阅，然后单击 **“属性”** 。  
  
2.  在 **“SQL Server 订阅属性”** 对话框中，单击 **“重新初始化订阅”** 。  
  
3.  单击 **“是”** 。  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     下次同步订阅时，默认情况下会将一个新快照应用于订阅数据库。 有关详细信息，请参阅 [重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
> [!NOTE]  
>  合并复制允许在应用快照之前将所有未完成的更改上载到发布服务器，但同步管理器没有提供此选项。 若要上载更改，请先同步，然后再重新初始化订阅。  
  
### <a name="to-set-properties-for-a-pull-subscription-in-synchronization-manager"></a>在同步管理器中设置请求订阅的属性  
  
1.  在 **“要同步的项”** 对话框中，选择订阅，然后单击 **“属性”** 。  
  
2.  在下列选项卡上查看和修改属性：  
  
    -   **标识**  
  
    -   **“订阅服务器登录”** 、 **“分发服务器登录”** 和 **“发布服务器登录”** （仅适用于合并复制）  
  
    -   **“Web 服务器信息”** （适用于运行 SQL Server 2005 或更高版本的订阅服务器中的合并订阅）  
  
    -   **其他**  
  
     建议对所有连接都使用 Windows 身份验证。 有关分发代理和合并代理所需权限的信息，请参阅 [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-remove-a-pull-subscription-from-synchronization-manager"></a>从同步管理器中删除请求订阅  
  
1.  在 **“要同步的项”** 对话框中，选择订阅，然后单击 **“属性”** 。  
  
2.  在 **“SQL Server 订阅属性”** 对话框中，单击 **“删除订阅”** 。  
  
3.  从 **“删除订阅”** 对话框中选择一个选项。  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-use-the-interactive-resolver"></a>使用交互式冲突解决程序  
  
1.  启用要使用交互式解决方法的项目和订阅。 有关详细信息，请参阅[指定合并复制属性](../../relational-databases/replication/merge/specify-merge-replication-properties.md)。  
  
2.  如果启用了交互式冲突解决，并且一个和多个项目存在冲突，那么在同步管理器开始同步订阅后，交互式冲突解决程序就会自动启动。 交互式冲突解决程序每次显示一个冲突，并为每个冲突提出建议的解决方法（基于创建发布和订阅时指定的冲突解决程序）。  
  
3.  根据需要编辑交互式冲突解决程序中显示的任何列，然后单击下列按钮之一以解决冲突：  
  
    -   **接受建议**  
  
    -   **接受发布服务器**  
  
    -   **接受订阅服务器**  
  
    -   **“自动解决所有冲突”** （无须进一步输入即可解决当前的所有冲突）  
  
     选定的行然后应用于发布服务器和/或订阅服务器，并在后续同步过程中传播到拓扑中的其他节点。  
  
> [!NOTE]  
>  所做的编辑只有在属于为解决冲突而选择的行时才生效。 例如，如果在 **“发布服务器”** 下进行编辑，然后单击 **“接受订阅服务器”** ，那么所做的编辑将被忽略。  
  
## <a name="see-also"></a>另请参阅  
 [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)  
  
  
