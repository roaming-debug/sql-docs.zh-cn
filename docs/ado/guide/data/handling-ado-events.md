---
description: 处理 ADO 事件
title: 处理 ADO 事件 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO]
- ADO, events
- event handlers [ADO]
ms.assetid: e9003457-0762-48b3-942f-0820266b158f
author: rothja
ms.author: jroth
ms.openlocfilehash: e3bcc0d4d436afcc10b4d079938adf2bad237d88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037407"
---
# <a name="handling-ado-events"></a>处理 ADO 事件
ADO 事件模型支持某些同步和异步 ADO 操作，这些操作会在操作开始之前或完成之后发出 *事件* 或通知。 事件实际上是对在应用程序中定义的事件处理程序例程的调用。  
  
 如果为操作开始之前发生的一组事件提供处理程序函数或过程，则可以检查或修改传递给操作的参数。 由于尚未执行此操作，因此可以取消该操作或使其完成。  
  
 如果你异步使用 ADO，则操作完成后发生的事件尤其重要。 例如，启动异步 [记录集](../../reference/ado-api/open-method-ado-recordset.md) 的应用程序。当操作结束时，将通过执行完成事件通知打开操作。  
  
 使用 ADO 事件模型会增加应用程序的开销，但比处理异步操作的其他方法（如使用循环监视对象的 [状态](../../reference/ado-api/state-property-ado.md) 属性）提供的灵活性更高。  
  
> [!NOTE]
>  若要处理事件，ADO 需要具有消息泵，或在单线程单元 (STA) 模型中使用。 ADO 事件是通过创建隐藏窗口内部处理的。 当需要激发事件时，ADO 将消息发送到此窗口。 这样做是为了确保将事件发送到连接点上调用 **IConnectionPoint：： Advise** 的线程。 当应该接收通知的线程不会抽取窗口消息时，此体系结构可能会导致问题。 潜在问题包括不会传递给线程的 ADO 事件，全局窗口广播会超时并可能降低整个系统的速度，因为隐藏的窗口不处理消息。 STA 线程通常运行一条消息泵，因此，此问题不会在 STA 线程上自行表现。 但是，MTA 线程通常不会有消息泵，因此该问题通常会在 MTA 线程上表现出自身。  
  
 本部分包含以下主题。  
  
-   [ADO 事件处理程序摘要](./ado-event-handler-summary.md)  
  
-   [事件类型](./types-of-events.md)  
  
-   [事件参数](./event-parameters.md)  
  
-   [事件处理程序的协同工作原理](./how-event-handlers-work-together.md)  
  
-   [ADO 事件实例化（按语言）](./ado-event-instantiation-by-language.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO 事件处理程序摘要](./ado-event-handler-summary.md)   
 [按语言的 ADO 事件实例化](./ado-event-instantiation-by-language.md)   
 [ADO 事件](../../reference/ado-api/ado-events.md)   
 [事件参数](./event-parameters.md)   
 [事件类型](./types-of-events.md)