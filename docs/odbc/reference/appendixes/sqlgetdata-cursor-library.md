---
description: SQLGetData（游标库）
title: SQLGetData (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetData function [ODBC], Cursor Library
ms.assetid: ff40c9c0-b847-4426-a099-1bff47e6e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e0cb6a73fb294e8b47ab82c988f8d07a4a220df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202791"
---
# <a name="sqlgetdata-cursor-library"></a>SQLGetData（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLGetData** 函数。 有关 **SQLGetData** 的常规信息，请参阅 [SQLGetData 函数](../../../odbc/reference/syntax/sqlgetdata-function.md)。  
  
 游标库通过先使用 **WHERE** 子句构造一个 **SELECT** 语句来实现 **SQLGetData** ，该语句可枚举当前行中每个绑定列中存储的值。 然后，它会执行 **SELECT** 语句以重新选择该行，并在驱动程序中调用 **SQLGetData** ，以从数据源中检索数据， (与缓存) 不同。  
  
> [!CAUTION]  
>  由游标库构造的用于标识当前行的 **WHERE** 子句可能无法标识任何行、标识不同行或标识多个行。 有关详细信息，请参阅 [构造搜索语句](../../../odbc/reference/appendixes/constructing-searched-statements.md)。  
  
 如果 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，则可以对第0列调用 **SQLGetData** ，以返回书签数据。  
  
 对 **SQLGetData** 的调用受到以下限制：  
  
-   对于只进游标，不能调用 **SQLGetData** 。  
  
-   仅当满足以下条件时，才能调用 **SQLGetData** ： **SELECT** 语句生成的结果集;**SELECT** 语句不包含联接、**联合** 子句或 **GROUP by** 子句;在选择列表中使用别名或表达式的任何列都不与 **SQLBindCol** 绑定。  
  
-   如果驱动程序仅支持一个活动语句，则在执行 **SELECT** 语句并调用 **SQLGetData** 之前，游标库将提取结果集的其余部分。
