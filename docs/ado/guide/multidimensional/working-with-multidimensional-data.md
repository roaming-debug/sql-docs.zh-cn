---
description: 使用多维数据
title: 使用多维数据 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional data [ADO]
ms.assetid: 84387746-aa3e-44fd-ad6c-a8214a6966dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 33ae3c1ca1cb8f89378b4f2ea468b41d6d3a4ac5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036617"
---
# <a name="working-with-multidimensional-data"></a>使用多维数据
*单元集* 是针对多维数据的查询的结果。 它包含轴的集合，通常不超过四个轴，通常只有2到3个轴。 *轴* 是一个或多个维度的成员集合，用于在多维数据集中查找或筛选特定值。  
  
 *位置* 是沿轴的点。 对于包含单个维度的轴，这些位置是维度成员的子集。 如果一个轴包含多个维度，则每个位置都是一个复合实体，其中 n 是包含 *n* 个部分的，其中 *n* 是沿该轴定向的维度数。 位置的每个部分都是一个构成维度中的成员。  
  
 例如，如果包含销售数据的多维数据集中的地理维度和产品维度沿单元集的 x 轴方向，则沿此轴的位置可能包含成员 "USA" 和 "计算机"。 在此示例中，确定沿 x 轴的位置要求每个维度的成员沿轴方向。  
  
 *单元格* 是位于轴坐标交叉点处的对象。 每个单元都有多个与之关联的信息，包括数据本身、带格式的字符串 (单元数据) 的可显示形式以及单元序号值。  (每个单元格都是单元集中唯一的序号值。 单元集内第一个单元格的序号值为零，而具有八列的单元集的第二行中最左边的单元格的序号值为8。 )   
  
 例如，多维数据集具有以下六个维度 (请注意，此多维数据集架构与 [多维架构和数据) 概述](./overview-of-multidimensional-schemas-and-data.md) 中给定的示例略有不同：  
  
-   Salesperson  
  
-   地理 (自然层次结构) -洲、国家/地区、州等  
  
-   季度-季度、月、日  
  
-   年  
  
-   度量值-Sales、PercentChange、BudgetedSales  
  
-   产品  
  
 以下单元集表示所有产品1991的销售额：  
  
> [!NOTE]
>  可以将示例中的单元值视为轴位置序号的有序对，其中第一个数字表示 x 轴位置，第二个数字表示 y 轴位置。  
  
 此单元集的特征如下：  
  
-   轴维度：季度、销售人员、地域  
  
-   筛选器维度：度量值，年，产品  
  
-   两个轴：列 (x、轴 0) 和行 (y，或轴 1)   
  
-   x 轴：两个嵌套维度，销售人员和地理位置  
  
-   y 轴：季度维度  
  
 X 轴具有两个嵌套维度：销售人员和地理位置。 从地理位置，选择四个成员：西雅图、波士顿、美国南部和日本。 从销售人员选择两个成员：情人和 Nash。 这会生成此轴上的总计八个位置 (8 = 4 * 2) 。  
  
 每个坐标都表示为一个具有两个成员的位置-一个来自 "销售人员" 维度，另一个是 "地域" 维度：  
  
```console
(Valentine, Seattle), (Valentine, Boston), (Valentine, USA_North),  
(Valentine, Japan), (Nash, Seattle), (Nash, Boston), (Nash, USA_North),  
(Nash, Japan)  
```  
  
 Y 轴只有一个维度，其中包含以下八个位置：  
  
```console
Jan, Feb, Mar, Qtr2, Qtr3, Oct, Nov, Dec  
```  
  
 单元集、单元格、轴和位置都 ADO MD 由相应对象表示：单元 [集](../../reference/ado-md-api/cellset-object-ado-md.md)、 [单元格](../../reference/ado-md-api/cell-object-ado-md.md)、 [轴](../../reference/ado-md-api/axis-object-ado-md.md)和 [位置](../../reference/ado-md-api/position-object-ado-md.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ADO MD 对象模型](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (多维)  (ADO MD) ](./ado-multidimensional-ado-md.md)   
 [多维架构和数据的概述](./overview-of-multidimensional-schemas-and-data.md)   
 [用 ADO MD 进行编程](./programming-with-ado-md.md)   
 [使用 ADO 与 ADO MD](./using-ado-with-ado-md.md)