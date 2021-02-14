---
title: 版本
description: 您可以在 Master Data Services 的模型中创建主数据的多个版本。 了解版本以及何时使用它们。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- version flags [Master Data Services], about version flags
- versions [Master Data Services]
- version flags [Master Data Services]
- versions [Master Data Services], version flags
ms.assetid: 752ec96d-53d7-4160-8ed2-92e0324645f3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 54c250aa3b6a6e803f90522218b1eef854af7903
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348846"
---
# <a name="versions-master-data-services"></a>版本 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，您可以创建模型中主数据的多个版本。 在验证数据时可以锁定版本，并在验证数据之后提交。 提交的版本组成可审核的更改记录。 创建的每个版本包含模型的所有成员、属性值、层次结构成员、层次结构关系和集合。  
  
## <a name="when-to-use-versions"></a>何时使用版本  
 使用版本可以：  
  
-   维护主数据随时间变化的可审核记录。  
  
-   防止用户更改，确保所有数据已根据业务规则成功验证。  
  
-   锁定模型供订阅系统使用。  
  
-   测试不同的层次结构，而不必立即将其实现。  
  
> [!NOTE]  
>  更改模型的结构时，例如创建新实体或基于域的属性时，更改应用到所有版本。 如果您查看模型的早期版本，将显示实体或属性，但数据不存在。  
  
## <a name="version-flags"></a>版本标志  
 版本可用于用户或订阅系统时，可以设置一个标志来标识该版本。 可以根据需要在版本间移动此标志。 标志帮助用户和订阅系统确定要使用模型的哪个版本。  
  
## <a name="workflow-for-version-management"></a>版本管理的工作流  
 使用以下工作流进行版本管理：  
  
1.  创建模型并使用公司的主数据填充 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库时，自动创建初始版本。 用户基于权限在需要时可以更改此版本。  
  
2.  当您要提交模型的一个版本时，锁定该版本，以便只有模型管理员可以更新数据。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。 如果配置了通知，则每次版本的状态发生更改时，电子邮件通知都会发送给模型管理员。 有关详细信息，请参阅[配置电子邮件通知 (Master Data Services)](../master-data-services/configure-email-notifications-master-data-services.md)。  
  
3.  将业务规则应用于锁定的版本的数据并查看任何验证问题。 如有必要，可以填写缺少的信息或恢复导致问题的事务。 还可以解锁该版本，以便用户进行更改。  
  
4.  在所有数据通过验证后，提交该版本并将其标记为可供订阅系统使用。 无法更改已提交的版本。  
  
5.  复制已提交的版本，并通知用户他们可以开始使用模型的新版本。  
  
## <a name="sequential-or-simultaneous-versions"></a>顺序版本或同时版本  
 可以创建模型的顺序版本或同时版本。  
  
-   **顺序版本：** 每次提交版本时，可以创建新的副本并为版本提供下一个序列号。 例如，可以复制 **“版本 7”** 的模型，并将副本命名为 **“版本 8”**。  
  
-   **同时版本：** 要同时使用数据的两个或多个版本时，可以创建模型的同时版本。 如果您的公司存在与正常业务流程相符的重组或合并行为，并且您要确定如何使新的主数据适应现有结构，同时版本将非常有用。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中的设置确定是复制所有版本还是仅复制那些已提交的版本。 若要创建同时版本，必须配置 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 以允许您复制所有版本。 此设置在“系统设置”表中也提供。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|更改现有版本的名称。|[更改版本名称 (Master Data Services)](../master-data-services/change-a-version-name-master-data-services.md)|  
|锁定版本，以便只有管理员才能编辑其数据。|[锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)|  
|取消锁定版本，以便用户可以编辑其数据。|[取消锁定版本 (Master Data Services)](../master-data-services/unlock-a-version-master-data-services.md)|  
|验证所有数据后，提交版本。|[提交版本 (Master Data Services)](../master-data-services/commit-a-version-master-data-services.md)|  
|创建新的标志来标记版本。|[创建版本标志 (Master Data Services)](../master-data-services/create-a-version-flag-master-data-services.md)|  
|更改现有版本标志的名称。|[更改版本标志名称 (Master Data Services)](../master-data-services/change-a-version-flag-name-master-data-services.md)|  
|将现有标志分配给版本。|[向版本分配标志 (Master Data Services)](../master-data-services/assign-a-flag-to-a-version-master-data-services.md)|  
|创建现有版本的新副本|[复制版本 (Master Data Services)](../master-data-services/copy-a-version-master-data-services.md)|  
|删除现有版本。|[删除版本 (Master Data Services)](../master-data-services/delete-a-version-master-data-services.md)|  
|从版本中清除软删除的成员|[清除版本成员 (Master Data Services)](../master-data-services/purge-version-members-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [撤消事务 (Master Data Services)](../master-data-services/reverse-a-transaction-master-data-services.md)  
  
-   [通知 (Master Data Services)](../master-data-services/notifications-master-data-services.md)  
  
-   [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  
