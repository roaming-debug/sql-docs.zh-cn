---
description: ADO 事件实例化：ADO 和 WFC
title: ADO 事件实例化： ADO 和 WFC |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 9ee4be21-657b-407a-afa4-0b27a6b096ce
author: rothja
ms.author: jroth
ms.openlocfilehash: b7ed585adcfc9542e3d396f2b0fd1a82d65cdcf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028349"
---
# <a name="ado-event-instantiation-ado-and-wfc"></a>ADO 事件实例化：ADO 和 WFC
适用于 Windows 基础类 (ado/WFC) 的 ADO 构建于 ADO 事件模型上，并提供简化的应用程序编程接口。 通常情况下，ADO/WFC 会截获 ADO 事件，将事件参数合并为单个事件类，然后调用事件处理程序。  
  
### <a name="to-use-ado-events-in-adowfc"></a>在 ADO/WFC 中使用 ADO 事件  
  
1.  定义自己的事件处理程序以处理事件。 例如，如果要处理 **ConnectionEvent** 系列中的 **ConnectComplete** 事件，可以使用以下代码：  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    {  
        System.out.println("onConnectComplete:" + e);  
    }  
    ```  
  
2.  定义用于表示事件处理程序的处理程序对象。 对于 **ConnectionEvent** 类型的事件，处理程序对象应为数据类型 **ConnectEventHandler** ，对于 **RecordsetEvent** 类型的事件，则为数据类型 **RecordsetEventHandler** 。 例如，为 **ConnectComplete** 事件处理程序编写以下代码：  
  
    ```  
    ConnectionEventHandler handler =   
        new ConnectionEventHandler(this, "onConnectComplete");  
    ```  
  
     **ConnectionEventHandler** 构造函数的第一个参数是对类的引用，该类包含第二个参数中命名的事件处理程序。  
  
3.  将事件处理程序添加到指定用于处理特定事件类型的处理程序列表。 使用带有名称的方法，如 **加载**_项 (_ *处理程序*) 。  
  
4.  ADO/WFC 在内部实现所有 ADO 事件处理程序。 因此，由 **连接** 或 **记录集** 操作导致的事件被 ADO/WFC 事件处理程序截获。  
  
     ADO/WFC 事件处理程序在 ado/WFC **ConnectionEvent** 类的实例中传递 ado **ConnectionEvent** 参数，或在 ado/wfc **RecordsetEvent** 类的实例中传递 ado **RecordsetEvent** 参数。 这些 ADO/WFC 类合并了 ADO 事件参数;也就是说，每个 ADO/WFC 类对于所有 ADO **ConnectionEvent** 或 **RecordsetEvent** 方法中的每个唯一参数都包含一个数据成员。  
  
5.  ADO/WFC 随后将调用事件处理程序和 ADO/WFC 事件对象。 例如， **onConnectComplete** 处理程序的签名如下所示：  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    ```  
  
     第一个参数是发送事件 ([连接](../../reference/ado-api/connection-object-ado.md) 或 [记录集](../../reference/ado-api/recordset-object-ado.md)) 的对象的类型，第二个参数是 ADO/WFC 事件对象 (**ConnectionEvent** 或 **RecordsetEvent**) 。  
  
     事件处理程序的签名比 ADO 事件简单。 但是，您仍然必须了解 ADO 事件模型，才能了解哪些参数适用于事件以及如何做出响应。  
  
6.  从事件处理程序返回到 ADO 事件的 ADO/WFC 处理程序。 ADO/WFC 将相关的 ADO/WFC 事件数据成员复制回 ADO 事件参数，然后 ADO 事件处理程序返回。  
  
7.  完成处理后，从 ADO/WFC 事件处理程序列表中删除处理程序。 使用具有名称（如 **removeOn**_事件_ 名称） (*处理程序*) 的方法。  
  
## <a name="see-also"></a>另请参阅  
 [ADO 事件处理程序摘要](./ado-event-handler-summary.md)   
 [ADO-WFC 语法索引](../../reference/ado-api/ado-wfc-syntax-index.md)   
 [事件参数](./event-parameters.md)   
 [事件处理程序如何协同工作](./how-event-handlers-work-together.md)   
 [事件类型](./types-of-events.md)