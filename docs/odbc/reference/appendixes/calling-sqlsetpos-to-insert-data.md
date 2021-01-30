---
description: 调用 SQLSetPos 以插入数据
title: 调用 SQLSetPos 以插入数据 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLSetPos
- SQLSetPos function [ODBC], inserting data
- backward compatibility [ODBC], SqlSetPos
ms.assetid: 03e5c4d0-2bb3-4649-9781-89cab73f78eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c9ddd58e9f463d7878bb4b9b8710ce0e6dfecc4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110900"
---
# <a name="calling-sqlsetpos-to-insert-data"></a>调用 SQLSetPos 以插入数据
当 *使用 odbc 1.x* *驱动程序的 odbc 1.x 应用程序* 使用 SQL_ADD 的 *操作* 参数调用 **SQLSetPos** 时，驱动程序管理器不会将此调用映射到 **SQLBulkOperations**。 如果 *ODBC 1.x 驱动程序应* 与使用 SQL_ADD 调用 **SQLSetPos** 的应用程序配合使用，则驱动程序应支持该操作。  
  
 当调用 **SQLSetPos** 时，行为的主要区别是在状态 S6 中调用时出现 SQL_ADD。 在 ODBC *2.x 中，* 当调用 **SQLSetPos** 时，驱动程序将返回 S1010，SQL_ADD 并在将游标定位到 **SQLFetch**) 之后，在状态 S6 (中调用。 在 ODBC *3.x 中，* SQLBulkOperations *操作* SQL_ADD 的可以在 S6 状态下调用。 行为的另一个主要差别在于， **SQLBulkOperations** 操作 SQL_ADD 的 *操作* 可以在中调用，而 **SQLSetPos** 的 SQL_ADD **操作** 不能。 对于 ODBC 1.x 中相同调用可能发生的语句转换 *，请参阅*[附录 B： ODBC 状态转换表](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md)。
