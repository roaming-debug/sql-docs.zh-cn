---
description: 对游标和已准备语句的事务影响
title: 事务对游标和预定义语句的影响 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- rolling back transactions [ODBC]
- committing transactions [ODBC]
- transactions [ODBC], rolling back
- cursors [ODBC], transaction commits or roll backs
- prepared statements [ODBC]
- transactions [ODBC], cursors
ms.assetid: 523e22a2-7b53-4c25-97c1-ef0284aec76e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 495bb8455c3e13b88d2d3ae6b400c5c0f2167604
ms.sourcegitcommit: 0f484f32709a414f05562bbaafeca9a9fc57c9ed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2020
ms.locfileid: "94631673"
---
# <a name="effect-of-transactions-on-cursors-and-prepared-statements"></a>对游标和已准备语句的事务影响
提交或回滚事务会对游标和访问计划产生以下影响之一：  
  
-   所有游标都将关闭，并且将删除该连接上的预定义语句的访问计划，或  
  
-   所有游标都已关闭，并且该连接上的预定义语句的访问计划保持不变，或 
  
-   所有游标都保持打开状态，并且该连接上的预定义语句的访问计划仍保持不变。  
  
 例如，假设数据源出现在此列表中的第一个行为，这是这些行为的最严格限制。 现在假设某个应用程序执行以下操作：  
  
1.  将提交模式设置为手动提交。  
  
2.  在语句1上创建销售订单的结果集。  
  
3.  当用户突出显示订单时，将在报表2的销售订单中创建行的结果集。  
  
4.  当用户更新行时，调用 **SQLExecute** 以执行已准备就绪的已定位 update 语句。  
  
5.  调用 **SQLEndTran** 以提交定位的 update 语句。  
  
 由于数据源的行为，对步骤5中的 **SQLEndTran** 的调用会使其关闭语句1和2上的游标，并删除所有语句的访问计划。 应用程序必须重新创建语句1和2，才能重新创建结果集并 reprepare 语句3上的语句。  
  
 在自动提交模式下， **SQLEndTran** 提交事务以外的其他函数：  
  
-   **SQLExecute** 或 **SQLExecDirect** 在前面的示例中，在步骤4中对 **SQLExecute** 的调用将提交事务。 这会导致数据源关闭语句1和2上的游标，并删除该连接上所有语句上的访问计划。  
  
-   前面的示例中的 **SQLBulkOperations** 或 **SQLSetPos** 假设在步骤4中，应用程序使用语句2上的 SQL_UPDATE 选项调用 **SQLSetPos** ，而不是在语句3上执行定位的 UPDATE 语句。 这会提交一个事务，并导致数据源关闭语句1和2上的游标，并放弃该连接上的所有访问计划。  
  
-   **SQLCloseCursor** 在前面的示例中，假设当用户突出显示不同的销售订单时，应用程序会在为新销售订单创建行的结果之前，在语句2上调用 **SQLCloseCursor** 。 对 **SQLCloseCursor** 的调用将提交创建行结果集的 **SELECT** 语句，并导致数据源在语句1上关闭游标，然后放弃该连接上的所有访问计划。  
  
 应用程序（尤其是基于屏幕的应用程序，用户在其中滚动结果集并更新或删除行）必须小心地围绕此行为编写代码。  
  
 若要确定提交或回滚事务时数据源的行为方式，应用程序需要使用 SQL_CURSOR_COMMIT_BEHAVIOR 和 SQL_CURSOR_ROLLBACK_BEHAVIOR 选项调用 **SQLGetInfo** 。
