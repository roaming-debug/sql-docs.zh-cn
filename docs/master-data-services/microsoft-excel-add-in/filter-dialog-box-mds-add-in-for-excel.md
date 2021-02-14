---
description: “筛选器”对话框（用于 Excel 的 MDS 外接程序）
title: “筛选器”对话框
ms.custom: microsoft-excel-add-in
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b987b141-5abf-4161-a073-4cfc3e7f5aae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e650bd924e893b21cbc6b154887d9cd6e66565b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352772"
---
# <a name="filter-dialog-box-mds-add-in-for-excel"></a>“筛选器”对话框（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在中 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] ，使用 "**筛选器**" 对话框可以在将 MDS 管理的数据加载到 Excel 中之前缩小其列表范围。  
  
 此对话框包含三个部分： **“列”**、 **“行”** 和 **“摘要”**。  
  
## <a name="columns"></a>列  
 使用“列”  部分可以确定要在 Excel 中显示的属性（列）。  
  
|控件名称|说明|  
|------------------|-----------------|  
|属性类型|属性类型描述您想要使用的成员的类型。 在大多数情况下，类型为 **“叶”**。 有关成员类型的详细信息，请参阅[成员 (Master Data Services)](../../master-data-services/members-master-data-services.md)。|  
|显式层次结构|如果您选择 **“已合并”** 属性类型，请选择已合并成员所属的层次结构。 有关详细信息，请参阅 [显式层次结构 (Master Data Services)](../../master-data-services/explicit-hierarchies-master-data-services.md)。|  
|属性组|属性组是对属性子集进行分组的方式。 如果您想要显示可用属性的子集，请选择属性组。 有关属性组的详细信息，请参阅[属性组 (Master Data Services)](../../master-data-services/attribute-groups-master-data-services.md)。|  
|全选|单击此选项可选择列表中显示的所有属性。|  
|全部清除|单击此选项可清除列表中显示的选定属性。<br /><br /> 您不能清除 **Name** 和 **Code**。|  
|向上箭头/向下箭头|单击此选项可在列表中上下移动所选的属性。 从上到下的顺序对应于列在工作表中从左到右的显示顺序。|  
  
## <a name="rows"></a>“行”  
 使用“行”  部分可以确定要在 Excel 中显示的成员（行）。 通过定义要显示的行的筛选条件可以确定要显示的行。  
  
|控件名称|说明|  
|------------------|-----------------|  
|属性|显示筛选所依据的属性。 如果未列出任何属性，这是因为尚未添加任何属性。<br /><br /> 注意：可依据不准备在工作表中显示的属性进行筛选。|  
|运算符|显示对应于所选属性的类型的运算符。 有关详细信息，请参阅 [Filter 运算符 (Master Data Services)](../../master-data-services/filter-operators-master-data-services.md)。|  
|条件|筛选要依据的条件。|  
|更新摘要|处理大型数据集时，单击该选项可使用要加载的数据的详细信息来更新 **“摘要”** 部分。|  
|添加|在您单击 **“列”** 部分中的属性，然后单击 **“添加”** 后，将向筛选器列表添加属性。|  
|全部删除|从列表中删除所有筛选器。|  
|删除|从列表中删除所选的筛选器。|  
  
## <a name="summary"></a>总结  
 使用 **“摘要”** 部分可以在加载数据之前先查看有关要加载的数据的详细信息。  
  
|控件名称|说明|  
|------------------|-----------------|  
|建模|模型的名称。|  
|Version|版本的名称。|  
|实体|实体的名称。|  
|“行”|基于在 **“行”** 部分中应用的筛选器要加载到 Excel 中的行数。|  
|列|基于在 **“列”** 部分中所选的属性要加载到 Excel 中的列数。|  
  
## <a name="see-also"></a>另请参阅  
 [导出 &#40;MDS Add-in for Excel 前筛选数据&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)   
 [概述：将数据导出到 Excel（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  
