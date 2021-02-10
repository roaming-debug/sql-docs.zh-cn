---
description: BeginTrans、CommitTrans 和 RollbackTrans 方法 (ADO)
title: BeginTrans、CommitTrans 和 RollbackTrans 方法 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::raw_RollbackTrans
- Connection15::CommitTrans
- Connection15::raw_CommitTrans
- Connection15::raw_BeginTrans
- Connection15::BeginTrans
- Connection15::RollbackTrans
helpviewer_keywords:
- BeginTrans method [ADO]
- CommitTrans method [ADO]
- RollbackTrans method [ADO]
ms.assetid: d4683472-4120-4236-8640-fa9ae289e23e
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b8eca71f9b9a17b026a084a51c0f02b63e028a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035577"
---
# <a name="begintrans-committrans-and-rollbacktrans-methods-ado"></a>BeginTrans、CommitTrans 和 RollbackTrans 方法 (ADO)
这些事务方法管理 [连接](./connection-object-ado.md) 对象中的事务处理，如下所示：  
  
-   **BeginTrans** 开始新的事务。  
  
-   **CommitTrans** 保存所有更改并结束当前事务。 它还可能会启动新的事务。  
  
-   **RollbackTrans** 取消在当前事务中所做的任何更改并结束事务。 它还可能会启动新的事务。  
  
## <a name="syntax"></a>语法  
  
```  
  
level = object.BeginTrans()  
object.BeginTrans  
object.CommitTrans  
object.RollbackTrans  
```  
  
## <a name="return-value"></a>返回值  
 **BeginTrans** 可作为一个函数调用，该函数返回一个指示事务的嵌套级别的 **长** 变量。  
  
#### <a name="parameters"></a>参数  
 *object*  
 **连接** 对象。  
  
## <a name="connection"></a>连接  
 如果要将对源数据所做的一系列更改保存为单个单元，请将这些方法与 **连接** 对象一起使用。 例如，若要在帐户之间转移资金，可以从一个金额中减去一个金额，并将相同的金额添加到另一个。 如果任一更新失败，则帐户将不再平衡。 在打开的事务中进行这些更改可以确保所有更改或不会经历任何更改。  
  
> [!NOTE]
>  并非所有提供程序都支持事务。 验证提供程序定义的属性 "**事务 DDL**" 是否出现在 **连接** 对象的 [Properties](./properties-collection-ado.md) 集合中，指示提供程序支持事务。 如果提供程序不支持事务，则调用这些方法之一将返回错误。  
  
 调用 **BeginTrans** 方法后，该提供程序将不会立即提交所做的更改，直到调用 **CommitTrans** 或 **RollbackTrans** 结束该事务为止。  
  
 对于支持嵌套事务的提供程序，在打开的事务内调用 **BeginTrans** 方法将启动一个新的嵌套事务。 返回值指示嵌套的级别：返回值为 "1" 指示你已打开顶级事务 (也就是说，事务未嵌套在另一个事务) 中，"2" 指示你已打开第二级事务 (嵌套在顶级事务) 中的事务等。 调用 **CommitTrans** 或 **RollbackTrans** 只会影响最近打开的事务;必须先关闭或回滚当前事务，然后才能解析任何更高级别的事务。  
  
 调用 **CommitTrans** 方法将保存在连接上打开的事务中所做的更改并结束该事务。 调用 **RollbackTrans** 方法会反转在打开的事务中所做的任何更改，并结束该事务。 如果没有任何打开的事务，则调用任一方法都会生成错误。  
  
 根据 **连接** 对象的 " [属性](./attributes-property-ado.md) " 属性，调用 **CommitTrans** 或 **RollbackTrans** 方法可能会自动启动一个新事务。 如果 " **属性** " 属性设置为 **adXactCommitRetaining**，则提供程序将在 **CommitTrans** 调用后自动启动新事务。 如果 " **属性** " 属性设置为 **adXactAbortRetaining**，则提供程序将在 **RollbackTrans** 调用后自动启动新事务。  
  
## <a name="remote-data-service"></a>远程数据服务  
 **BeginTrans**、 **CommitTrans** 和 **RollbackTrans** 方法在客户端 **连接** 对象上不可用。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [BeginTrans、CommitTrans 和 RollbackTrans 方法示例 (VB) ](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [BeginTrans、CommitTrans 和 RollbackTrans 方法示例 (VC + +) ](./begintrans-committrans-and-rollbacktrans-methods-example-vc.md)   
 [Attributes 属性 (ADO)](./attributes-property-ado.md)