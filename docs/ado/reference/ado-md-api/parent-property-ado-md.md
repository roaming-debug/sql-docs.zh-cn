---
description: Parent 属性 (ADO MD)
title: 父属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parent
- Member::Parent
helpviewer_keywords:
- Parent property [ADO MD]
ms.assetid: 32c278c1-d8e1-4bb7-9ecd-2fbfdffee34b
author: rothja
ms.author: jroth
ms.openlocfilehash: 0674b3455611e8700e8859d4ced5d4052c51135c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050948"
---
# <a name="parent-property-ado-md"></a>Parent 属性 (ADO MD)
指示作为层次结构中当前 [成员](./member-object-ado-md.md) 的父成员的成员。  
  
## <a name="return-values"></a>返回值  
 返回一个 [成员](./member-object-ado-md.md) 对象，并且是只读的。  
  
## <a name="remarks"></a>备注  
 位于层次结构顶层 (根) 的成员没有父项。 此属性仅在属于某一 [级别](./level-object-ado-md.md)对象的 **成员** 对象上受支持。 从属于某个 [位置](./position-object-ado-md.md)对象的 **成员** 对象引用此属性时，将发生错误。  
  
## <a name="applies-to"></a>应用于  
 [成员对象 (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [Children 属性 (ADO MD)](./children-property-ado-md.md)