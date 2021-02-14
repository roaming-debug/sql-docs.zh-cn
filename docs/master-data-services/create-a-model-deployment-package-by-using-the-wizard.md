---
description: 使用向导创建模型部署包
title: 使用向导创建模型部署包
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deployment packages [Master Data Services], creating
- models [Master Data Services], creating a deployment package
- creating packages [Master Data Services]
ms.assetid: b24ec4c2-1378-4c72-ac69-4ec2647030f0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: d0c122440543cc511d78b2c38b688fde672ec6ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272628"
---
# <a name="create-a-model-deployment-package-by-using-the-wizard"></a>使用向导创建模型部署包

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  使用 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 模型部署向导可创建只包含模型对象的包。 如果需要在包中包含数据，请参阅 [使用 MDSModelDeploy 创建模型部署包](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序中，您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   模型对于您要创建的包必须存在。 有关详细信息，请参阅[创建模型 (Master Data Services)](../master-data-services/create-a-model-master-data-services.md)。  
  
### <a name="to-create-a-model-deployment-package"></a>创建模型部署包  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在 **“模型视图”** 页上，从菜单栏中，指向 **“系统”** ，然后单击 **“部署”**。  
  
3.  在 **“模型部署向导”** 上，单击 **“创建”**。  
  
4.  在 **“创建包”** 页上，从 **“模型”** 列表中选择一个模型。  
  
5.  单击“下一步”  。  
  
6.  单击“下载”  。  
  
7.  保存文件。  
  
8.  单击“**关闭**”以关闭向导。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [使用向导部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)  
  
## <a name="see-also"></a>另请参阅  
 [部署模型 (Master Data Services)](../master-data-services/deploying-models-master-data-services.md)  
  
  
