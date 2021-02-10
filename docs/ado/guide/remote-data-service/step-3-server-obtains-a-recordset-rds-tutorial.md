---
description: 步骤 3：服务器获取记录集（RDS 教程）
title: 步骤3： Server (RDS 教程) 获取记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server obtains Recordset
ms.assetid: 9c6779c9-1208-4696-ac51-c39f3a6d9240
author: rothja
ms.author: jroth
ms.openlocfilehash: d031c1af5719b711c5555af0db1bc1e420e2fc8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036097"
---
# <a name="step-3-server-obtains-a-recordset-rds-tutorial"></a>步骤 3：服务器获取记录集（RDS 教程）
服务器程序使用连接字符串和命令文本在数据源中查询所需的行。 ADO 通常用于检索此 **记录集**，但也可以使用其他 Microsoft 数据访问接口（如 OLE DB）。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 自定义服务器程序可能如下所示：  
  
```vb
Public Function ServerProgram(cn as String, qry as String) as Object  
Dim rs as New ADODB.Recordset  
   rs.CursorLocation = adUseClient  
   rs.Open qry, cn   
   rs.ActiveConnection = Nothing  
   Set ServerProgram = rs  
End Function  
```  
  
## <a name="see-also"></a>另请参阅  
 [步骤4： Server 返回记录集 (RDS 教程) ](./step-4-server-returns-the-recordset-rds-tutorial.md)   
 [RDS 教程 (VBScript)](./rds-tutorial-vbscript.md)