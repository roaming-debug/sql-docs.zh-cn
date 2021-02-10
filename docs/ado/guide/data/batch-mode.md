---
description: 批处理模式
title: 批处理模式 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data updates [ADO], batch mode
- batch mode [ADO]
- updating data [ADO], batch mode
ms.assetid: 0cb548e0-fcb4-4c49-98c8-be287911f826
author: rothja
ms.author: jroth
ms.openlocfilehash: ee9e67527b109b36c7ded266086f9efea06e4381
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028022"
---
# <a name="batch-mode"></a>批处理模式
当 **LockType** 属性设置为 **adLockBatchOptimistic** ，并且提供程序支持批处理更新时，批处理模式有效。 某些锁类型设置不可用，具体取决于游标位置。 例如，当 **CursorLocation** 设置为 **adUseClient** 时，悲观锁定类型不可用。 相反，当光标位于服务器上时，提供程序不支持批处理开放式锁定。 只应将批处理更新用于键集或静态游标。  
  
 **UpdateBatch** 方法用于将复制缓冲区中保存的 **记录集** 更改发送到服务器以更新数据源。 在下一部分中，我们将在批处理模式下打开一个 **记录集** ，对复制缓冲区进行更改，然后通过调用 **UpdateBatch** 将更改发送到数据源。  
  
 本节包含下列主题：  
  
-   [发送更新：UpdateBatch 方法](./sending-the-updates-updatebatch-method.md)  
  
-   [筛选更新的记录](./filtering-for-updated-records.md)  
  
-   [处理失败的更新](./dealing-with-failed-updates.md)  
  
-   [检测和解决冲突](./detecting-and-resolving-conflicts.md)  
  
-   [断开连接并重新连接记录集](./disconnecting-and-reconnecting-the-recordset.md)  
  
-   [更新已加入结果：唯一表](./updating-joined-results-unique-table.md)