---
description: 错误对象
title: Error 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error
helpviewer_keywords:
- error object [ADO]
ms.assetid: a175d453-fa55-4f49-9ede-a26d83177919
author: rothja
ms.author: jroth
ms.openlocfilehash: aa7a9f478398c1e6348513076ac9e9f3d8b815a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025054"
---
# <a name="error-object"></a>错误对象
包含有关与涉及提供程序的单个操作相关的数据访问错误的详细信息。  
  
## <a name="remarks"></a>备注  
 涉及 ADO 对象的任何操作都可能会生成一个或多个提供程序错误。 出现每个错误时，会将一个或多个 **错误** 对象放入 [连接](../../../ado/reference/ado-api/connection-object-ado.md)对象的 [错误](../../../ado/reference/ado-api/errors-collection-ado.md)集合中。 当另一个 ADO 操作生成错误时，将清除 **错误** 集合，并将一组新的 **错误** 对象放置在 **错误** 集合中。  
  
> [!NOTE]
>  每个 **错误** 对象都表示特定提供程序错误，而不是 ADO 错误。 ADO 错误将公开给运行时异常处理机制。 例如，在 Microsoft Visual Basic 中，出现特定于 ADO 的错误时将触发 **出错** 事件，并显示在 **错误** 对象中。 有关 ADO 错误的完整列表，请参阅 [ErrorValueEnum](../../../ado/reference/ado-api/errorvalueenum.md) 主题。  
  
 您可以读取 **错误** 对象的属性，以获取有关每个错误的特定详细信息，包括以下各项：  
  
-   [Description](../../../ado/reference/ado-api/description-property.md)属性，它包含错误的文本。 这是默认属性。  
  
-   [Number](../../../ado/reference/ado-api/number-property-ado.md)属性，它包含错误常量的 **长** 整数值。  
  
-   [Source](../../../ado/reference/ado-api/source-property-ado-error.md)属性，用于标识引发错误的对象。 如果在对数据源发出请求后 **错误** 集合中有多个 **错误** 对象，则此方法特别有用。  
  
-   [SQLState](../../../ado/reference/ado-api/sqlstate-property.md)和[NativeError](../../../ado/reference/ado-api/nativeerror-property-ado.md)属性，提供 SQL 数据源的信息。  
  
 当出现提供程序错误时，它将被置于 **连接** 对象的 **错误** 集合中。 ADO 支持通过单个 ADO 操作返回多个错误，以允许特定于提供程序的错误消息。 若要在错误处理程序中获取此丰富的错误消息，请使用您所使用的语言或环境的适当错误捕获功能，然后使用嵌套循环来枚举 **错误** 集合中每个 **错误** 对象的属性。  
  
> [!NOTE]
>  **Microsoft Visual Basic 和 VBScript 用户** 如果没有有效的 **连接** 对象，你将需要从 **错误** 对象中检索错误信息。  
  
 正如提供程序一样，ADO 会在发出可能产生新的提供程序错误的调用之前清除 **OLE 错误信息** 对象。 但是，仅当提供程序生成新错误或调用 [Clear](../../../ado/reference/ado-api/clear-method-ado.md)方法时，才会清除和填充 **连接** 对象上的 **错误** 集合。  
  
 某些属性和方法会返回 **错误** 集合中显示为 **错误** 对象的警告，但不会停止执行程序。 在对 [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)对象调用 [Resync](../../../ado/reference/ado-api/resync-method.md)、 [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)或 [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md)方法之前，**连接** 对象上的 [Open](../../../ado/reference/ado-api/open-method-ado-connection.md)方法;或在 **记录集** 对象上设置 [Filter](../../../ado/reference/ado-api/filter-property.md)属性，对 **Errors** 集合调用 **Clear** 方法。 通过这种方式，您可以读取 **Errors** 集合的 [Count](../../../ado/reference/ado-api/count-property-ado.md)属性来测试返回的警告。  
  
 **错误** 对象对于脚本编写是不安全的。  
  
 本部分包含以下主题。  
  
-   [错误对象属性、方法和事件](../../../ado/reference/ado-api/error-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) ](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VC + +) ](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [ADO) 的连接对象 (](../../../ado/reference/ado-api/connection-object-ado.md)   
 [ADO)  (收集 ](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [附录 A：提供程序](../../../ado/guide/appendixes/appendix-a-providers.md)
