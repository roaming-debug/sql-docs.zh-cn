---
description: 游标和锁定特征
title: 光标和锁定特征 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- locks [ADO], characteristics
- adOpenDynamic [ADO]
- cursors [ADO], characteristics
ms.assetid: 459c29cb-4230-42bf-8cc2-f3132ccc7aba
author: rothja
ms.author: jroth
ms.openlocfilehash: cecbdd12fd98e6f58d07cfe30c6c0b8a044ed15e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037679"
---
# <a name="cursor-and-lock-characteristics"></a>游标和锁定特征
尽管游标的特征取决于提供程序的功能，但以下优点和缺点通常适用于各种类型的游标和锁。  
  
|光标或锁定类型|优点|缺点|  
|-------------------------|----------------|-------------------|  
|**adOpenForwardOnly**|-资源不足要求|-无法向后滚动<br />-无数据并发|  
|**adOpenStatic**|-可滚动|-无数据并发|  
|**adOpenKeyset**|-一些数据并发<br />-可滚动|-资源要求更高<br />-在断开连接的情况下不可用|  
|**adOpenDynamic**|-高数据并发<br />-可滚动|-最高资源需求<br />-在断开连接的情况下不可用|  
|**adLockReadOnly**|-资源不足要求<br />-高度可伸缩|-数据无法通过游标进行更新|  
|**adLockBatchOptimistic**|-批更新<br />-允许断开连接的方案<br />-其他用户能够访问数据|-多个用户可以同时更改数据|  
|**adLockPessimistic**|-锁定时其他用户无法更改数据|-阻止其他用户在锁定时访问数据|  
|**adLockOptimistic**|-其他用户能够访问数据|-多个用户可以同时更改数据|
