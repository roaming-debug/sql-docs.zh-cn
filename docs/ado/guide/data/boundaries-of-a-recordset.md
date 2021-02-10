---
description: 记录集的边界
title: 记录集的边界 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EOF property [ADO], boundaries of a Recordset
- Recordset object [ADO], boundaries of a Recordset
- BOF property [ADO], boundaries of a Recordset
ms.assetid: c0dd4a0f-478d-4c5e-b5d5-7535f211d064
author: rothja
ms.author: jroth
ms.openlocfilehash: f737ad11af65045a1d923f427c7487e4b751aaa8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028012"
---
# <a name="boundaries-of-a-recordset"></a>记录集的边界
**Recordset** 支持 **BOF** 和 **EOF** 属性，分别描绘数据集的开头和结尾。 可以将 **BOF** 和 **EOF** 视为位于 **记录集** 开头和结尾的 "虚拟" 记录。 计算 **BOF** 和 **EOF** 后，示例 **记录集** 现在如下所示：  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|BOF|||  
|7|叔叔 Bob 的随机是 Pears|30.0000|  
|14|Tofu|23.2500|  
|28|Rssle Sauerkraut|45.6000|  
|51|Manjimup 是苹果|53.0000|  
|74|Longlife Tofu|10.0000|  
|EOF|||  
  
 当游标移过最后一条记录时， **EOF** 将设置为 **True**;否则，其值为 **False**。 同样，当光标移动到第一条记录之前， **BOF** 将设置为 **True**;否则，其值为 **False**。 这些属性通常用于枚举数据集中的记录，如以下 JScript 代码段中所示。  
  
```  
while (objRecordset.EOF != true)   
{  
   // Work on the current record.  
   ...  
   // Advance the cursor forward to the next record.  
   objRecordset.MoveNext();  
}  
or  
while (objRecordset.BOF != true)   
{  
   // Work on the current record.  
   ...  
   // Move the cursor to the previous record.  
   objRecordset.MovePrevious();  
}  
```  
  
 如果 **BOF** 和 **EOF** 都为 **True**，则 **Recordset** 对象为空。 对于新打开的非空 **Recordset** 对象，这两个属性都将为 **False** 。 可以结合使用 **BOF** 和 **EOF** 属性来确定 **Recordset** 对象是否为空，如以下 JScript 代码段中所示。  
  
```  
if (objRecordset.EOF == true && objRecordset.BOF == true)  
{  
   WScript.Echo("we got an empty dataset.");  
}  
else  
{  
   WScript.Echo("we got a full dataset.");  
}  
```  
  
 此方案适用于所有类型的游标，并且独立于基础提供程序。 如果尝试通过检查 **记录集** 的 **RecordCount** 属性值是否为零 (0) 来确定 Recordset 对象的空，则必须采取预防措施，以便使用支持返回结果中的记录数的适当游标和提供程序。  
  
 如果删除 **记录集** 对象中的最后一个记录，游标将保持不确定状态。 在您尝试重新定位当前记录（具体取决于提供程序）之前， **BOF** 和 **EOF** 属性可能会保留 **为 False** 。 有关详细信息，请参阅 [使用 Delete 方法删除记录](./deleting-records-using-the-delete-method.md)。