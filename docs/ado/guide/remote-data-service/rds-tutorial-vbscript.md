---
description: RDS 教程 (VBScript)
title: RDS 教程 (VBScript) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RDS tutorial [ADO], VBScript
ms.assetid: e2a48c4d-88b1-43ff-a202-9cdec54997d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 693f5858a80d5bc986dc16f993aa5c5a78c54d7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031869"
---
# <a name="rds-tutorial-vbscript"></a>RDS 教程 (VBScript)
这是在 Microsoft Visual Basic Scripting Edition 中编写的 RDS 教程。 有关本教程用途的说明，请参阅 [RDS 教程](./rds-tutorial.md)。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 在本教程中， [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 和 [RDS。](../../reference/rds-api/dataspace-object-rds.md) 在设计时创建了空间，也就是说，它们是用对象标记定义的，如下所示： `<OBJECT>...</OBJECT>` 。 另外，还可以在运行时通过 [CreateObject 方法在 (RDS) ](../../reference/rds-api/createobject-method-rds.md) 方法中创建它们。 例如， **RDS。** 可以创建 DataControl 对象，如下所示：  
  
```vb
Set DC = Server.CreateObject("RDS.DataControl")  
   <!-- RDS.DataControl -->  
   <OBJECT   
      ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33">  
   </OBJECT>  
  
   <!-- RDS.DataSpace -->  
   <OBJECT   
      ID="DS1" WIDTH=1 HEIGHT=1  
      CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
   </OBJECT>  
  
   <SCRIPT LANGUAGE="VBScript">  
  
   Sub RDSTutorial()  
   Dim DF1   
```  
  
## <a name="step-1---specify-a-server-program"></a>步骤 1-指定服务器程序  
 VBScript 可以通过访问 Active Server 页面可用的 VBScript **request.servervariables** 方法来发现正在运行的 IIS Web 服务器的名称：  
  
```vb
"https://<%=Request.ServerVariables("SERVER_NAME")%>"  
```  
  
 但对于本教程，请使用假想服务器 "yourServer"。  
  
> [!NOTE]
>  请注意 **ByRef** 参数的数据类型。 VBScript 不允许您指定变量类型，因此您必须始终传递 **Variant**。 当使用 HTTP 时，如果使用 Rds 调用，则 RDS 将允许将变量传递给需要非变量的方法 **。空间** 对象 [CreateObject](../../reference/rds-api/createobject-method-rds.md) 方法。 使用 DCOM 或进程内服务器时，必须与客户端和服务器端上的参数类型相匹配，否则将收到 "类型不匹配" 错误。  
  
```vb
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "https://yourServer")  
```  
  
## <a name="step-2a---invoke-the-server-program-with-rdsdatacontrol"></a>步骤 2a-通过 RDS 调用服务器程序。DataControl  
 此示例只是说明 RDS 的默认行为的注释 **。DataControl** 是执行指定的查询。  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial2A()  
   Dim RS  
   DC1.Refresh  
   Set RS = DC1.Recordset  
...  
```  
  
## <a name="step-2b---invoke-the-server-program-with-rdsserverdatafactory"></a>步骤 2b-通过 RDSServer 调用服务器程序。 DataFactory  
  
## <a name="step-3---server-obtains-a-recordset"></a>步骤 3-服务器获取记录集  
  
## <a name="step-4---server-returns-the-recordset"></a>步骤 4-服务器返回记录集  
  
```vb
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors")  
```  
  
## <a name="step-5---datacontrol-is-made-usable-by-visual-controls"></a>步骤 5-DataControl 可供视觉对象控件使用  
  
```vb
' Assign the returned recordset to the DataControl.  
  
DC1.SourceRecordset = RS  
```  
  
## <a name="step-6a---changes-are-sent-to-the-server-with-rdsdatacontrol"></a>步骤 6a-通过 RDS 将更改发送到服务器。DataControl  
 此示例只是一个说明 **RDS 如何DataControl** 执行更新。  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial6A()  
Dim RS  
DC1.Refresh  
...  
Set RS = DC1.Recordset  
' Edit the Recordset object...  
' The SERVER and CONNECT properties are already set from Step 2A.  
Set DC1.SourceRecordset = RS  
...  
DC1.SubmitChanges  
```  
  
## <a name="step-6b---changes-are-sent-to-the-server-with-rdsserverdatafactory"></a>步骤 6b-通过 RDSServer 将更改发送到服务器。 DataFactory  
  
```vb
DF.SubmitChanges "DSN=Pubs", RS  
  
End Sub  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 **本教程到此结束。**  
  
## <a name="see-also"></a>另请参阅  
 [RDS 教程](./rds-tutorial.md)