---
description: ConnectionString 属性 (ADO)
title: " (ADO) 的 ConnectionString 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionString
helpviewer_keywords:
- ConnectionString property [ADO]
ms.assetid: 3be75b75-4d36-4479-ab64-9a456869252a
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f8fd13b1f00e7ccebf3b334573948d76aeb7242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034637"
---
# <a name="connectionstring-property-ado"></a>ConnectionString 属性 (ADO)
指示用于建立与数据源的连接的信息。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值。  
  
## <a name="remarks"></a>备注  
 使用 **ConnectionString** 属性来指定数据源，方法是传递包含一系列由分号分隔的 *参数* *= 值* 语句的详细连接字符串。  
  
 ADO 支持 **ConnectionString** 属性的五个参数;任何其他参数将直接传递给提供程序，而无需 ADO 处理。 ADO 支持的参数如下所示。  
  
|参数|说明|  
|--------------|-----------------|  
|*Provider =*|指定要用于连接的访问接口的名称。|  
|*文件名 =*|指定特定于提供程序的文件的名称 (例如，保留的数据源对象) 包含预设的连接信息。|  
|*远程提供程序 =*|指定打开客户端连接时要使用的提供程序的名称。 仅 (远程数据服务。 ) |  
|*远程服务器 =*|指定打开客户端连接时要使用的服务器的路径名称。 仅 (远程数据服务。 ) |  
|*URL =*|将连接字符串指定为标识资源的绝对 URL，如文件或目录。|  
  
 设置 **ConnectionString** 属性并打开 [连接](./connection-object-ado.md) 对象后，提供程序可以更改属性的内容，例如，通过将 ADO 定义的参数名称映射到特定提供程序的等效项。  
  
 **ConnectionString** 属性会自动继承用于 [Open](./open-method-ado-connection.md)方法的 *connectionstring* 参数的值，因此，您可以在 **打开** 方法调用过程中重写当前的 **ConnectionString** 属性。  
  
 由于 *文件名* 参数会使 ADO 加载关联的提供程序，因此不能同时传递 *Provider* 和 *File Name* 参数。  
  
 连接关闭时， **ConnectionString** 属性是可读/写的，当连接处于打开状态时为只读。  
  
 **ConnectionString** 属性中参数的重复项将被忽略。 使用任何自变量的最后一个实例。  
  
> [!NOTE]
>  **远程数据服务使用情况** 当用于客户端 **连接** 对象时， **ConnectionString** 属性只能包括 *远程提供程序* 和 *远程服务器* 参数。  
  
 下表列出了每个 Windows 操作系统的默认 ADO 提供程序：  
  
|默认 ADO 提供程序|Windows 操作系统|  
|--------------------------|------------------------------|  
|MSDASQL<br /><br />  (为了提高源代码的可读性，请在连接字符串中显式指定提供程序名称。 ) |Windows 2000 (32) <br /><br /> Windows XP（32 位）<br /><br /> Windows 2003 Server (32) <br /><br /> Windows Vista（32 位）<br /><br /> Windows Vista Service Pack 1 或更高版本 (32 位和 64) <br /><br /> Windows Vista 之后的 windows 版本 (32 位和64位) |  
|无默认设置。<br /><br /> 当 ADO 应用程序在以下操作系统上运行且未显式指定提供程序时，ADO 将返回以下错误： "ADODB.RECORDSET"。连接：未指定提供程序，并且没有指定的默认提供程序|Windows 2000 (64) <br /><br /> Windows XP（64 位）<br /><br /> Windows 2003 Server (64) <br /><br /> Windows Vista（64 位）|  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ConnectionString、ConnectionTimeout 和 State 属性示例 (VB) ](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [ConnectionString、ConnectionTimeout 和 State 属性示例 (VC + +) ](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [附录 A：提供程序](../../guide/appendixes/appendix-a-providers.md)