---
description: 了解自定义文件
title: 了解自定义文件 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- customization file in RDS [ADO]
ms.assetid: 136f74bf-8d86-4a41-be66-c86cbcf81548
author: rothja
ms.author: jroth
ms.openlocfilehash: 1d2b674e5ecc927989873c82303f2b2410c31d31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036057"
---
# <a name="understanding-the-customization-file"></a>了解自定义文件
自定义文件中的每个节标头由方括号 (**[]**) ，其中包含类型和参数。 四种节类型由文本字符串 **connect**、 **sql**、 **userlist** 或 **日志** 指示。 参数是文本字符串、默认值、用户指定的标识符或没有任何内容。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 因此，每个部分都标记有以下节标头之一：  
  
```console
  
[ connect default ] [ connect    
identifier   
] [ sql  default ] [ sql    
identifier   
] [ userlist    
identifier   
] [ logs ]  
  
```  
  
 节标头包含以下部分。  
  
|部分|说明|  
|----------|-----------------|  
|**connect**|修改连接字符串的文本字符串。|  
|**sql**|用于修改命令字符串的文字字符串。|  
|**userlist**|用于修改特定用户的访问权限的文字字符串。|  
|**logs**|指定记录操作错误的日志文件的文本字符串。|  
|**default**|如果未指定或找不到标识符，则使用文本字符串。|  
|*identifier*|与 **连接** 或 **命令** 字符串中的字符串匹配的字符串。<br /><br /> -如果节标头包含 **connect** 并且在连接字符串中找到标识符字符串，请使用此部分。<br />-如果节标头包含 **sql** 并且在命令字符串中找到了标识符字符串，请使用此部分。<br />-如果节标头包含 **userlist** ，并且标识符字符串与 **连接** 部分标识符匹配，请使用此部分。|  
  
 **DataFactory** 调用处理程序，并传递客户端参数。 处理程序将在与相应的节标头中的标识符匹配的客户端参数中搜索整个字符串。 如果找到匹配项，则该部分的内容将应用于客户端参数。  
  
 在下列情况下将使用特定部分：  
  
-   如果客户端连接字符串关键字 "**Data Source =**_value_" 的值部分与 **连接** 部分标识符匹配，则使用 "**连接**" 部分。 
  
-   如果客户端命令字符串包含与 **sql** 部分标识符匹配的字符串，则使用 **sql** 部分。  
  
-   如果没有匹配的标识符，则使用带有默认参数的 **connect** 或 **sql** 部分。  
  
-   如果 **userlist** 节标识符与 **连接** 部分标识符相匹配，则使用 **userlist** 部分。 如果有匹配项， **userlist** 节的内容将应用于 **connect** 部分控制的连接。  
  
-   如果连接或命令字符串中的字符串与任何 **connect** 或 **sql** 节标头中的标识符不匹配，并且没有带默认参数的 **connect** 或 **sql** 节标头，则使用客户端字符串而不进行修改。  
  
-   只要 **DataFactory** 处于操作中，就会使用 "**日志**" 部分。  
  
## <a name="see-also"></a>另请参阅  
 [自定义文件连接部分](./customization-file-connect-section.md)   
 [自定义文件日志部分](./customization-file-logs-section.md)   
 [自定义文件 SQL 部分](./customization-file-sql-section.md)   
 [自定义文件 UserList 部分](./customization-file-userlist-section.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [必需的客户端设置](./required-client-settings.md)   
 [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)