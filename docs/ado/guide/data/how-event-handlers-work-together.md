---
description: 事件处理程序的协同工作原理
title: 事件处理程序如何协同工作 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- unpaired event handlers [ADO]
- paired event handlers [ADO]
- single event handlers [ADO]
- event handlers [ADO]
- multiple object event handlers [ADO]
ms.assetid: a86c8a02-dd69-420d-8a47-0188b339858d
author: rothja
ms.author: jroth
ms.openlocfilehash: dd6950ec37ddf974470e29ae554603723b299ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037347"
---
# <a name="how-event-handlers-work-together"></a>事件处理程序的协同工作原理
除非您正在 Visual Basic 进行编程，否则不管是否确实处理所有事件，都必须实现 **连接** 事件和 **记录集** 事件的所有事件处理程序。 您必须执行的实现工作量取决于编程语言。 有关详细信息，请参阅 [按语言的 ADO 事件实例化](./ado-event-instantiation-by-language.md)。  
  
## <a name="paired-event-handlers"></a>配对事件处理程序  
 每个都将有一个关联的 **完整** 事件处理程序。 例如，当应用程序更改字段的值时，将调用 **WillChangeField** 事件处理程序。 如果更改是可接受的，则应用程序保持 **adStatus** 参数不变，并执行操作。 操作完成后， **FieldChangeComplete** 事件会通知应用程序操作已完成。 如果成功完成， **adStatus** 将包含 **adStatusOK**;否则， **adStatus** 将包含 **adStatusErrorsOccurred** ，必须检查 **错误** 对象以确定错误的原因。  
  
 调用 **WillChangeField** 时，你可能会确定不应进行此更改。 在这种情况下，将 **adStatus** 设置为 **adStatusCancel。** 操作已取消， **FieldChangeComplete** 事件接收到 **adStatusErrorsOccurred** 的 **adStatus** 值。 **Error** 对象包含 **AdErrOperationCancelled** ，以便 **FieldChangeComplete** 处理程序知道该操作已取消。 但是，您需要在更改 **adStatus** 参数之前检查其值，因为如果将 **adStatus** **设置为 "** adStatusCantDeny"，则将该参数设置为 "  " 时，不会产生任何影响。  
  
 有时操作可能会引发多个事件。 例如， **Recordset** 对象具有对 **字段** 更改和 **记录** 更改的成对事件。 当应用程序更改 **字段** 的值时，将调用 **WillChangeField** 事件处理程序。 如果它确定操作可以继续，则也会引发 **WillChangeRecord** 事件处理程序。 如果此处理程序还允许事件继续，则会进行更改并调用 **FieldChangeComplete** 和 **RecordChangeComplete** 事件处理程序。 为特定操作调用事件处理程序的顺序未定义，因此应避免编写依赖于按特定顺序调用处理程序的代码。  
  
 在实例中，如果引发了多个事件，则其中一个事件可能会取消挂起的操作。 例如，当应用程序更改 **字段** 的值时，通常会调用 **WillChangeField** 和 **WillChangeRecord** 事件处理程序。 但是，如果在第一个事件处理程序中取消了该操作，则会立即通过 **adStatusOperationCancelled** 调用其关联的 **完整** 处理程序。 从不调用第二个处理程序。 但是，如果第一个事件处理程序允许事件继续，则将调用另一个事件处理程序。 如果它取消了该操作，则将按前面的示例中所示调用两个 **完整** 事件。  
  
## <a name="unpaired-event-handlers"></a>未成对的事件处理程序  
 只要传递到事件的状态不是 **adStatusCantDeny**，您可以通过在 *status* 参数中返回 **adStatusUnwantedEvent** 来关闭任何事件的事件通知。 例如，第一次调用 **完整** 的事件处理程序时，可以返回 **adStatusUnwantedEvent**。 稍后 **将只接收** 事件。 但是，某些事件可能会出于多种原因而触发。 在这种情况下，该事件将具有 *Reason* 参数。 当你返回 **adStatusUnwantedEvent** 时，只有在此事件发生时才会停止接收该事件的通知。 换句话说，你可能会收到可能触发事件的每个可能原因的通知。  
  
 如果要检查将在操作中使用的参数 **，则可以使用 Single。** 您可以修改这些操作参数，也可以取消该操作。  
  
 或者，将 " **完成** 事件通知" 保持为启用状态。 调用第一个事件处理程序时，将返回 **adStatusUnwantedEvent**。 随后将仅接收 **完整** 事件。  
  
 单个 **完整** 事件处理程序对于管理异步操作非常有用。 每个异步操作都有一个适当的 **完整** 事件。  
  
 例如，填充大型 [Recordset](../../reference/ado-api/recordset-object-ado.md) 对象可能需要很长时间。 如果你的应用程序已适当编写，你可以启动 `Recordset.Open(...,adAsyncExecute)` 操作并继续其他处理。 当 **记录集** 由 **ExecuteComplete** 事件填充时，最终会收到通知。  
  
## <a name="single-event-handlers-and-multiple-objects"></a>单个事件处理程序和多个对象  
 编程语言（例如 Microsoft Visual C++®）的灵活性使您能够从多个对象处理一个事件处理程序事件。 例如，你可能有一个从多个 **连接** 对象的 "**断开连接**" 事件处理程序事件。 如果某个连接结束，则会调用 " **断开连接** " 事件处理程序。 由于事件处理程序对象参数将设置为相应的 **连接** 对象，因此可以告知哪个连接引发了事件。  
  
> [!NOTE]
>  此方法不能在 Visual Basic 中使用，因为该语言只能将一个对象关联到一个事件处理程序。  
  
## <a name="see-also"></a>另请参阅  
 [ADO 事件处理程序摘要](./ado-event-handler-summary.md)   
 [按语言的 ADO 事件实例化](./ado-event-instantiation-by-language.md)   
 [事件参数](./event-parameters.md)   
 [事件类型](./types-of-events.md)