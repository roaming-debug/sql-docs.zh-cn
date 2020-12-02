---
description: Merge Join Transformation
title: 合并联接转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.mergejointrans.f1
- sql13.dts.designer.mergejointransformation.f1
helpviewer_keywords:
- datasets [Integration Services]
- Merge Join transformation
- datasets [Integration Services], joining
- joining datasets [Integration Services]
- joins [SQL Server], SSIS
ms.assetid: cd8b0412-f83b-4bd2-b227-e53dcfd941a8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fab9313b76527744a53af64203794f7ec1b696d6
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88477646"
---
# <a name="merge-join-transformation"></a>Merge Join Transformation

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  合并联接转换提供了一个输出，该输出是通过使用 FULL、LEFT 或 INNER 联接将两个已排序数据集进行联接而生成的。 例如，可使用 LEFT 联接来联接包含产品信息的表与列出产品的制造国家/地区的表。 结果是一个列出所有产品及其产地国家/地区的表。  
  
 可以按照下列方法来配置合并联接转换：  
  
-   指定联接是 FULL、LEFT 联接，还是 INNER 联接。  
  
-   指定联接所使用的列。  
  
-   指定转换是否将空值处理为等于其他空值。  
  
    > [!NOTE]  
    >  如果不将空值处理为相等的值，则转换将如同 SQL Server 数据库引擎那样处理空值。  
  
 此转换有两个输入和一个输出。 它不支持错误输出。  
  
## <a name="input-requirements"></a>输入要求  
 合并联接转换要求输入已排序的数据。 有关此重要要求的详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。  
  
## <a name="join-requirements"></a>联接要求  
 合并联接转换要求被联接的列具有匹配的元数据。 例如，不能将具有数值数据类型的列和具有字符数据类型的列相联接。 如果数据为字符串数据类型，第二个输入中列的长度必须小于或等于被合并的第一个输入中列的长度。  
  
## <a name="buffer-throttling"></a>缓冲区中止  
 您不再必须配置 **MaxBuffersPerInput** 属性的值，因为 Microsoft 已进行了更改，减少了合并联接转换将占用过多内存的风险。 在合并联接的多个输入以不相等速率生成数据时，有时候可能会发生此问题。  
  
## <a name="related-tasks"></a>Related Tasks  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关如何设置此转换的属性的信息，请单击下列主题之一：  
  
-   [使用合并联接转换扩展数据集](../../../integration-services/data-flow/transformations/extend-a-dataset-by-using-the-merge-join-transformation.md)  
  
-   [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [为合并转换和合并联接转换排序数据](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="merge-join-transformation-editor"></a>合并联接转换编辑器
  可以使用 **“合并联接转换编辑器”** 对话框指定联接类型、联接列和输出列，以合并通过联接组合的两个输入。  
  
> [!IMPORTANT]  
>  合并联接转换要求输入已排序的数据。 有关此重要要求的详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。  
  
### <a name="options"></a>选项  
 **联接类型**  
 指定要使用内部联接、左外部联接还是完全联接。  
  
 **交换输入**  
 通过使用“交换输入”按钮来交换输入的顺序。 对于左外部联接选项，此选项可能有用。  
  
 **输入**  
 对于要在合并的输出中包含的每个列，请首先从可用输入列表中相应地进行选择。  
  
 输入显示在两个单独的表中。 请选择要包含在输出中的列。 通过拖动列可以在表之间创建联接。 若要删除某个联接，请选定该联接，再按 Delete 键。  
  
 **输入列**  
 从所选输入的可用列的列表中选择要包含在合并的输出中的列。  
  
 **输出别名**  
 为每个输出列键入一个别名。 默认值为输入列的名称；不过，您也可以任选一个唯一的描述性名称。  
  
## <a name="see-also"></a>另请参阅  
 [合并转换](../../../integration-services/data-flow/transformations/merge-transformation.md)   
 [Union All 转换](../../../integration-services/data-flow/transformations/union-all-transformation.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
