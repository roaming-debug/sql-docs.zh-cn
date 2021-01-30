---
description: 'BeginTransComplete、CommitTransComplete 和 RollbackTransComplete 事件 (ADO) '
title: " (ADO) 的 BeginTrans、CommitTrans、RollbackTrans 事件 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommitTransComplete
- Connection::BeginTransComplete
- Connection::RollbackTransComplete
- Connection::CommitTransComplete
- RollbackTransComplete
- BeginTransComplete
helpviewer_keywords:
- CommitTransComplete event [ADO]
- RollbackTransComplete event [ADO]
- BeginTransComplete event [ADO]
ms.assetid: ec4e4b38-e9c6-4757-b2ef-4e468ae5f1d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 013df2c56a973d8df6b51434f016ee7642fc4f71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164792"
---
# <a name="begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado"></a>BeginTransComplete、CommitTransComplete 和 RollbackTransComplete 事件 (ADO) 
当 [连接](./connection-object-ado.md) 对象上的关联操作完成执行后，将调用这些事件。  
  
-   **BeginTransComplete** 在 [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 操作后调用。  
  
-   **CommitTransComplete** 在 [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 操作后调用。  
  
-   **RollbackTransComplete** 在 [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 操作后调用。  
  
## <a name="syntax"></a>语法  
  
```  
  
BeginTransComplete TransactionLevel, pError, adStatus, pConnection  
CommitTransComplete pError, adStatus, pConnection  
RollbackTransComplete pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>参数  
 *TransactionLevel*  
 一个 **长整型** 值，该值包含引发此事件的 **BeginTrans** 的新事务级别。  
  
 *pError*  
 一个 [错误](./error-object.md) 对象。 它描述了 EventStatusEnum 的值为 **adStatusErrorsOccurred** 时所发生的错误;否则，不会设置。  
  
 *adStatus*  
 [EventStatusEnum](./eventstatusenum.md)状态值。 当调用这些事件中的任何一个时，如果导致事件的操作成功，则将此参数设置为 **adStatusOK** ; 如果操作失败，则设置为 **adStatusErrorsOccurred** 。  
  
 这些事件可以通过在事件返回之前将此参数设置为 **adStatusUnwantedEvent** 来阻止后续通知。  
  
 *pConnection*  
 发生此事件的 **连接** 对象。  
  
## <a name="remarks"></a>备注  
 在 Visual C++ 中，多个 **连接** 可以共享同一事件处理方法。 方法使用返回的 **连接** 对象来确定导致事件的对象。  
  
 如果 " [属性](./attributes-property-ado.md) " 属性设置为 **adXactCommitRetaining** 或 **adXactAbortRetaining**，则在提交或回滚事务后将启动新事务。 使用 **BeginTransComplete** 事件可忽略第一个事务开始事件之外的所有事件。  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](./ado-events-model-example-vc.md)   
 [BeginTrans、CommitTrans 和 RollbackTrans 方法示例 (VB) ](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)   
 [BeginTrans、CommitTrans 和 RollbackTrans 方法 (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)