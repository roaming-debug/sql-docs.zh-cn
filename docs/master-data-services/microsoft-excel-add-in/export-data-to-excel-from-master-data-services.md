---
description: 将数据从 Master Data Services 导出到 Excel
title: 将数据导出到 Excel
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: dd29389b-928c-4e50-995c-c6af27f97805
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e8b7ab33b2d10ab09a8ab3fcf76532dd571936df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337102"
---
# <a name="export-data-to-excel-from-master-data-services"></a>将数据从 Master Data Services 导出到 Excel

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，必须先从 MDS 存储库中加载数据，之后才能处理这些数据。  
  
 如果想要在加载之前筛选数据集，请参阅[导出前筛选数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-    您必须有权访问“资源管理器”功能区域。  
  
### <a name="to-export-data-from-mds-into-excel"></a>将数据从 MDS 导出到 Excel  
  
1.  打开 Excel，在 **“主数据”** 选项卡上连接到 MDS 存储库。 有关详细信息，请参阅[连接到 MDS 存储库（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)。  
  
2.  在 **“主数据资源管理器”** 窗格中，选择所需模型和版本。 随之填充实体列表。  
  
    -   如果 **“主数据资源管理器”** 窗格不可见，请在 **“连接并加载”** 组中，单击 **“显示资源管理器”**。  
  
    -   如果 **“主数据资源管理器”** 窗格被禁用，则是因为现有工作表中已包含 MDS 管理的数据。 若要启用该窗格，请打开一个新工作表。  
  
3.  在“主数据资源管理器”窗格的实体列表中，双击要加载的实体。  
  
    > [!NOTE]  
    >  -   只有前一百万个成员才能加载到 Excel 中。 若要在加载前对列表进行筛选，请在 **“连接并加载”** 组中，单击 **“筛选器”**。  
    > -   在作为约束列表（基于域的属性）的列中，默认情况下只加载前 25,000 个值。 您可以在 excelusersettings.config 文件（位于安装 Excel 的计算机上）的 MaximumDbaEntitySize 属性中更改该数值。 此文件位于 C:\Users\\<user\>\AppData\Local\Microsoft\Microsoft SQL Server\130\MasterDataServices\\。  
    >   
    >      如果基于域的属性的值数超过 MaximumDbEntitySize 属性设置，则不会加载值列表。  
  
    > [!NOTE]  
    >  使用用于 32 位 Excel 的 Microsoft Excel 外接程序加载文本分隔数据时，如果“要加载的单元计数”和“要发布的单元计数”属性均设置为最大值 1000，则将出现内存不足错误。 必须使用 64 位 Excel，才能使用“要加载的单元计数”和“要发布的单元计数”的最大值设置。  
  
## <a name="next-steps"></a>后续步骤  
 [将数据从 Excel 导入 Master Data Services（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/import-data-from-excel-to-master-data-services-mds-add-in-for-excel.md)  
  
## <a name="see-also"></a>另请参阅  
 [概述：将数据导出到 Excel &#40;MDS Add-in for Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)   
 ["筛选器" 对话框 &#40;MDS Add-in for Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-dialog-box-mds-add-in-for-excel.md)   
 [概述：从 Excel 导入数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
