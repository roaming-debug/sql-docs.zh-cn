---
description: BOF、EOF 属性 (ADO)
title: BOF、EOF 属性 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::BOF
- Recordset15::EOF
helpviewer_keywords:
- EOF property [ADO]
- BOF property [ADO]
ms.assetid: 36c31ab2-f3b6-4281-89b6-db7e04e38fd2
author: rothja
ms.author: jroth
ms.openlocfilehash: 8dd77c7bd3e7c92ec5eb970d0134db4e6e39258f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035487"
---
# <a name="bof-eof-properties-ado"></a>BOF、EOF 属性 (ADO)
-   **BOF** 指示当前记录位置位于 [记录集](./recordset-object-ado.md) 对象中的第一条记录之前。  
  
-   **EOF** 指示当前记录位置在 **Recordset** 对象的最后一条记录之后。  
  
## <a name="return-value"></a>返回值  
 **BOF** 和 **EOF** 属性返回 **布尔** 值。  
  
## <a name="remarks"></a>备注  
 使用 **BOF** 和 **EOF** 属性来确定 **recordset** 对象是否包含记录，或者在从记录移到记录时是否超出 **recordset** 对象的限制。  
  
 如果当前记录位置在第一条记录之前，则 **BOF** 属性返回 **True** (-1) ; 如果当前记录位置位于第一条记录的前面或后面，则 **为 False** (0) 。  
  
 如果当前记录位置晚于最后一条记录，则 **EOF** 属性返回 **True** ; 如果当前记录位置位于最后一条记录之前，则返回 **False** 。  
  
 如果 **BOF** 或 **EOF** 属性为 **True**，则没有当前记录。  
  
 如果打开的 **记录集** 对象不包含任何记录，则 **BOF** 和 **EOF** 属性将设置为 **True** (查看 [RecordCount](./recordcount-property-ado.md) 属性以获取有关 **记录集** 的此状态的详细信息) 。 如果打开的 **记录集** 对象至少包含一条记录，则第一条记录为当前记录， **BOF** 和 **EOF** 属性为 **False**。  
  
 如果您删除 **记录集** 对象中的最后一个记录，则 **BOF** 和 **EOF** 属性可能会保持 **为假** ，直到您尝试重新定位当前记录为止。  
  
 此表显示了允许使用不同的 **BOF** 和 **EOF** 属性组合的 **移动** 方法。  
  
||MoveFirst<br /><br /> MoveLast|MovePrevious<br /><br /> 移动 < 0|移动0|MoveNext<br /><br /> 移动 > 0|  
|------|-----------------------------|---------------------------------|------------|-----------------------------|  
|**BOF** =**True**， **EOF** = **False**|然后用户才能访问|错误|错误|然后用户才能访问|  
|**BOF** =**False**， **EOF** = **True**|允许|允许|错误|错误|  
|均 **为 True**|错误|错误|错误|错误|  
|均 **为 False**|允许|允许|允许|允许|  
  
 允许 **Move** 方法并不保证该方法能够成功定位记录;这只意味着调用指定的 **Move** 方法不会生成错误。  
  
 下表显示了在调用各种 **Move** 方法但无法成功定位记录时， **BOF** 和 **EOF** 属性设置会发生什么情况。  
  
||BOF|EOF|  
|------|---------|---------|  
|**MoveFirst**、 **MoveLast**|设置为 **True**|设置为 **True**|  
|**移动** 0|没有变化|没有变化|  
|**MovePrevious**、 **Move** < 0|设置为 **True**|没有变化|  
|**MoveNext**， **移动** > 0|没有变化|设置为 **True**|  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [BOF、EOF 和 Bookmark 属性示例 (VB) ](./bof-eof-and-bookmark-properties-example-vb.md)   
 [BOF、EOF 和 Bookmark 属性示例 (VC + +) ](./bof-eof-and-bookmark-properties-example-vc.md)