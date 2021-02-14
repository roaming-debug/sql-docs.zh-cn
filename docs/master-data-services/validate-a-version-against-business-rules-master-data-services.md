---
description: 针对业务规则验证版本 (Master Data Services)
title: 针对业务规则验证版本
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- validating versions [Master Data Services]
- validating versions [Master Data Services], about validating versions
- versions [Master Data Services], validating
- business rules [Master Data Services], applying to all members
ms.assetid: 5aee7901-6d05-41d4-8bbb-c6f26791d1df
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9a5ad62b83d7a12129da352ef7265996dd770297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272378"
---
# <a name="validate-a-version-against-business-rules-master-data-services"></a>针对业务规则验证版本 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，对某一版本进行验证以便将业务规则应用于该模型版本中的所有成员。  
  
 此过程说明如何使用 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序来验证数据。 如果您在 MDS 数据库中具有权限，可以改用存储过程。 有关详细信息，请参阅 [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)。  
  
> [!NOTE]  
>  所有成员必须通过验证后，才能提交版本。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“版本管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   版本的状态必须是 **“打开”** 或 **“已锁定”**。  
  
-   在 **“验证版本”** 页上，存在的成员必须具有并非 **“验证已成功”** 的其他状态。  
  
### <a name="to-validate-a-version"></a>验证版本  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“版本管理”**。  
  
2.  在 **“管理版本”** 页上，从菜单栏中，单击 **“验证版本”**。  
  
3.  在 **“验证版本”** 页上，选择要验证的模型和版本。  
  
4.  单击 **“验证”** 。  
  
5.  在确认对话框中，单击 **“确定”**。  
  
    > [!NOTE]  
    >  一旦不再显示进度指示器，则表明该版本已完成验证。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [验证状态 &#40;Master Data Services&#41;](../master-data-services/validation-statuses-master-data-services.md)   
 [验证存储过程 &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md)   
 [版本 &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)   
 [业务规则 &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
  
