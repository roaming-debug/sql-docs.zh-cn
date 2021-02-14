---
title: " (MDSModelDeploy 创建模型部署包) "
description: 使用 MDSModelDeploy 在 Master Data Services 中创建部署包。 包可以只包含模型对象，也可以包含模型对象和数据。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: c2687e39-dc20-494f-a707-2aa29f4c329e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ef35b92932cd4e55b47e2f75a16825f14202a815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272638"
---
# <a name="create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>使用 MDSModelDeploy 创建模型部署包

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，使用 MDSModelDeploy 工具来创建包。 根据您指定的命令，包可以包含：  
  
-   仅模型对象。  
  
-   模型对象和数据。  
  
 如果需要部署仅包含模型对象的包，可改为在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序中使用模型部署向导。 有关详细信息，请参阅 [使用向导创建模型部署包](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md)。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
1.  运行 MDSModelDeploy 工具所需的基本权限如下所示：  
  
    -   与 MDS 配置管理器相同的 Windows 权限（Windows 管理员）  
  
    -   针对 MDS 数据库的 DBA 权限。  
  
2.  使用 MDSModelDeploy 工具创建包所需的权限如下所示：  
  
    -   针对数据模型的 MDS 模型管理员权限。  
  
    -   MDS ImportExport 功能区权限。  
  
3.  使用 MDSModelDeploy 工具部署模型所需的权限如下所示：  
  
    -   MDS 资源管理器功能权限  
  
    -   MDS 系统管理功能权限。  
  
4.  使用 MDSModelDeploy 工具列出模型所需的权限如下所示：  
  
    -   MDS 资源管理器功能权限  
  
    -   查看列表的模型所需的针对数据模型的 MDS 模型管理员权限。  
  
 模型对于您要创建的包必须存在。 有关详细信息，请参阅[创建模型 (Master Data Services)](../master-data-services/create-a-model-master-data-services.md)。  
  
 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>使用 MDSModelDeploy 创建模型部署包  
  
1.  打开一个管理员命令提示符。  
  
2.  导航到 MDSModelDeploy.exe 所在的位置。  
  
    -   如果 MDS 安装在默认位置，则该文件位于 *驱动器盘符*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration。  
  
    -   如果 MDS 未安装在默认位置，请在本地计算机上搜索 MDSModelDeploy.exe。  
  
3.  可选。 查看选项和帮助。  
  
    -   若要显示所有可用选项，请键入 `MDSModelDeploy` ，然后按 Enter 键。  
  
    -   若要显示某个选项的帮助，请键入以下命令，其中 OptionName 是该选项的名称：`MDSModelDeploy help OptionName`。  
  
4.  可选。 如果您有多个 Web 应用程序，通过键入下面的命令并按 Enter 键，确定您要部署到的服务的名称：  
  
    ```  
    MDSModelDeploy listservices  
    ```  
  
     随即返回一个值列表，例如 `MDS1, Default Web Site, MDS`。 需要此列表中的第一个值（在此例中为 `MDS1`）来部署模型。  
  
5.  若要创建包含模型对象和数据的包，请键入以下命令，其中 *ModelName*、 *VersionName*、 *ServiceName* 和 *PackageName* 分别是模型名称、版本名称、服务名称以及 .pkg 输出文件的名称：  
  
    ```  
    MDSModelDeploy createpackage -model ModelName -version VersionName -service ServiceName -package PackageName -includedata  
    ```  
  
     如果您不希望包含数据，请不要使用 `-version` 和 `-includedata` 开关。  
  
6.  按 Enter。 成功创建包后，将显示一条消息“MDSModelDeploy 操作已成功完成”。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [使用 MDSModelDeploy 部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
## <a name="see-also"></a>另请参阅  
 [模型部署选项 &#40;Master Data Services&#41;](../master-data-services/model-deployment-options-master-data-services.md)   
 [部署模型 (Master Data Services)](../master-data-services/deploying-models-master-data-services.md)  
  
  
