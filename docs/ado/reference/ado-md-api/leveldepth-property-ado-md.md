---
description: LevelDepth 属性 (ADO MD)
title: LevelDepth 属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- LevelDepth
- Member::LevelDepth
helpviewer_keywords:
- LevelDepth property [ADO MD]
ms.assetid: 8a1cfe2c-f207-4445-b152-ade090f64608
author: rothja
ms.author: jroth
ms.openlocfilehash: e987d2cab7f20930bbdefbd4bcf959717a28ba7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169795"
---
# <a name="leveldepth-property-ado-md"></a>LevelDepth 属性 (ADO MD)
指示层次结构的根和 [成员](./member-object-ado-md.md)之间的级别数。  
  
## <a name="return-values"></a>返回值  
 返回一个 **长** 整型，并且是只读的。  
  
## <a name="remarks"></a>备注  
 使用 **LevelDepth** 属性来确定 [成员](./member-object-ado-md.md)对象与层次结构的根级别的距离。 根级别的成员的 **LevelDepth** 是0。 这对应于[Level](./level-object-ado-md.md)对象的[Depth](./depth-property-ado-md.md)属性。  
  
## <a name="applies-to"></a>应用于  
 [成员对象 (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [深度属性 (ADO MD) ](./depth-property-ado-md.md)   
 [级别对象 (ADO MD)](./level-object-ado-md.md)