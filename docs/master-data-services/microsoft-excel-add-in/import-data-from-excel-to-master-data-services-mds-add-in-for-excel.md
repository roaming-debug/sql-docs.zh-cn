---
description: 将数据从 Excel 导入 Master Data Services（用于 Excel 的 MDS 外接程序）
title: 从 Excel 导入数据
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 89fce454-a816-4b33-a26a-d1b9741d269b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 22d69fafa852007c9642fe641205185115b42175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352748"
---
# <a name="import-data-from-excel-to-master-data-services-mds-add-in-for-excel"></a>将数据从 Excel 导入 Master Data Services（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，如果你在 Excel 中完成处理后想要保存更改以便于其他用户访问，可以将数据发布到 MDS 存储库。  
  
> [!NOTE]
>  -   发布更改时，将删除 MDS 管理的单元上的注释。  
> -   在 MDS 托管单元中不支持公式。 MDS 托管单元中的公式作为文本值处理。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-    您必须有权访问“资源管理器”功能区域。  
  
-   活动工作表必须包含 MDS 管理的数据，并且您必须对 MDS 管理的数据做出更改或向其添加数据。  
  
-   如果添加的是成员，在自动为该实体生成代码时不必指定 **Code** 值。 有关详细信息，请参阅 [&#41;&#40;Master Data Services 自动创建代码 ](../../master-data-services/automatic-code-creation-master-data-services.md)。  
  
### <a name="to-publish-data-to-the-mds-repository"></a>将数据发布到 MDS 存储库  
  
1.  在 **“发布并验证”** 组中，单击 **“发布”**。  
  
2.  可选。 如果显示“发布并添加批注”对话框，请选择为所有更新共享相同的批注（注释），或者为每个更改逐个添加批注。  
  
3.  可选。 选中 **“不再显示此对话框”** 复选框。 通过选择 **“设置”** 并选中 **“在发布时显示‘发布并添加批注’对话框”** 复选框，将来可始终显示该对话框。  
  
4.  单击“发布” 。  
  
> [!NOTE]  
>  如果要向工作表添加新成员（行）但无法将其成功添加至 MDS 存储库，则你可能并非对工作表中的所有属性都具有“更新”权限。 在 **“检查”** 选项卡上的 **“更改”** 组中，单击 **“取消工作表保护”** ，然后再次尝试发布。  
  
## <a name="next-steps"></a>后续步骤  
 [应用业务规则（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)  
  
## <a name="see-also"></a>另请参阅  
 [概述：从 Excel &#40;MDS Add-in for Excel 导入数据&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)   
 [验证数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md)  
  
  
