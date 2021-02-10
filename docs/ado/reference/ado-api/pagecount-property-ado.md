---
description: PageCount 属性 (ADO)
title: " (ADO) 的 PageCount 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageCount
helpviewer_keywords:
- PageCount property [ADO]
ms.assetid: b601b56c-0ac4-44ee-bc91-c3d2d104f00a
author: rothja
ms.author: jroth
ms.openlocfilehash: 828a0f9d872ab1040700941d85980e0b5deecf98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041227"
---
# <a name="pagecount-property-ado"></a>PageCount 属性 (ADO)
指示 [Recordset](./recordset-object-ado.md) 对象包含多少页数据。  
  
## <a name="return-value"></a>返回值  
 返回一个 **长整型** 值，该值指示 **记录集中** 的页数。  
  
## <a name="remarks"></a>备注  
 使用 **PageCount** 属性来确定 **记录集** 对象中的数据页的数目。 *页面* 是大小等于 [PageSize](./pagesize-property-ado.md) 属性设置的记录组。 即使最后一个页面不完整，因为记录数少于 **PageSize** 值，也会在 **PageCount** 值中作为附加页面计数。 如果 **记录集** 对象不支持此属性，则该值将为-1，指示 **PageCount** 为无法确定。  
  
 有关页面功能的详细信息，请参阅 **PageSize** 和 [AbsolutePage](./absolutepage-property-ado.md) 属性。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VB) ](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VC + +) ](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePage 属性 (ADO) ](./absolutepage-property-ado.md)   
 [PageSize 属性 (ADO) ](./pagesize-property-ado.md)   
 [RecordCount 属性 (ADO)](./recordcount-property-ado.md)