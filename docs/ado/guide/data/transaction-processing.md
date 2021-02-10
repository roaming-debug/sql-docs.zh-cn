---
description: 事务处理
title: 事务处理 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [ADO]
- data updates [ADO], transaction processing
- updating data [ADO], transaction processing
- nested transactions [ADO]
ms.assetid: 74ab6706-e2dc-42cb-af77-dbc58a9cf4ce
author: rothja
ms.author: jroth
ms.openlocfilehash: f534d38ea52e942a00e2e95f837b096ed6fef0f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032379"
---
# <a name="transaction-processing"></a>事务处理
用于分隔跨连接执行的一系列数据访问操作的开始和结束的 *事务* 。 根据数据源的事务功能， **连接** 对象还允许您创建和管理事务。 例如，使用 Microsoft OLE DB 提供程序 SQL Server 访问 Microsoft SQL Server 上的数据库时，可以为执行的命令创建多个嵌套事务。  
  
 ADO 可确保对由事务中的操作导致的数据源的更改成功地一起发生或根本不存在。  
  
 如果取消该事务，或如果其中一个操作失败，则结果将与事务中的所有操作都不发生相同。 数据源将保留在事务开始之前。  
  
 ADO 提供以下方法来控制事务： **BeginTrans**、 **CommitTrans** 和 **RollbackTrans**。 如果要将对源数据所做的一系列更改保存为单个单元，请将这些方法与 **连接** 对象一起使用。 例如，若要在帐户之间转移资金，可以从一个金额中减去一个金额，并将相同的金额添加到另一个。 如果任一更新失败，则帐户将不再平衡。 在打开的事务中进行这些更改可以确保所有更改或不会经历任何更改。  
  
> [!NOTE]
>  并非所有提供程序都支持事务。 验证提供程序定义的属性 "**事务 DDL**" 是否出现在 **连接** 对象的 [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) 集合中，指示提供程序支持事务。 如果提供程序不支持事务，则调用这些方法之一将返回错误。  
  
 调用 **BeginTrans** 方法后，该提供程序将不会立即提交所做的更改，直到调用 **CommitTrans** 或 **RollbackTrans** 结束该事务为止。  
  
 调用 **CommitTrans** 方法将保存在连接上打开的事务中所做的更改并结束该事务。 调用 **RollbackTrans** 方法会反转在打开的事务中所做的任何更改，并结束该事务。 如果没有任何打开的事务，则调用任一方法都会生成错误。  
  
 根据 **连接** 对象的 " [属性](../../../ado/reference/ado-api/attributes-property-ado.md) " 属性，调用 **CommitTrans** 或 **RollbackTrans** 方法可能会自动启动一个新事务。 如果 " **属性** " 属性设置为 **adXactCommitRetaining**，则提供程序将在 **CommitTrans** 调用后自动启动新事务。 如果 " **属性** " 属性设置为 **adXactAbortRetaining**，则提供程序将在 **RollbackTrans** 调用后自动启动新事务。  
  
## <a name="transaction-isolation-level"></a>事务隔离级别  
 使用 **IsolationLevel** 属性可以设置 **连接** 对象上事务的隔离级别。 直到下一次调用 [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) 方法时，此设置才会生效。 如果请求的隔离级别不可用，则提供程序可能返回下一个更高的隔离级别。 有关有效值的详细信息，请参阅 ADO 程序员参考中的 **IsolationLevel** 属性。  
  
## <a name="nested-transactions"></a>嵌套事务  
 对于支持嵌套事务的提供程序，在打开的事务内调用 **BeginTrans** 方法将启动一个新的嵌套事务。 返回值指示嵌套的级别：返回值为 "1" 指示你已打开顶级事务 (也就是说，事务未嵌套在另一个事务) 中，"2" 指示你已打开第二级事务 (嵌套在顶级事务) 中的事务等。 调用 **CommitTrans** 或 **RollbackTrans** 只会影响最近打开的事务;必须先关闭或回滚当前事务，然后才能解析任何更高级别的事务。
