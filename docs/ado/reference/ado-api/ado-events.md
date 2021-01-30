---
description: ADO 事件
title: ADO 事件 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- events [ADO]
- ADO, events
ms.assetid: 0ded5ad9-8f83-4224-95af-38512783b972
author: rothja
ms.author: jroth
ms.openlocfilehash: 343d24455490674a9c926d94a902906992679a2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164929"
---
# <a name="ado-events"></a>ADO 事件

|事件|说明|  
|-|-|  
|[BeginTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|在 **BeginTrans** 操作后调用。|  
|[CommitTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|在 **CommitTrans** 操作后调用。|  
|[ConnectComplete](./connectcomplete-and-disconnect-events-ado.md)|在连接开始后调用。|  
|[断开连接](./connectcomplete-and-disconnect-events-ado.md)|在连接结束后调用。|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|尝试移到超过 **记录集** 末尾的行时调用。|  
|[ExecuteComplete](./executecomplete-event-ado.md)|执行完命令后调用。|  
|[FetchComplete](./fetchcomplete-event-ado.md)|在将较长异步操作中的所有记录都检索到 **记录集** 后调用。|  
|[FetchProgress](./fetchprogress-event-ado.md)|在长时间异步操作期间定期调用，以报告当前已检索到 **记录集中** 的行数。|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|在一个或多个 **字段** 对象的值更改后调用。|  
|[InfoMessage](./infomessage-event-ado.md)|在 **ConnectionEvent** 操作期间出现警告时调用。|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|在 **记录集中** 的当前位置发生更改后调用。|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|在一个或多个记录发生更改后调用。|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|在 **记录集** 更改后调用。|  
|[RollbackTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|在 **RollbackTrans** 操作后调用。|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|在挂起操作之前调用，更改 **记录集中** 一个或多个 **字段** 对象的值。|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|在 **记录集** (行) 中的一个或多个记录更改之前调用。|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|在挂起的操作更改 **记录集** 之前调用。|  
|[WillConnect](./willconnect-event-ado.md)|在连接启动之前调用。|  
|[WillExecute](./willexecute-event-ado.md)|在此连接上执行挂起命令之前调用，并为用户给出检查和修改挂起执行参数的机会。|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|在挂起的操作更改 **记录集中** 的当前位置 *之前*，调用 **WillMove** 事件。|  
  
## <a name="see-also"></a>另请参阅  
 [ADO API 参考](./ado-api-reference.md)   
 [ADO 集合](./ado-collections.md)   
 [ADO 动态属性](./ado-dynamic-properties.md)   
 [ADO 枚举常量](./ado-enumerated-constants.md)   
 [附录 B： ADO 错误](../../guide/appendixes/appendix-b-ado-errors.md)   
 [处理 ADO 事件](../../guide/data/handling-ado-events.md)   
 [ADO 方法](./ado-methods.md)   
 [ADO 对象模型](./ado-object-model.md)   
 [ADO 对象和接口](./ado-objects-and-interfaces.md)   
 [ADO 属性](./ado-properties.md)