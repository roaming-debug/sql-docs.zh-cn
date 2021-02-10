---
description: 步骤 4：服务器返回记录集（RDS 教程）
title: 步骤4： Server 返回 (RDS 教程) 的记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server returns Recordset
ms.assetid: 3d1855c4-419c-4810-b5ea-6c874b5e2905
author: rothja
ms.author: jroth
ms.openlocfilehash: 1008e4c9212f13bd8f4fb2ba8765ac52230aa612
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031729"
---
# <a name="step-4-server-returns-the-recordset-rds-tutorial"></a>步骤 4：服务器返回记录集（RDS 教程）
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 RDS 会将检索到的 **记录集** 对象转换为可发送回客户端的窗体 (也就是说， *它* 将 **记录记录集**) 。 转换的具体形式及其发送方式取决于服务器是在 Internet 上还是在 intranet、局域网上，或者是动态链接库。 但是，此详细信息并不重要;重要的是，RDS 将 **记录集** 发送回客户端。  
  
 在客户端，返回一个 **记录集** 对象并将其分配给本地变量。  
  
```vb
Sub RDSTutorial4()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query("DSN=Pubs", "SELECT * FROM Authors")  
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [步骤5： DataControl 在 RDS 教程 (可用) ](./step-5-datacontrol-is-made-usable-rds-tutorial.md)   
 [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)