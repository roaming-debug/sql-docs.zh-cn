---
description: 发送更新：UpdateBatch 方法
title: 发送更新： UpdateBatch 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 87123797-831f-48e0-94b5-f669f9ca194a
author: rothja
ms.author: jroth
ms.openlocfilehash: 88e441fd0ff58f63cf5ac701ec2620c29048b1e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032489"
---
# <a name="sending-the-updates-updatebatch-method"></a>发送更新：UpdateBatch 方法
下面的代码通过将 LockType 属性设置为 adLockBatchOptimistic，将 CursorLocation 设置为 adUseClient，以批处理模式打开记录集。 它添加两个新记录，并更改现有记录中字段的值，保存原始值，然后调用 UpdateBatch 将更改发送回数据源。  
  
## <a name="remarks"></a>备注  
  
```  
'BeginBatchUpdate  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT ShipperId, CompanyName, Phone FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
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
  
    ' Send the updates  
    objRs1.UpdateBatch  
'EndBatchUpdate  
```  
  
 如果在调用 UpdateBatch 方法时编辑当前记录或添加新记录，ADO 将自动调用 Update 方法，以便在将批处理更改传输到提供程序之前保存当前记录的所有挂起的更改。  
  
## <a name="see-also"></a>另请参阅  
 [批处理模式](../../../ado/guide/data/batch-mode.md)
