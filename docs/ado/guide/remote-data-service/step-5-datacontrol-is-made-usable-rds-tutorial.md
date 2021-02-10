---
description: 步骤 5：DataControl 变为可用（RDS 教程）
title: 步骤5： DataControl 在 RDS 教程)  (可用 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 6453043514657b03e72eeea032451f647f6c2eec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036087"
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>步骤 5：DataControl 变为可用（RDS 教程）
返回的 **记录集** 对象可供使用。 您可以检查、导航或编辑它，就像对待任何其他 **记录集** 一样。 您可以对 **记录集** 执行哪些操作取决于您的环境。 Visual Basic 和 Visual C++ 具有可直接或间接使用 **记录集** 来实现数据控件的可视化控件。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 例如，如果在 Microsoft Internet Explorer 中显示网页，则可能希望在视觉对象控件中显示 **记录集** 对象数据。 网页上的视觉对象控件无法直接访问 **记录集** 对象。 但是，它们可以通过 RDS 访问 **Recordset** 对象 [。DataControl](../../reference/rds-api/datacontrol-object-rds.md)。 **RDS。** 当视觉对象的 [SourceRecordset](../../reference/rds-api/recordset-sourcerecordset-properties-rds.md)属性设置为 **Recordset** 对象时，DataControl 变得可用。  
  
 视觉对象对象的 **DATASRC** 参数必须设置为 **RDS。DataControl**，并将其 **DATAFLD** 属性设置为)  (列的 **记录集** 对象字段。  
  
 在本教程中，设置 **SourceRecordset** 属性：  
  
```vb
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [步骤6：将更改发送到服务器 (RDS 教程) ](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)