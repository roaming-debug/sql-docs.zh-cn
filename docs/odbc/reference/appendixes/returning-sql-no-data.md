---
description: 返回 SQL_NO_DATA
title: 返回 SQL_NO_DATA |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL_NO_DATA [ODBC]
- backward compatibility [ODBC], SQL_NO_DATA
- compatibility [ODBC], SQL_NO_DATA
ms.assetid: deed0163-9d1a-4e9b-9342-3f82e64477d2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f61d676897bf7b5633a0c9d37c12e44ae9de1cbe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187163"
---
# <a name="returning-sql_no_data"></a>返回 SQL_NO_DATA
当 *odbc 1.x 应用程序* 但 odbc *1.x 驱动程序* 调用 **SQLExecDirect**、 **SQLExecute** 或 **SQLParamData**，并执行搜索的 update 或 delete 语句时，但不会影响数据源中的任何行时 *，ODBC 1.x* 驱动程序应返回 SQL_SUCCESS。 当 *使用 odbc 1.x* *驱动程序的 odbc 1.x 应用程序* 调用 **SQLExecDirect**、 **SQLExecute** 或 **SQLParamData** 的相同结果时 *，ODBC 3.x* 驱动程序应返回 SQL_NO_DATA。  
  
 如果在一批语句中搜索的 update 或 delete 语句不会影响数据源中的任何行，则 **SQLMoreResults** 将返回 SQL_SUCCESS。 它不能返回 SQL_NO_DATA，因为这意味着不会有更多的结果，而不会影响不影响任何行的搜索更新/删除结果。
