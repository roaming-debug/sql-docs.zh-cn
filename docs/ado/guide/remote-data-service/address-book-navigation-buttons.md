---
description: 通讯簿导航按钮
title: 通讯簿导航按钮 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], navigation buttons
- address book application scenario [ADO], navigation buttons
ms.assetid: f0dd84c6-5c33-4ab9-82b4-4c42dfdd2277
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee0bb698c64de6fb04f776caa4e34c27b8d49a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032057"
---
# <a name="address-book-navigation-buttons"></a>通讯簿导航按钮
通讯簿应用程序显示网页底部的导航按钮。 通过选择数据的第一行或最后一行或与当前所选内容相邻的行，可使用导航按钮在 HTML 网格显示中浏览数据。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="navigation-sub-procedures"></a>导航 Sub 过程  
 通讯簿应用程序包含多个过程，这些过程允许用户单击 **第一个**、 **下** 一个、 **上** 一个和 **最后** 一个按钮来四处移动数据。  
  
 例如，单击 **第一个** 按钮将激活 VBScript First_OnClick Sub 过程。 此过程执行 [MoveFirst](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) 方法，使数据的第一行成为当前选定内容。 单击 " **上一步** " 按钮将激活 Last_OnClick Sub 过程，该过程调用 [MoveLast](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) 方法，使最后一行数据成为当前选定内容。 其余的导航按钮的工作方式类似。  
  
```vb
' Move to the first record in the bound Recordset.  
Sub First_OnClick  
   DC1.Recordset.MoveFirst  
End Sub  
  
' Move to the next record from the current position   
' in the bound Recordset.  
Sub Next_OnClick  
   If Not DC1.Recordset.EOF Then    ' Cannot move beyond bottom record.  
      DC1.Recordset.MoveNext  
      Exit Sub  
   End If     
End Sub  
  
' Move to the previous record from the current position in the bound   
' Recordset.  
Sub Prev_OnClick  
   If Not DC1.Recordset.BOF Then    ' Cannot move beyond top record.  
      DC1.Recordset.MovePrevious  
      Exit Sub  
   End If  
End Sub  
  
' Move to the last record in the bound Recordset.  
Sub Last_OnClick  
   DC1.Recordset.MoveLast  
End Sub  
```  
  
## <a name="see-also"></a>另请参阅  
 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS)](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)