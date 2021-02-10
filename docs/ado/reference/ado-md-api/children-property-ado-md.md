---
description: Children 属性 (ADO MD)
title: 子属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Children
- Children
helpviewer_keywords:
- Children property [ADO MD]
ms.assetid: 61d36468-1ccd-467a-9cb5-17d0bfacc766
author: rothja
ms.author: jroth
ms.openlocfilehash: 7fae1b50b219abc4e5841bb537d0befaf35a4b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055671"
---
# <a name="children-property-ado-md"></a>Children 属性 (ADO MD)
返回一个 [成员](./members-collection-ado-md.md) 集合，其中的当前 [成员](./member-object-ado-md.md) 是层次结构中的父级。  
  
## <a name="return-values"></a>返回值  
 返回 **成员** 集合，它是只读的。  
  
## <a name="remarks"></a>备注  
 **子** 属性包含 **成员** 集合，该集合的当前 **成员** 为其层次结构父级。 叶级 **成员** 对象在 **members** 集合中没有子成员。 此属性仅在属于某一 [级别](./level-object-ado-md.md)对象的 **成员** 对象上受支持。 从属于某个 [位置](./position-object-ado-md.md)对象的 **成员** 对象引用此属性时，将发生错误。  
  
## <a name="applies-to"></a>应用于  
 [成员对象 (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [ChildCount 属性 (ADO MD)](./childcount-property-ado-md.md)