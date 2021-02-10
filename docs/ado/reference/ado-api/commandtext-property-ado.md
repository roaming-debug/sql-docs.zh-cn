---
description: CommandText 属性 (ADO)
title: " (ADO) 的 CommandText 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandText
helpviewer_keywords:
- CommandText property [ADO]
ms.assetid: 4dd7e82a-8da5-4a4e-b439-11a29286fa0e
author: rothja
ms.author: jroth
ms.openlocfilehash: fb418696392fe250bd659ba68306df10a7c36a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026612"
---
# <a name="commandtext-property-ado"></a>CommandText 属性 (ADO)
指示要对提供程序发出的命令的文本。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 获取或设置一个 **字符串** 值，该值包含提供程序命令，如 SQL 语句、表名、相对 URL 或存储过程调用。 默认为空字符串 ("")。  
  
## <a name="remarks"></a>备注  
 使用 **CommandText** 属性可以设置或返回 [命令](./command-object-ado.md) 对象表示的命令的文本。 通常，这将是一条 SQL 语句，但也可以是提供程序所识别的任何其他类型的命令语句，如存储过程调用。 SQL 语句必须是提供程序的查询处理器支持的特定方言或版本。  
  
 如果在设置 **CommandText** 属性时，**命令** 对象的 "已 [准备](./prepared-property-ado.md)" 属性设置为 " **True** "，并且 **命令** 对象绑定到打开的连接，则 ADO 将准备该查询 (即，当调用 [Execute](./execute-method-ado-command.md)或 [open](./open-method-ado-connection.md)方法时，由提供程序存储的编译形式的查询) 。  
  
 根据 [CommandType](./commandtype-property-ado.md) 属性设置，ADO 可能会改变 **CommandText** 属性。 您可以随时读取 **CommandText** 属性，以查看 ADO 在执行过程中将使用的实际命令文本。  
  
 使用 **CommandText** 属性可以设置或返回一个相对 URL，该 URL 指定一个资源，如文件或目录。 资源相对于由绝对 URL 显式指定的位置，或由打开的 [连接](./connection-object-ado.md) 对象隐式。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [命令对象 (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VB) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VC + +) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Requery 方法](./requery-method.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (JScript) ](./activeconnection-commandtext-timeout-type-size-example-jscript.md)