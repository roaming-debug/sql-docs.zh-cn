---
description: MSSQLSERVER_1205
title: MSSQLSERVER_1205 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1205 (Database Engine error)
ms.assetid: 9fe3f67c-df3c-4642-a3a4-ccc0e138b632
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8ce9d2d99d3db0fc18586b9292876d2e4e6c985b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197166"
---
# <a name="mssqlserver_1205"></a>MSSQLSERVER_1205
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|1205|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LK_VICTIM|  
|消息正文|事务(进程 ID %d)与另一个进程被死锁在 %.*ls 资源上，并且已被选作死锁牺牲品。 重新运行该事务。|  
  
## <a name="explanation"></a>说明

在单独的事务上以相互冲突的顺序访问资源，从而导致[死锁](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks)。 例如：  
  
- Transaction1 更新 Table1.Row1，而 Transaction2 更新 Table2.Row2 
- Transaction1 尝试更新 Table2.Row2，但因 Transaction2 尚未提交而被阻止  
- Transaction2 现在尝试更新 Table1.Row1，但因 Transaction1 尚未提交而被阻止
- 之所以出现死锁，是因为 Transaction1 在等待 Transaction2 完成，但 Transaction2 在等待 Transaction1 完成。  
  
系统将检测到此死锁，并选择其中一个涉及的事务作为“牺牲品”。 然后，它将发出此错误消息，回滚该牺牲品的事务。  有关详细信息，请参阅[死锁](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks)。

## <a name="user-action"></a>用户操作  

重新执行事务。 您还可以修订应用程序以避免死锁。 可以重试被选作牺牲品的事务，而且很可能成功，具体取决于同时执行的操作。  
  
为防止或避免出现死锁，请考虑让所有的事务按相同顺序（先访问 Table1，然后访问 Table2）访问行 。 这样，虽然可能会出现阻止，但可避免出现死锁。  
  
有关详细信息，请参阅[处理死锁](../sql-server-transaction-locking-and-row-versioning-guide.md?#handling-deadlocks)和[将死锁减至最少](../sql-server-transaction-locking-and-row-versioning-guide.md#deadlock_minimizing)。
