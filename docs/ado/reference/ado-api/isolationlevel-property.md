---
description: IsolationLevel 属性
title: IsolationLevel 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::IsolationLevel
helpviewer_keywords:
- IsolationLevel property
ms.assetid: ea84e4b2-fbf2-4eef-b9ce-796b22e21800
author: rothja
ms.author: jroth
ms.openlocfilehash: dc9f116c565321051184d16fa0cdf963cc4bfe0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170931"
---
# <a name="isolationlevel-property"></a>IsolationLevel 属性
指示 [连接](./connection-object-ado.md) 对象的隔离级别。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 [IsolationLevelEnum](./isolationlevelenum.md) 值。 默认值为 **adXactReadCommitted**。  
  
## <a name="remarks"></a>备注  
 使用 **IsolationLevel** 属性可以设置 **连接** 对象的隔离级别。 直到下一次调用 [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 方法时，此设置才会生效。 如果请求的隔离级别不可用，则提供程序可能返回下一个更高的隔离级别，而不更新 **IsolationLevel** 属性。  
  
 **IsolationLevel** 属性是可读/写的。  
  
> [!NOTE]
>  **远程数据服务使用情况** 使用客户端 **连接** 对象时，只能将 **IsolationLevel** 属性设置为 **adXactUnspecified**。 由于用户在客户端缓存上使用断开连接的 **记录集** 对象，因此可能会出现多用户问题。 例如，当两个不同的用户尝试更新同一记录时，远程数据服务只允许将记录更新到 "win" 的用户。 第二个用户的更新请求将失败并出现错误。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [IsolationLevel 和 Mode Properties (VB) ](./isolationlevel-and-mode-properties-example-vb.md)   
 [IsolationLevel 和 Mode Properties (VC + +) ](./isolationlevel-and-mode-properties-example-vc.md)