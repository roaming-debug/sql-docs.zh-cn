---
description: Description 属性 (ADO MD)
title: Description 属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Description
- Level::Description
- CubeDef::Description
- Hierarchy::Description
- Description
- Dimension::Description
helpviewer_keywords:
- Description property [ADO MD]
ms.assetid: 6d626d35-0bf3-4f24-9934-ad9c9c91273a
author: rothja
ms.author: jroth
ms.openlocfilehash: 4fbb1ff444c4d5b2cbf233ec2999411ab11ddfd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051158"
---
# <a name="description-property-ado-md"></a>Description 属性 (ADO MD)
返回当前对象的文本说明。  
  
## <a name="return-values"></a>返回值  
 返回一个 **字符串** ，并且为只读。  
  
## <a name="remarks"></a>备注  
 对于 [成员](./member-object-ado-md.md) 对象， **Description** 仅适用于度量值和公式成员。 **Description** 为所有其他类型的成员 ( "" ) 返回空字符串。 有关各种类型成员的详细信息，请参阅 [Type](./type-property-ado-md.md) 属性。  
  
 此属性仅在属于 [级别](./level-object-ado-md.md)对象的 **成员** 对象上受支持。 从属于某个 [位置](./position-object-ado-md.md)对象的 **成员** 对象引用此属性时，将发生错误。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [CubeDef 对象 (ADO MD)](./cubedef-object-ado-md.md)  
        [维度对象 (ADO MD)](./dimension-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [层次结构对象 (ADO MD)](./hierarchy-object-ado-md.md)  
        [级别对象 (ADO MD)](./level-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [成员对象 (ADO MD)](./member-object-ado-md.md)  
    :::column-end:::
:::row-end:::