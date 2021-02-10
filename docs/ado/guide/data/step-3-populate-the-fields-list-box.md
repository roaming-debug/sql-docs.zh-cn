---
description: 步骤 3：填充字段列表框
title: 步骤3：填充 "字段" 列表框 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 315c32dc-aeb1-4629-b30e-87b44e8f84d1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6555f71d210524622e936026c8945168635de140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032429"
---
# <a name="step-3-populate-the-fields-list-box"></a>步骤 3：填充字段列表框
若要填充 "字段" 列表框，请将以下代码插入到的 Click 事件处理程序中 `lstMain` ：  
  
```  
Private Sub lstMain_Click()  
    Dim rec As Record  
    Dim rs As Recordset  
    Set rec = New Record  
    Set rs = New Recordset  
    grs.MoveFirst  
    grs.Move lstMain.ListIndex  
    lstDetails.Clear  
    rec.Open grs  
    Select Case rec.RecordType  
        Case adCollectionRecord:  
            Set rs = rec.GetChildren  
            While Not rs.EOF  
                lstDetails.AddItem rs(0)  
                rs.MoveNext  
            Wend  
        Case adSimpleRecord:  
            recFields rec, lstDetails, txtDetails  
  
        Case adStructDoc:  
    End Select  
  
End Sub  
```  
  
 此代码将分别声明并实例化本地记录和记录集对象 `rec` `rs` 。  
  
 与中所选资源相对应的行 `lstMain` 成为的当前行 `grs` 。 然后，"详细信息" 列表框将被清除，并以的 `rec` 当前行 `grs` 作为源打开。  
  
 如果资源是由 [RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md)指定的集合记录，则 `rs` 会在记录的子级上打开本地记录集。然后， `lstDetails` 将用的行中的值填充 `rs` 。  
  
 如果资源是简单记录， `recFields` 则调用。 有关的详细信息 `recFields` ，请参阅下一步。  
  
 如果资源是结构化文档，则不实现任何代码。  
  
## <a name="see-also"></a>另请参阅  
 [Internet 发布方案](../../../ado/guide/data/internet-publishing-scenario.md)   
 [步骤2：初始化主列表框](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)   
 [步骤 4：填充详细信息文本框](../../../ado/guide/data/step-4-populate-the-details-text-box.md)
