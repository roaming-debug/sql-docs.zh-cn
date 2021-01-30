---
description: 附录 B：ODBC 状态转换表
title: 附录 B： ODBC 状态转换表 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC]
- transitioning states [ODBC], about state transitions
- state transitions [ODBC], about state transitions
ms.assetid: 15088dbe-896f-4296-b397-02bb3d0ac0fb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 67c14205590ccdf9d20a30f44c13aa2da5abbe8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212566"
---
# <a name="appendix-b-odbc-state-transition-tables"></a>附录 B：ODBC 状态转换表
本附录中的表说明 ODBC 函数如何导致环境、连接、语句和描述符状态的转换。 环境、连接、语句或描述符的状态通常指示何时，使用相应的句柄类型的函数 (环境、连接、语句或描述符) 可以调用。 环境、连接、语句和描述符状态大致重叠，如下图所示。 例如，连接状态 C5、C6 和语句状态 S1 到 S12 的精确重叠与数据源相关，因为事务在不同数据源的不同时间开始，并且描述符状态 D1i (隐式分配的描述符) 取决于与说明符关联的语句的状态，而状态 D1e (显式分配的描述符) 与任何语句的状态无关。 有关每种状态的说明，请参阅本附录后面的 [环境转换](../../../odbc/reference/appendixes/environment-transitions.md)、 [连接转换](../../../odbc/reference/appendixes/connection-transitions.md)、 [语句转换](../../../odbc/reference/appendixes/statement-transitions.md)和 [描述符转换](../../../odbc/reference/appendixes/descriptor-transitions.md)。  
  
 环境和连接状态重叠如下：  
  
 ![环境和连接状态重叠](../../../odbc/reference/appendixes/media/app01.gif "app01")  
  
 连接和语句状态重叠如下：  
  
 ![连接和语句状态重叠](../../../odbc/reference/appendixes/media/app02.gif "app02")  
  
 语句和描述符状态重叠，如下所示：  
  
 ![语句和描述符状态重叠](../../../odbc/reference/appendixes/media/app03.gif "app03")  
  
 连接和描述符状态重叠如下：  
  
 ![连接和描述符状态重叠](../../../odbc/reference/appendixes/media/app04.gif "app04")  
  
 转换表中的每个条目可以为下列值之一：  
  
-   **--** -执行函数后，状态将保持不变。  
  
-   **E**  

     **_n_** 、 **C_n_**、 **S_n_** 或 **D_n_** -环境、连接、语句或描述符状态移动到指定状态。  
 
-   **(IH)** -传递到函数的句柄无效。 如果句柄为空句柄，或者为错误类型的有效句柄，例如，在需要语句句柄时传递了一个连接句柄，则该函数将返回 SQL_INVALID_HANDLE;否则，该行为是未定义的，并且可能是致命的。 仅当在指定的状态下调用函数时，才会显示此错误。 此错误不会更改状态，并且始终由驱动程序管理器检测到，如括号所示。  
  
-   **NS** -下一个状态。 语句转换与语句不会经历异步状态相同。 例如，假设创建结果集的语句从状态 S1 进入状态 S11，因为 **SQLExecDirect** 返回 SQL_STILL_EXECUTING。 State S11 中的 NS 表示法表示语句的转换与状态 S1 中创建结果集的语句的转换相同。 如果 **SQLExecDirect** 返回错误，则语句将保留在 S1 中;如果成功，语句将移到 "状态 S5";如果它需要数据，则该语句将移到 state S8;如果仍在执行，它将保持 S11 状态。  

-   **_Xxxxx_**  或 **(*xxxxx*)** -与转换表相关的 SQLSTATE;驱动程序管理器检测到的 SQLSTATEs 括在括号中。 函数返回 SQL_ERROR 和指定的 SQLSTATE，但状态不会更改。 例如，如果在 **SQLPrepare** 之前调用了 **SQLExecute** ，则会返回 SQLSTATE HY010 (函数序列错误) 。  

> [!NOTE]  
>  这些表不显示与不更改状态的转换表无关的错误。 例如，当在环境状态 E1 中调用 **SQLAllocHandle** 并返回 SQLSTATE HY001 (内存分配错误) 时，环境仍处于状态 E1;这不会显示在 **SQLAllocHandle** 的环境转换表中。  
  
 如果环境、连接、语句或描述符可以移至多个状态，则会显示每个可能的状态，并且一个或多个脚注说明了每个转换发生的条件。 以下脚注可能出现在任何表中。  
  
|交叉|含义|  
|--------------|-------------|  
|b|前后。 游标位于结果集的开头之前或结果集结束之后。|  
|c|当前函数。 当前函数以异步方式执行。|  
|d|需要数据。 函数返回 SQL_NEED_DATA。|  
|e|错误。 函数返回 SQL_ERROR。|  
|i|行无效。 游标定位在结果集中的某一行上，并且该行已被删除或在某一操作中发生了错误。 如果行状态数组存在，则该行的行状态数组中的值是 SQL_ROW_DELETED 或 SQL_ROW_ERROR。  (SQL_ATTR_ROW_STATUS_PTR 语句特性指向行状态数组。 ) |  
|nf|未找到。 函数返回 SQL_NO_DATA。 当 **SQLExecDirect**、 **SQLExecute** 或 **SQLParamData** 在执行搜索的 update 或 delete 语句之后 SQL_NO_DATA 返回时，此功能不适用。|  
|np|未准备好。 未准备语句。|  
|nr|没有结果。 语句将不会创建结果集，也不会创建结果集。|  
|o|其他函数。 其他函数是异步执行的。|  
|p|有所. 语句已准备就绪。|  
|r|后果. 语句将创建 (可能为空) 结果集。|  
|s|成功。 函数返回 SQL_SUCCESS_WITH_INFO 或 SQL_SUCCESS。|  
|v|有效的行。 游标定位在结果集中的某一行上，并且该行已成功插入，已成功更新，或者该行上的另一项操作已成功完成。 如果行状态数组存在，则该行的行状态数组中的值为 SQL_ROW_ADDED、SQL_ROW_SUCCESS 或 SQL_ROW_UPDATED。  (SQL_ATTR_ROW_STATUS_PTR 语句特性指向行状态数组。 ) |  
|x|正在执行。 函数返回 SQL_STILL_EXECUTING。|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 在此示例中，当 *HandleType* 为 SQL_HANDLE_ENV 时，环境状态转换表中的行 **SQLFreeHandle** 如下所示。  
  
|步进<br /><br /> 未分配|E1<br /><br /> 已分配|E2<br /><br /> 连接|  
|------------------------|----------------------|-----------------------|  
| (IH) |步进| (HY010) |  
  
 如果在 *HandleType* 设置为 SQL_HANDLE_ENV 的环境状态 E0 中调用 **SQLFreeHandle** ，则驱动程序管理器将返回 SQL_INVALID_HANDLE。 如果在 *HandleType* 设置为 SQL_HANDLE_ENV 的状态 E1 中调用该函数，则当函数成功并且在该函数失败时，环境将转到状态 E0。 如果在将 *HandleType* 设置为 SQL_HANDLE_ENV 的状态 E2 中调用该方法，则驱动程序管理器始终返回 SQL_ERROR HY010 (函数序列错误) 并且环境仍处于状态 E2。  
  
 若要了解状态转换表，需要了解哪些项 (环境、连接、语句或描述符) 它们引用的项。 假设函数接受类型为 X 的项的句柄。该函数的 X 状态转换表描述了如何使用类型为 X 的项的句柄调用函数，从而影响该项。 例如， **SQLDisconnect** 接受连接句柄。 **SQLDisconnect** 的连接状态转换表描述了 **SQLDisconnect** 如何影响它所调用的连接的状态。  
  
 假设函数接受类型为 Y 的项的句柄，其中 Y 不等于 X。该函数的 X 状态转换表描述了如何使用与 Y 类型的项关联的类型为 X 的句柄来调用函数，这将影响类型 Y 的项。例如， **SQLDisconnect** 的语句状态转换表描述在使用与该语句关联的连接的句柄调用时， **SQLDisconnect** 将如何影响该语句的状态。  
  
 本附录包含以下主题。  
  
-   [环境转换](../../../odbc/reference/appendixes/environment-transitions.md)  
  
-   [连接转换](../../../odbc/reference/appendixes/connection-transitions.md)  
  
-   [语句转换](../../../odbc/reference/appendixes/statement-transitions.md)  
  
-   [描述符转换](../../../odbc/reference/appendixes/descriptor-transitions.md)
