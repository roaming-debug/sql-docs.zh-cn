---
description: 多维架构和数据的概述
title: 多维架构和数据概述 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional schemas and data
ms.assetid: ce37fa06-c581-4d80-9a9b-c3aa66408909
author: rothja
ms.author: jroth
ms.openlocfilehash: 2dcd54dfb8ba5797588c615f987830dcb8aa5736
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032179"
---
# <a name="overview-of-multidimensional-schemas-and-data"></a>多维架构和数据的概述
## <a name="understanding-multidimensional-schemas"></a>了解多维架构  
 ADO MD 中的中央元数据对象是 *多维数据集*，它由一组结构化的相关维度、层次结构、级别和成员组成。  
  
 *维度* 是多维数据库中独立的数据类别，派生自业务实体。 维度通常包含要用作数据库度量值的查询条件的项。  
  
 *层次结构* 是维度的聚合路径。 一个维度可能具有多个级别的粒度，它们具有父子关系。 层次结构定义这些级别如何相关。  
  
 *级别* 是层次结构中的聚合步骤。 对于包含多个信息层的维度，每个层都是一个级别。  
  
 *成员* 是维度中的数据项。 通常，使用成员创建标题或描述数据库的度量值。  
  
 多维数据集由 ADO MD 中的 [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) 对象表示。 维度、层次结构、级别和成员还由其对应的 ADO MD 对象表示： [维度](../../reference/ado-md-api/dimension-object-ado-md.md)、 [层次结构](../../reference/ado-md-api/hierarchy-object-ado-md.md)、 [级别](../../reference/ado-md-api/level-object-ado-md.md)和 [成员](../../reference/ado-md-api/member-object-ado-md.md)。  
  
### <a name="dimensions"></a>维度  
 多维数据集的维度依赖于要在数据库中建模的业务实体和数据类型。 通常，每个维度都是独立的入口点或用于选择数据的机制。  
  
 例如，包含销售数据的多维数据集包含以下五个维度：销售人员、地理、时间、产品和度量值。 度量值维度包含实际的销售数据值，而其他维度则表示对销售数据值进行分类和分组的方式。  
  
 "地域" 维度具有以下成员集：  
  
```console
{All, North America, Europe, Canada, USA, UK, Germany, Canada-West,  
Canada-East, USA-NW, USA-SW, USA-NE, USA-SE, England, Scotland,   
Wales,Ireland, Germany-North, Germany-South, Ottawa, Toronto,   
Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston,   
Shreveport, Miami, Boston, New York, London, Dover, Glasgow,   
Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin,   
Hamburg, Munich, Stuttgart}  
```  
  
### <a name="hierarchies"></a>层次结构  
 层次结构定义了可以 "汇总" 或分组的维度级别的方式。 一个维度可以有多个层次结构。 "地域" 维度中存在自然层次结构：  
  
### <a name="levels"></a>级别  
 在上图所示的 "地理位置" 维度中，每个框表示层次结构中的一个级别。  
  
 每个级别都有一组成员，如下所示：  
  
-   世界 `= {All}`  
  
-   洲 `= {North America, Europe}`  
  
-   （ `= {Canada, USA, UK, Germany}`  
  
-   区域 `= {Canada-East, Canada-West, USA-NE, USA-NW, USA-SE, USA-SW, England, Ireland, Scotland, Wales, Germany-North, Germany-South}`  
  
-   城市 `= {Ottawa, Toronto, Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston, Shreveport, Miami, Boston, New York, London, Dover, Glasgow, Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin, Hamburg, Munich, Stuttgart}`  
  
### <a name="members"></a>成员  
 层次结构的叶级别的成员没有子级，根级别的成员没有父项。 所有其他成员至少具有一个父级和至少一个子成员。 例如，地域维度中的层次结构树的部分遍历产生以下父子关系：  
  
-   `{All} (parent of) {Europe, North America}`  
  
-   `{North America} (parent of) {Canada, USA}`  
  
-   `{USA} (parent of) {USA-NE, USA-NW, USA-SE, USA-SW}`  
  
-   `{USA-NW} (parent of) {Boise, Seattle}`  
  
 成员可以按维度在一个或多个层次结构中进行合并。 请考虑一个时间维度，其中有两种方法可从 "天" 级别汇总到 "年" 级别：  
  
 此示例还说明了另一种特征： Year-Week 层次结构中 Week 级别的某些成员未出现在 Year-Quarter 层次结构的任何级别。 因此，层次结构不需要包含维度的所有成员。  
  
## <a name="see-also"></a>另请参阅  
 [ADO MD 对象模型](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (多维)  (ADO MD) ](./ado-multidimensional-ado-md.md)   
 [用 ADO MD 进行编程](./programming-with-ado-md.md)   
 [使用 ADO 与 ADO MD](./using-ado-with-ado-md.md)   
 [使用多维数据](./working-with-multidimensional-data.md)