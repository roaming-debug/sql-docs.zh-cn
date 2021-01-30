---
description: 处理定位更新和删除语句
title: 正在处理定位更新和删除语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- SQL statements [ODBC], cursor library
- ODBC cursor library [ODBC], positioned update or delete
- cursor library [ODBC], statement processing
ms.assetid: 2975dd97-48e6-4d0a-a9c7-40759a7d94c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f2d7283524a516b81b6489b543fd3169f7f9aeb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159539"
---
# <a name="processing-positioned-update-and-delete-statements"></a>处理定位更新和删除语句
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 游标库通过将此类语句中的 **WHERE CURRENT** of 子句替换为每个绑定列枚举其缓存中存储的值的 **where** 子句，来支持定位的 update 和 delete 语句。 游标库将新构造的 **UPDATE** 和 **DELETE** 语句传递到要执行的驱动程序。 对于定位的 update 语句，游标库随后将从行集缓冲区中的值更新其缓存，并将行状态数组中的相应值设置为 SQL_ROW_UPDATED。 对于定位的 delete 语句，它将行状态数组中的相应值设置为 SQL_ROW_DELETED。  
  
> [!CAUTION]  
>  由游标库构造的用于标识当前行的 **WHERE** 子句可能无法标识任何行、标识不同行或标识多个行。 有关详细信息，请参阅本附录后面的 [构造搜索语句](../../../odbc/reference/appendixes/constructing-searched-statements.md)。  
  
 定位的 update 和 delete 语句受到下列限制：  
  
-   定位的 update 和 delete 语句仅在以下情况下可用：当 **SELECT** 语句生成结果集时;如果 **SELECT** 语句不包含联接、 **联合** 子句或 **GROUP by** 子句，则为;在选择列表中使用别名或表达式的任何列均未绑定到 **SQLBindCol**。  
  
-   如果应用程序准备定位的 update 或 delete 语句，则必须在调用 **SQLFetch** 或 **SQLFetchScroll** 后执行此操作。 尽管游标库将该语句提交给驱动程序以准备准备，但它会关闭该语句，并在应用程序调用 **SQLExecute** 时直接执行该语句。  
  
-   如果驱动程序仅支持一个活动语句，则游标库将提取结果集的其余部分，然后在执行定位的 update 或 delete 语句之前从其缓存中 refetches 当前行集。 如果应用程序随后调用在结果集中返回元数据的函数 (例如， **SQLNumResultCols** 或 **SQLDescribeCol**) ，则游标库将返回错误。  
  
-   如果对包含时间戳列的表的列执行定位的 update 或 delete 语句，而该时间戳列在每次执行更新时都会自动更新，则所有后续定位的 update 或 delete 语句都将在时间戳列被绑定时失败。 出现这种情况的原因是，游标库创建的搜索的 update 或 delete 语句并不能准确地识别要更新的行。 对于 timestamp 列，搜索语句中的值将与时间戳列的自动更新值不匹配。
