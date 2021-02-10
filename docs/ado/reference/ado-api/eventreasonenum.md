---
description: EventReasonEnum
title: EventReasonEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EventReasonEnum
helpviewer_keywords:
- EventReasonEnum enumeration [ADO]
ms.assetid: 7d4a5496-ec2d-4936-b36a-7049a82be4b4
author: rothja
ms.author: jroth
ms.openlocfilehash: d8789e0c2819bfd4a976dfa047b6081284e7292d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034177"
---
# <a name="eventreasonenum"></a>EventReasonEnum
指定导致事件发生的原因。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adRsnAddNew**|1|操作已添加一条新记录。|  
|**adRsnClose**|9|操作关闭了 **记录集**。|  
|**adRsnDelete**|2|操作删除了记录。|  
|**adRsnFirstChange**|11|操作对记录进行第一次更改。|  
|**adRsnMove**|10|操作在记录 **集中** 移动记录指针。|  
|**adRsnMoveFirst**|12|操作将记录指针移动到记录 **集中** 的第一条记录。|  
|**adRsnMoveLast**|15|操作将记录指针移动到记录 **集中** 的最后一条记录。|  
|**adRsnMoveNext**|13|操作将记录指针移动到记录 **集中** 的下一条记录。|  
|**adRsnMovePrevious**|14|操作将记录指针移动到记录 **集中** 的上一个记录。|  
|**adRsnRequery**|7|操作将重新查询 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)。|  
|**adRsnResynch**|8|操作将 **记录集** 与数据库重新同步。|  
|**adRsnUndoAddNew**|5|操作反转了新记录的添加。|  
|**adRsnUndoDelete**|6|操作已反转记录的删除。|  
|**adRsnUndoUpdate**|4|操作反转了记录的更新。|  
|**adRsnUpdate**|3|操作更新了现有记录。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. EventReason|  
|AdoEnums. EventReason. 关闭|  
|AdoEnums. EventReason. DELETE|  
|AdoEnums.EventReason.FIRSTCHANGE|  
|AdoEnums. EventReason|  
|AdoEnums. EventReason. MOVEFIRST|  
|AdoEnums. EventReason. MOVELAST|  
|AdoEnums. EventReason|  
|AdoEnums. EventReason. MOVEPREVIOUS|  
|AdoEnums. EventReason|  
|AdoEnums. EventReason|  
|AdoEnums.EventReason.UNDOADDNEW|  
|AdoEnums.EventReason.UNDODELETE|  
|AdoEnums.EventReason.UNDOUPDATE|  
|AdoEnums. EventReason. UPDATE|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [WillChangeRecord 和 RecordChangeComplete 事件 (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)  

        [WillChangeRecordset 和 RecordsetChangeComplete 事件 (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)  
    :::column-end:::
    :::column:::
        [WillMove 和 MoveComplete 事件 (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)  
    :::column-end:::
:::row-end:::
