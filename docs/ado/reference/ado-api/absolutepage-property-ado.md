---
description: AbsolutePage 属性 (ADO)
title: " (ADO) 的 AbsolutePage 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AbsolutePage
helpviewer_keywords:
- AbsolutePage property [ADO]
ms.assetid: ddb58a35-ec3a-423c-a504-3c65e62c23d4
author: rothja
ms.author: jroth
ms.openlocfilehash: 5f5b8019bedb2273c15ee3362f7fd3cb9688308e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035969"
---
# <a name="absolutepage-property-ado"></a>AbsolutePage 属性 (ADO)
指示当前记录所在的页。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 对于32位代码，设置或返回 (PageCount **) 的**[记录集](./recordset-object-ado.md)对象中的页数，或返回 [PositionEnum](./positionenum.md)值之一 [](./pagecount-property-ado.md) 。  
  
 对于64位代码，请使用提供的数据类型来存储64位值。 例如，你可以使用 **Long** 或其他64值（如 DBORDINAL）。 不要使用 **PositionEnum** 值，因为它们限制为32位长度。  
  
## <a name="remarks"></a>备注  
 此属性可用于标识当前记录所在的页码。 它使用 [PageSize](./pagesize-property-ado.md) 属性以逻辑方式将 **记录集** 对象的总行集计数划分为一系列页面，其中每个页面的记录数等于 **PageSize** (除了最后一页，其中可能包含) 的记录。 提供程序必须支持适当的功能，此属性才可用。  
  
-   获取或设置 **AbsolutePage** 属性时，ADO 将 [AbsolutePosition](./absoluteposition-property-ado.md) 属性和 [PageSize](./pagesize-property-ado.md) 属性一起使用，如下所示：  
  
-   若要获取 **AbsolutePage**，ADO 首先检索 **AbsolutePosition**，然后将其除以 **PageSize**。  
  
-   若要设置 **AbsolutePage**，ADO 会按如下所示移动 **AbsolutePosition** ：它将 **PageSize** 乘以新的 **AbsolutePage** 值，然后将1添加到值中。 因此，成功设置 **AbsolutePage** 后 **记录集中** 的当前位置是该页中的第一条记录。  
  
 与 **AbsolutePosition** 属性类似，当当前记录是 **记录集** 的第一条记录时， **AbsolutePage** 是从1开始的，等于1。 设置此属性可以移动到特定页的第一条记录。 获取 **PageCount** 属性中的总页数。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VB) ](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VC + +) ](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePosition 属性 (ADO) ](./absoluteposition-property-ado.md)   
 [PageCount 属性 (ADO) ](./pagecount-property-ado.md)   
 [PageSize 属性 (ADO)](./pagesize-property-ado.md)