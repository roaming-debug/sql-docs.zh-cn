---
description: ADO 连接和记录集事件
title: ADO 事件处理程序摘要 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- event handlers [ADO]
ms.assetid: b34f4472-5e04-4a2c-ab64-38d6eca31a69
author: rothja
ms.author: jroth
ms.openlocfilehash: a447c149769d892149096c980b35e77e49de05da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028359"
---
# <a name="ado-connection-and-recordset-events"></a>ADO 连接和记录集事件
两个 ADO 对象可以引发事件： [连接](../../reference/ado-api/connection-object-ado.md) 对象和 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象。 **ConnectionEvent** 系列适用于 **连接** 对象上的操作， **RecordsetEvent** 系列与 **Recordset** 对象上的操作相关。

-   **连接事件**：当连接上的事务开始、提交或回滚时将发出事件;执行 [命令](../../reference/ado-api/command-object-ado.md) 时;当 **连接事件** 操作期间出现警告时;或 **连接** 的开始或结束时间。

-   **记录集事件**：事件是围绕异步提取操作发出的，以及当您在 **recordset** 对象的行中导航时， **更改记录集** 的行中的字段、更改 **记录集中** 的行、打开包含服务器端游标的 **记录集** 、关闭 **记录** 集或在 **记录集中** 进行任何更改。

 下表汇总了事件及其说明。

|ConnectionEvent|说明|
|---------------------|-----------------|
|[BeginTransComplete, CommitTransComplete, RollbackTransComplete](../../reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|**事务管理** -通知：连接上的当前事务已启动、提交或回滚。|
|[WillConnect](../../reference/ado-api/willconnect-event-ado.md)、 [ConnectComplete、Disconnect](../../reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|**连接管理** -通知当前连接将开始、已启动或已结束。|
|[WillExecute](../../reference/ado-api/willexecute-event-ado.md)、 [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md)|**命令执行管理** -通知：连接上的当前命令执行将启动或已结束。|
|[InfoMessage](../../reference/ado-api/infomessage-event-ado.md)|**信息** -有关当前操作的其他信息，请通知。|

|RecordsetEvent|说明|
|--------------------|-----------------|
|[FetchProgress](../../reference/ado-api/fetchprogress-event-ado.md)、 [FetchComplete](../../reference/ado-api/fetchcomplete-event-ado.md)|**检索状态** -数据检索操作的进度通知，或检索操作已完成。 仅当使用客户端游标打开 **记录集** 时，这些事件才可用。|
|[WillChangeField, FieldChangeComplete](../../reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|**字段更改管理** -通知当前字段的值将更改或已更改。|
|[WillMove、MoveComplete](../../reference/ado-api/willmove-and-movecomplete-events-ado.md)、 [EndOfRecordset](../../reference/ado-api/endofrecordset-event-ado.md)|**导航管理** -通知： **记录集中** 的当前行位置将发生更改、更改或已到达 **记录集** 的结尾。|
|[WillChangeRecord, RecordChangeComplete](../../reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|**行更改管理** -通知 **记录集** 的当前行中的某些内容将发生更改或已更改。|
|[WillChangeRecordset, RecordsetChangeComplete](../../reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|**记录集更改管理** -通知当前 **记录集中** 的某些内容将发生更改或已更改。|

## <a name="see-also"></a>另请参阅
 [按语言 ado 事件实例化的 ado](./ado-event-instantiation-by-language.md) [](../../reference/ado-api/ado-events.md)事件[事件参数](./event-parameters.md)[事件处理程序如何协同工作](./how-event-handlers-work-together.md)事件[类型](./types-of-events.md)