---
description: 锁定类型
title: 锁的类型 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AdLockBatchOptimistic [ADO]
- AdLockReadOnly [ADO]
- AdLockUnspecified [ADO]
- locks [ADO], types
- AdLockOptimistic [ADO]
- AdLockPessimistic [ADO]
ms.assetid: 12a978c0-b8a0-4ef0-87f0-a43c13659272
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d72b169247df929d923f3b2362a497647b8f8e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036897"
---
# <a name="types-of-locks"></a>锁定类型
## <a name="adlockbatchoptimistic"></a>adLockBatchOptimistic  
 指示开放式批处理更新。 对于批处理更新模式是必需的。  
  
 许多应用程序一次提取多行，然后需要进行协调更新，包括要插入、更新或删除的整个行集。 对于批处理游标，只需要一个到服务器的往返，从而提高更新性能并减少网络流量。 使用批处理游标库，可以创建静态游标，然后断开与数据源的连接。 此时，您可以对行进行更改，然后重新连接并将更改发布到批处理中的数据源。  
  
## <a name="adlockoptimistic"></a>adLockOptimistic  
 指示提供程序仅在调用 **Update** 方法时使用开放式锁定锁定记录。 这意味着，另一个用户可能会在编辑记录的时间和调用 **Update** 时更改数据，从而导致冲突。 在发生冲突的可能性较低或可以很容易解决冲突的情况下，请使用此锁定类型。  
  
## <a name="adlockpessimistic"></a>adLockPessimistic  
 指示悲观锁定，按记录记录。 提供程序执行必要的操作以确保成功编辑记录，通常是在编辑前直接在数据源中锁定记录。 当然，这意味着在你开始编辑后，其他用户将无法使用这些记录，直到通过调用 Update 释放该锁 **。** 在系统中使用这种类型的锁定，您不能承受对数据的并发更改，例如保留系统中的数据更改。  
  
## <a name="adlockreadonly"></a>adLockReadOnly  
 指示只读记录。 不能更改数据。 只读锁是 "最快" 类型的锁，因为它不需要服务器保持对记录的锁定。  
  
## <a name="adlockunspecified"></a>adLockUnspecified  
 不指定锁的类型。
