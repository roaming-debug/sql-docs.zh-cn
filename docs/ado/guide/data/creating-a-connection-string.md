---
description: 创建连接字符串
title: 创建连接字符串 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
- connection strings [ADO]
ms.assetid: 14eae122-2d1e-40c8-b88e-b7cb8dfbc93b
author: rothja
ms.author: jroth
ms.openlocfilehash: eda5438d4fbda93b9e8c19602b9236d1812e6826
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033287"
---
# <a name="creating-a-connection-string"></a>创建连接字符串
连接字符串包含参数/值对的列表 (也就是说，参数) 由分号分隔。 例如：  
  
```syntax
"arg1=val1; arg2=val2; ... argN=valN;"  
```  
  
 所有参数都必须由 ADO 或指定的提供程序识别。  
  
 ADO 识别连接字符串中的以下五个参数。  
  
|参数|说明|  
|--------------|-----------------|  
|*提供程序*|指定要用于连接的访问接口的名称。|  
|*文件名*|指定特定于提供程序的文件的名称 (例如，保留的数据源对象) 包含预设的连接信息。|  
|*URL*|将连接字符串指定为标识资源的绝对 URL，如文件或目录。|  
|*远程提供程序*|指定打开客户端连接时要使用的提供程序的名称。 仅 (远程数据服务。 ) |  
|*远程服务器*|指定打开客户端连接时要使用的服务器的路径名称。 仅 (远程数据服务。 ) |  
  
 其他参数传递给 *提供程序* 参数中名为的提供程序，而无需 ADO 处理。  
  
 HelloData 中的 HelloData 应用程序 [：一个简单的 ADO 应用](./hellodata-a-simple-ado-application.md) 程序使用以下连接字符串：  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Data Source=MySqlServer;" & _  
             "Initial Catalog=Northwind;Integrated Security='SSPI';"  
```  
  
 在此连接字符串中，ADO 仅识别 `"Provider=SQLOLEDB"` 参数，该参数指定作为 ADO 数据源的 SQL Server 的 Microsoft OLE DB 提供程序。 参数/值对的其余部分 `"Data Source=MySqlServer; Initial Catalog=Northwind;Integrated Security='SSPI';"` 逐字传递到此提供程序。 此类参数的类型和有效性是特定于提供程序的。 有关可以在连接字符串中传递的有效参数的信息，请参阅各个提供商的文档。  
  
 根据 SQL Server 文档的 OLE DB 提供程序，您可以将 "Server" 替换为 *数据源* 参数，将 "Database" 替换为 *初始目录* 参数。 因此，以下连接字符串将产生与上面的结果相同的结果：  
  
```vb
m_sConnStr = "Provider=SQLOLEDB;Server=MySqlServer;" & _  
             "Database=Northwind;Integrated Security='SSPI';"  
```