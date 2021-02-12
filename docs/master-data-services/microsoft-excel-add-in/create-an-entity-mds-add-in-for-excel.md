---
description: 创建实体（用于 Excel 的 MDS 外接程序）
title: 创建实体
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d354abb3-88fe-4b40-a374-f6256b84ffae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b6a150ba658ff65991f0c6d73cbb557268075474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272408"
---
# <a name="create-an-entity-mds-add-in-for-excel"></a>创建实体（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，管理员可以创建新的实体来存储数据。 当您创建实体时，应加载要存储的数据的至少一个抽样。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域和 **“资源管理器”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md)。  
  
-   您必须具有要在其中创建实体的现有模型。 有关详细信息，请参阅[创建模型 (Master Data Services)](../../master-data-services/create-a-model-master-data-services.md)。  
  
-   请确保您的数据满足以下要求：  
  
    -   数据应具有标题行。  
  
    -   具有 **“名称”** 和 **“代码”** 列是有帮助的。 **“代码”** 是每行的唯一标识符。  
  
    -   您应该除了标题行之外还具有至少一行数据。 不是所有列都需要值，但数据应该代表将位于实体中的数据。  
  
    -   如果你具有包含唯一标识符的列（在 MDS 中称作 **代码**），则请确保这些值是唯一的。 如果没有包含标识符的列，则可以让这些列在您创建实体时自动生成。  
  
    -   请确保没有单元包含公式。  
  
    -   请确保没有单元包含时间值。 可以将日期值保存在 MDS 中，但不能保存时间值。  
  
### <a name="to-create-an-entity-and-load-data"></a>创建实体和加载数据  
  
1.  打开或创建包含您要加载的数据的 Excel 工作表。  
  
2.  选择要将新实体加载到的单元。  
  
3.  在 **“主数据”** 选项卡上的 **“生成模型”** 组中，单击 **“创建实体”**。  
  
4.  如果系统提示您连接到某一 MDS 存储库，则进行连接。  
  
5.  在 **“创建实体”** 对话框中，保留默认范围或者更改该范围以便应用于您想要加载的数据。  
  
6.  不要取消选中 **“我的数据带有标题”** 复选框。  
  
7.  从 **“模型”** 列表中，选择某一模型。  
  
8.   从“版本”列表中，选择某一版本。  
  
9. 在 **“新的实体名称”** 框中，键入实体的名称。  
  
10. 从 **“代码”** 列表中，选择包含唯一标识符或具有自动生成的代码的列。  
  
11. 可选。 从 **“名称”** 列表中，选择包含每个成员的名称的列。  
  
12. 单击“确定”。  在已成功创建该实体后，将显示一个新的标题行，单元将突出显示，并且工作表名称将更新以匹配该实体名称。  
  
## <a name="next-steps"></a>后续步骤  
  
-   若要查看发生的错误，请在 **“发布并验证”** 组中单击 **“显示状态”**。 将显示 ValidationStatus 和 InputStatus 列。 有关详细信息，请参阅[验证数据（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md)。  
  
-   确认属性以您期望的数据类型创建。  
  
## <a name="see-also"></a>另请参阅  
 [创建基于域的属性（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/create-a-domain-based-attribute-mds-add-in-for-excel.md)  
  
  
