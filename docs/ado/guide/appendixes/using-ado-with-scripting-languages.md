---
description: 配合使用 ADO 与脚本语言
title: 使用 ADO 和脚本语言 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- scripting languages [ADO]
- ADO, scripting languages
ms.assetid: 76fc4d00-0c9f-422b-af5c-af6ed8fb29d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f1919f3524f05035a4376f30cf2e36ae731bdea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028847"
---
# <a name="using-ado-with-scripting-languages"></a>配合使用 ADO 与脚本语言
在脚本环境中，ADO 允许通过服务器端脚本来公开数据。 在此方案中，ADO、它使用的基础 OLE DB 提供程序以及引用给定数据存储所需的任何其他组件均安装在运行 Internet Information Services (IIS) 的服务器上。 使用 Active Server Pages (ASP) ，ADO 是在可生成 HTML 的脚本中引用的组件，例如。 可以通过 HTTP 将此 HTML 内容传递到客户端 Web 浏览器。 通过使用脚本，网页可以将操作发送回服务器端脚本，从而使您可以更新、遍历或查看特定数据。  
  
 在网页中使用 ActiveX 对象之前，请务必了解该对象是否对脚本是安全的。 如果对象被视为脚本编写安全，则表示该控件不能在用户的计算机上执行任何有害操作，因此可以在不请求用户批准的情况下执行。 下表列出了 ADO 对象，并指示它们是否对脚本是安全的。  
  
|对象|脚本安全？|  
|------------|-------------------------|  
|ADO 连接|是|  
|ADO 命令|否|  
|ADO 参数|否|  
|ADO 记录集|是|  
|ADO 记录|是|  
|ADO 流|是|  
|ADO 错误|否|  
|ADOX 目录|否|  
|ADOX 单元集|否|  
|RDS DataControl|是|  
|RDS 空间|是|  
|RDS DataFactory|否|  
  
 下表列出了 Windows DAC/MDAC 中包含的提供程序，并指示它们是否对脚本是安全的。  
  
|提供程序|脚本安全？|  
|--------------|-------------------------|  
|形状|是|  
|保留|是|  
|Remote|是|  
|SQL Server (SQLOLEDB 的 OLE DB 提供程序) |否|  
|ODBC (MSDASQL 提供程序 OLE DB) |否|  
  
## <a name="odbc-data-sources"></a>ODBC 数据源  
 脚本编写和非脚本 ADO 代码之间的一个明显区别是 ODBC 数据源（如果使用）。 对于非脚本应用程序，可以在 ODBC 数据源管理器中创建一个用户 DSN。 对于在 IIS 下运行的脚本，必须创建系统 DSN;否则，您的脚本将无法识别您创建的数据源。 这适用于通过 microsoft IIS 使用适用于 ODBC 的 Microsoft OLE DB 提供程序的任何 ADO 脚本应用程序。  
  
## <a name="referencing-the-ado-library"></a>引用 ADO 库  
 不适用于脚本语言。  
  
## <a name="handling-events"></a>处理事件  
 不适用于脚本语言。  
  
 以下主题包含有关使用 ADO 和脚本语言的更具体的信息：  
  
-   [VBScript ADO 编程](./vbscript-ado-programming.md)  
  
-   [JScript ADO 编程](./jscript-ado-programming.md)  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft ActiveX 数据对象 (ADO) ](../../microsoft-activex-data-objects-ado.md)   
 [将 ADO 与 Microsoft Visual Basic 一起使用](./using-ado-with-microsoft-visual-basic.md)   
 [配合使用 ADO 与 Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md)