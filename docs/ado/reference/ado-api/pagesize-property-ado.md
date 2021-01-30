---
description: PageSize 属性 (ADO)
title: " (ADO) 的 PageSize 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageSize
helpviewer_keywords:
- PageSize property [ADO]
ms.assetid: e57930a6-46c4-4a17-a3b6-f79e94d5c9c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b4e3a85d2c496f88939100a79d464bf1076fd61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170629"
---
# <a name="pagesize-property-ado"></a>PageSize 属性 (ADO)
指示记录 [集中](./recordset-object-ado.md)有一页的记录数。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **长整型** 值，该值指示页上有多少记录。 默认值为 **10**。  
  
## <a name="remarks"></a>备注  
 使用 **PageSize** 属性来确定构成逻辑页数据的记录数。 通过建立页面大小，可以使用 [AbsolutePage](./absolutepage-property-ado.md) 属性移动到特定页面的第一条记录。 当你希望允许用户逐页浏览数据，一次查看一定数量的记录时，这非常有用。  
  
 此属性可随时设置，它的值将用于计算特定页面的第一条记录的位置。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VB) ](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage、PageCount 和 PageSize 属性示例 (VC + +) ](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePage 属性 (ADO) ](./absolutepage-property-ado.md)   
 [PageCount 属性 (ADO)](./pagecount-property-ado.md)