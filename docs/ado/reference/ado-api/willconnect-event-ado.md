---
description: WillConnect 事件 (ADO)
title: WillConnect 事件 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- WillConnect
- Connection::WillConnect
helpviewer_keywords:
- WillConnect event [ADO]
ms.assetid: da561d58-eb58-446c-a4fd-1838c76073c0
author: rothja
ms.author: jroth
ms.openlocfilehash: d135747f0599f646035d2e70e768bf755ac6fa21
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056102"
---
# <a name="willconnect-event-ado"></a>WillConnect 事件 (ADO)
在连接开始之前调用 **WillConnect** 事件。  
  
 **适用于：** [ADO (连接对象)](./connection-object-ado.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
WillConnect ConnectionString, UserID, Password, Options, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>参数  
 *ConnectionString*  
 一个 **字符串** ，其中包含挂起连接的连接信息。  
  
 *UserID*  
 包含挂起连接的用户名的 **字符串** 。  
  
 *密码*  
 一个 **字符串** ，其中包含挂起的连接的密码。  
  
 *选项*  
 一个 **长整型** 值，该值指示提供程序应如何计算 *ConnectionString*。 唯一的选择是 **adAsyncOpen**。  
  
 *adStatus*  
 [EventStatusEnum](./eventstatusenum.md)状态值。  
  
 调用此事件时，默认情况下，此参数设置为 **adStatusOK** 。 如果事件无法请求取消挂起操作，则将其设置为 **adStatusCantDeny** 。  
  
 在此事件返回之前，将此参数设置为 **adStatusUnwantedEvent** 以防止后续通知。 将此参数设置为 **adStatusCancel** 可请求导致取消此通知的连接操作。  
  
 *pConnection*  
 此事件通知适用的 [连接](./connection-object-ado.md) 对象。 **WillConnect** 事件处理程序对 **连接** 参数的更改将不会对 **连接** 产生任何影响。  
  
## <a name="remarks"></a>备注  
 调用 **WillConnect** 时， *ConnectionString*、 *UserID*、 *Password* 和 *Options* 参数设置为由导致此事件的操作所建立的值 (挂起的连接) ，并且可以在事件返回前更改。 **WillConnect** 可能会返回挂起的连接被取消的请求。  
  
 当取消此事件时，将调用 **ConnectComplete** ，并将其 *adStatus* 参数设置为 **adStatusErrorsOccurred**。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)