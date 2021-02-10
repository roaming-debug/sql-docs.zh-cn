---
description: 步骤 6：将更改发送到服务器（RDS 教程）
title: 步骤6：将更改发送到服务器 (RDS 教程) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
author: rothja
ms.author: jroth
ms.openlocfilehash: d6a769a22a86e145727079af6fc067b1552f2bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031709"
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>步骤 6：将更改发送到服务器（RDS 教程）
如果编辑 **Recordset** 对象，则 (的任何更改（即，添加、更改或) 删除的行）都可以发送回服务器。  
  
> [!NOTE]
>  可以用 ADO 对象和 Microsoft OLE DB 远程处理提供程序隐式调用 RDS 的默认行为。 查询可以返回 **记录集**，并且编辑的 **记录集** 可以更新数据源。 本教程不调用具有 ADO 对象的 RDS，但这是它的外观：  
  
```vb
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=https://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **部分 A** 假设在这种情况下，你只使用了 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 和 **记录集** 对象现在与 **RDS 关联。DataControl**。 如果仍设置 [服务器](../../reference/rds-api/server-property-rds.md)和 [连接](../../reference/rds-api/connect-property-rds.md)属性， [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md)方法会将数据源更新为 **记录集** 对象的任何更改。  
  
```vb
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "https://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **B 部分** 或者，您可以使用 [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象（指定连接和 **记录集** 对象）更新服务器。  
  
```vb
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **本教程到此结束。**  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft OLE DB 远程处理提供程序 (ADO 服务提供程序) ](../appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [RDS 教程](./rds-tutorial.md)   
 [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)