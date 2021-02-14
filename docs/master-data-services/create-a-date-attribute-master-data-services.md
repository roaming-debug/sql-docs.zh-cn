---
description: 创建日期属性 (Master Data Services)
title: 创建日期属性
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating date attributes [Master Data Services]
- attributes [Master Data Services], creating date attributes
ms.assetid: 22a8f1a3-b4f2-4cfa-8495-7daad5ce9d12
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: a32550093aefa8eb27af1736e317a7ce4caf0eaa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272718"
---
# <a name="create-a-date-attribute-master-data-services"></a>创建日期属性 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，在您希望用户输入日期作为属性值时创建日期属性。  
  
> [!NOTE]  
>  该属性称为 DateTime，但不支持时间值。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   您必须具有要为其创建属性的实体。 有关详细信息，请参阅[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)。  
  
### <a name="to-create-a-date-attribute"></a>创建日期属性  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
3.  在“管理实体”  页上，选择要为其创建属性的实体所在的行。  
  
4.  单击 **“属性”**。  
  
5.  在“管理属性”  页上，执行下列操作之一，然后单击“添加” 。  
  
    -   如果是叶成员的属性，请从“成员类型”  列表框中选择“叶”  。  
  
    -   如果属性是针对合并成员，则从“成员类型”  列表框中选择“合并”  。  
  
    -   如果属性是针对集合，则从“成员类型”  列表框中选择“集合”  。  
  
6.  在 **“名称”** 框中，键入属性的名称。 有关不可用作属性名称的单词列表，请参阅[保留字 (Master Data Services)](../master-data-services/reserved-words-master-data-services.md)。  
  
7.  （可选）键入显示名称，并在“说明”  框中键入属性的说明。  
  
8.  在 **“显示像素宽度”** 框中，键入要在 **“资源管理器”** 网格中显示的属性列的宽度。  
  
9. 从“属性类型”列表中，选择“自由格式”。  
  
10. 从 **“数据类型”** 列表中，选择 **“DateTime”**。  
  
11. 从 **“输入掩码”** 列表中，选择用于日期的格式。  
  
12. 根据需要，选择 **“启用更改跟踪”** 可以跟踪对属性组的更改。 有关详细信息，请参阅[向更改跟踪组添加属性 (Master Data Services)](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)。  
  
13. 单击“保存”。  
  
## <a name="to-display-the-time-portion-of-a-datetime-value"></a>显示日期时间值的时间部分  
 若要使用户界面显示日期时间值的时间部分，您必须为该属性选择一个适当的输入掩码。 对于日期时间属性没有内置的此类掩码，但您可以添加一个新掩码以便显示时间。 为此，在存储内置掩码的 MDS 数据库的 mdm.tblList 表中添加一行。 此行应具有以下各值：  
  
|列|Value|  
|-|-|  
|ListCode|lstInputMask|  
|ListName|“输入掩码”|  
|Seq|19|  
|List Option|dd/MM/yyyy hh:mm:ss tt|  
|Option ID|19|  
|IsVisible|1|  
|Group_ID|3|  
  
 在 mdm.tblList 表中输入具有上述值的行后，在“输入掩码”列表框中会提供“dd/MM/yyyy hh:mm:ss tt”掩码。 然后，您可以选择该掩码，以便在 MDS 资源管理器中某实体的日期时间属性列中显示日期和时间。  
  
 输入掩码是一个自定义 .NET DateTime 格式字符串。 有关详细信息，请参阅 [自定义日期和时间格式字符串](https://msdn.microsoft.com/library/8kb3ddd4\(v=vs.110\).aspx)  
  
## <a name="see-also"></a>另请参阅  
 [属性 &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [更改属性名称和数据类型 &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)   
 [&#40;Master Data Services 创建 Domain-Based 属性&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)   
 [创建文件属性 (Master Data Services)](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
  
