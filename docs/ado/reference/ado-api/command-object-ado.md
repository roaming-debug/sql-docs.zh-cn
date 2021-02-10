---
description: 命令对象 (ADO)
title: " (ADO) 的命令对象 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command
helpviewer_keywords:
- Command object [ADO]
ms.assetid: a02c22fb-542d-465e-a629-30fd59dcbebf
author: rothja
ms.author: jroth
ms.openlocfilehash: c8cb910eef7ef810b444813c1e6678b745c524e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026953"
---
# <a name="command-object-ado"></a>命令对象 (ADO)
定义要对数据源执行的特定命令。  
  
## <a name="remarks"></a>备注  
 使用 **Command** 对象可以查询数据库并返回记录 [集](./recordset-object-ado.md) 对象中的记录，执行大容量操作或操作数据库的结构。 根据提供程序的功能，在引用某些 **命令** 集、方法或属性时，可能会生成错误。  
  
 使用 **Command** 对象的集合、方法和属性，可以执行以下操作：  
  
-   定义命令的可执行文本 (例如，使用 [CommandText](./commandtext-property-ado.md) 属性) SQL 语句。 或者，对于除了简单字符串以外的命令或查询结构 (例如，XML 模板查询) 用 [CommandStream](./commandstream-property-ado.md) 属性定义该命令。  
  
-   根据需要，还可以使用 [方言](./dialect-property.md)属性指示 **CommandText** 或 **CommandStream** 中使用的命令方言。  
  
-   用 [参数](./parameter-object.md) 对象和 [参数](./parameters-collection-ado.md) 集合定义参数化查询或存储过程参数。  
  
-   指示是否应将参数名称传递到带有 [NamedParameters](./namedparameters-property-ado.md) 属性的提供程序。  
  
-   执行命令并返回 **Recordset** 对象（如果适用于 [Execute](./execute-method-ado-command.md) 方法）。  
  
-   在执行之前指定带有 [CommandType](./commandtype-property-ado.md) 属性的命令类型，以优化性能。  
  
-   控制提供程序在执行之前是否保存 [已准备的](./prepared-property-ado.md) (或编译的) 版本的命令。  
  
-   使用 [CommandTimeout](./commandtimeout-property-ado.md) 属性设置提供程序等待命令执行的秒数。  
  
-   通过设置其 [ActiveConnection](./activeconnection-property-ado.md)属性，将打开的连接与 **命令** 对象相关联。  
  
-   设置 [名称](./name-property-ado.md) 属性以将 **命令** 对象标识为关联的 [连接](./connection-object-ado.md) 对象上的方法。  
  
-   将 **命令** 对象传递到 **记录集** 的 [Source](./source-property-ado-recordset.md)属性以获取数据。  
  
-   使用 [Properties](./properties-collection-ado.md) 集合访问特定于提供程序的特性。  
  
> [!NOTE]
>  若要在不使用 **命令** 对象的情况下执行查询，请将查询字符串传递到 **连接** 对象的 [execute](./execute-method-ado-connection.md)方法或 **Recordset** 对象的 [Open](./open-method-ado-recordset.md)方法。 但是，当你想要保留命令文本并重新执行它，或使用查询参数时，需要使用 **命令** 对象。  
  
 若要创建独立于先前定义的 **连接** 对象的 **命令** 对象，请将其 **ActiveConnection** 属性设置为有效的连接字符串。 ADO 仍会创建一个 **连接** 对象，但不会将该对象分配给对象变量。 但是，如果要将多个 **命令** 对象与相同连接相关联，则应显式创建并打开 **连接** 对象;这会将 **连接** 对象分配给对象变量。 请确保 **连接** 对象已成功打开，然后将其分配给 **Command** 对象的 **ActiveConnection** 属性，因为分配关闭的 **连接** 对象会导致错误。 如果没有将 **命令** 对象的 **ActiveConnection** 属性设置为此对象变量，则 ADO 将为每个 **命令** 对象创建一个新的 **连接** 对象，即使使用相同的连接字符串。  
  
 若要执行 **命令**，请使用关联的 **连接** 对象上的 [名称](./name-property-ado.md)属性来调用它。 **命令** 必须将其 **ActiveConnection** 属性设置为 **Connection** 对象。 如果 **命令** 包含参数，则将其值作为参数传递给方法。  
  
 如果对同一连接执行了两个或多个 **命令** 对象，并且其中一个 **命令** 对象是带有 output 参数的存储过程，则会发生错误。 若要执行每个 **命令** 对象，请使用单独的连接或断开所有其他 **命令** 对象与连接的连接。  
  
 **Parameters** 集合是 **命令** 对象的默认成员。 因此，下面两个代码语句是等效的。  
  
```  
objCmd.Parameters.Item(0)  
objCmd(0)  
```  
  
-   **命令** 对象对于脚本编写是不安全的。  
  
 本部分包含以下主题。  
  
-   [命令对象属性、方法和事件](./command-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO) 的连接对象 (](./connection-object-ado.md)   
 [ADO) 的参数集合 (](./parameters-collection-ado.md)   
 [ADO)  (属性集合 ](./properties-collection-ado.md)   
 [附录 A：提供程序](../../guide/appendixes/appendix-a-providers.md)