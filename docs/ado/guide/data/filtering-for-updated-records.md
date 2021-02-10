---
description: 筛选更新的记录
title: 筛选已更新记录 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- filtering for updated records [ADO]
ms.assetid: 4a798921-d7bb-47c9-a252-550fd9463ec9
author: rothja
ms.author: jroth
ms.openlocfilehash: c96f0519ac3dc421fe3717bd19c00735223e3375
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033108"
---
# <a name="filtering-for-updated-records"></a>筛选更新的记录
在调用 UpdateBatch 之前，您可以使用 "记录集筛选器" 属性来仅查看自打开记录集以来或上次调用 UpdateBatch 后发生更改的记录。 为此，请将筛选器设置为等于 adFilterPendingRecords 以确定将更新多少条记录，如下面的代码示例中所示。  
  
## <a name="remarks"></a>备注  
 此示例通过在调用 UpdateBatch 之前筛选记录集来扩展以前的 UpdateBatch 示例，显示用户将更改哪些记录，并允许用户使用) 的 CancelBatch 方法取消更新 (。  
  
```  
'BeginFilterPend  
    '...  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
  
    ' Change value of Phone field for first record in Recordset, saving value  
    ' for later restoration.  
    intId = objRs1("ShipperId")  
    sPhone = objRs1("Phone")  
  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Add two new records  
    For i = 0 To 1  
        objRs1.AddNew  
        objRs1(1) = "New Shipper #" & CStr((i + 1))  
        objRs1(2) = "(nnn) 555-" & i & i & i & i  
    Next i  
  
    objRs1.Filter = adFilterPendingRecords  
  
    MsgBox "There are " & objRs1.RecordCount & " records pending.", _  
            , "Filter Pending"  
  
    ' Cancel the updates  
    objRs1.CancelBatch  
'EndFilterPend  
```  
  
## <a name="see-also"></a>另请参阅  
 [批处理模式](./batch-mode.md)