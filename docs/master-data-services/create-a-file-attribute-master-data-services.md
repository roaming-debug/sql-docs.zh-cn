---
description: 创建文件属性 (Master Data Services)
title: 创建文件属性
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating file attributes [Master Data Services]
- attributes [Master Data Services], creating file attributes
ms.assetid: d224886b-2ef1-4658-8b01-2213cc4b8df6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 25023eb11f9968f9edc3e7159f32252b0a116291
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272678"
---
# <a name="create-a-file-attribute-master-data-services"></a>创建文件属性 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，创建文件属性以便使用文件填充属性值。

## <a name="prerequisites"></a>先决条件
 若要执行此过程：

-   您必须有权访问 **“系统管理”** 功能区域。

-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。

-   要为其创建属性的实体必须存在。 有关详细信息，请参阅[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)。

## <a name="attribute-information"></a>属性信息
 对于创建的每个属性，系统都会在网格中添加一行（其中包含七列）。 下表对这些列进行了说明。

|列|说明|
|------------|-----------------|
|状态|属性状态。<br /><br /> 单击 "保存" 时，将显示 ![更新状态](../master-data-services/media/mds-statusicon-updating.png "用于更新状态的图标") 图像的图标，指示属性正在更新。<br /><br /> 如果在创建或编辑属性时出现错误，则会显示 ![错误状态图标图标](../master-data-services/media/mds-statusicon-error.png "错误状态图标") 。<br /><br /> 否则，状态为 "正常"，将显示 !["确定状态](../master-data-services/media/mds-statusicon-ok.png ""正常" 状态图标") " 图像的图标。|
|名称|特性名称。|
|显示名称|属性显示名称。|
|说明|属性说明。|
|显示像素宽度|属性宽度。|
|类型和属性|属性的类型和数据类型信息。|
|启用更改跟踪|指定是否启用该属性以进行更改跟踪，并在括号中显示组号。|

 单击属性后可看到以下信息。

-   **创建者**：创建属性的用户的用户名。

-   **创建时间**：创建属性的日期和时间。

-   **更新者**：上次更新属性的用户的用户名。

-   **创建时间**：上次更新属性的日期和时间。

### <a name="to-create-a-file-attribute"></a>创建文件属性

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

9. 从“属性类型”  列表中选择“文件” 。

10. 从“文件扩展名”列表中，选择用户可上载的一个文件类型，或者接受默认值 (*.\*) 以便允许所有文件类型。

11. 根据需要，选择 **“启用更改跟踪”** 可以跟踪对属性组的更改。 有关详细信息，请参阅[向更改跟踪组添加属性 (Master Data Services)](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)。

12. 单击“ **保存**”。

## <a name="see-also"></a>另请参阅
 [属性 &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md) [将属性名称和数据类型更改 &#40;Master Data Services ](../master-data-services/change-an-attribute-name-and-data-type-master-data-services.md)&#41;Domain-Based [创建 &#40;属性 ](../master-data-services/create-a-domain-based-attribute-master-data-services.md)Master Data Services&#41;[&#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)


