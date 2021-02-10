---
description: FilterAxis 属性 (ADO MD)
title: FilterAxis 属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset::FilterAxis
- FilterAxis
helpviewer_keywords:
- FilterAxis property [ADO MD]
ms.assetid: 9c656963-531e-4cd1-b698-d5f42a9b7ba3
author: rothja
ms.author: jroth
ms.openlocfilehash: e53967bf577bfc69c07d7c1e5e49e42a3ca613d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054712"
---
# <a name="filteraxis-property-ado-md"></a>FilterAxis 属性 (ADO MD)
指示有关当前 [单元集](./cellset-object-ado-md.md)的筛选器信息。  
  
## <a name="return-values"></a>返回值  
 返回一个 [轴](./axis-object-ado-md.md) 对象，它是只读的。  
  
## <a name="remarks"></a>备注  
 使用 **FilterAxis** 属性可以返回有关用于切分数据的维度的信息。 **轴** 的 [DimensionCount](./dimensioncount-property-ado-md.md)属性返回切片器维度数。 此轴通常只包含一行。  
  
 [单元格](./cellset-object-ado-md.md)集对象的 [轴](./axes-collection-ado-md.md)集合中不包含 **FilterAxis** 返回的 **轴**。  
  
## <a name="applies-to"></a>应用于  
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [Axis 对象 (ADO MD) ](./axis-object-ado-md.md)   
 [维度对象 (ADO MD) ](./dimension-object-ado-md.md)   
 [DimensionCount 属性 (ADO MD)](./dimensioncount-property-ado-md.md)