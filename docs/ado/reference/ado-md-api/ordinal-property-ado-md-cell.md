---
description: Ordinal 属性（ADO MD 单元）
title: 序号属性 (ADO MD 单元格) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: a6001168-b954-47f0-ba0d-c05c4cc40c58
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d76ed341fe71f7a5927f6a7e84a63322adcf13f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051038"
---
# <a name="ordinal-property-ado-md-cell"></a>Ordinal 属性（ADO MD 单元）
按单元格集中的位置唯一标识 [单元格](./cell-object-ado-md.md) 。  
  
## <a name="return-values"></a>返回值  
 返回一个 **长** 整型，并且为只读。  
  
## <a name="remarks"></a>备注  
 单元格的序号值用于唯一标识单元格集中的单元格。 从概念上讲，单元在单元集中编号，就如同单元集 *是一个二维* 数组，其中 *p* 是 [轴](./axes-collection-ado-md.md)的数目。 单元按行优先顺序从零开始编号。 下面是用于计算单元序号的公式：  
  
 单元格的序号值可以与单元格[集](./cellset-object-ado-md.md)对象的[Item](./item-property-ado-md-cellset.md)属性一起使用，以快速检索[单元](./cell-object-ado-md.md)。  
  
## <a name="applies-to"></a>应用于  
 [单元对象 (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VBScript) 的轴示例 ](./axis-example-vbscript.md)   
 [单元集对象 (ADO MD) ](./cellset-object-ado-md.md)   
 [项属性 (ADO MD 单元集) ](./item-property-ado-md-cellset.md)   
 [Ordinal 属性（ADO MD 位置）](./ordinal-property-ado-md-position.md)