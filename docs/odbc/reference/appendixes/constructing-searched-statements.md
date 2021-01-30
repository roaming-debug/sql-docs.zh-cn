---
description: 构造搜索语句
title: 构造搜索语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- searched statements [ODBC]
- ODBC cursor library [ODBC], statement processing
- ODBC cursor library [ODBC], searched statements
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- cursor library [ODBC], searched statements
- SQL statements [ODBC], searched statements
ms.assetid: e429254c-c43f-4fbf-98b2-5f1ed53501ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7928e3c730a7431d7f94a90a85073a3c818ae31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165401"
---
# <a name="constructing-searched-statements"></a>构造搜索语句
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 为了支持定位更新和删除语句，游标库从定位语句构造搜索 **更新** 或 **删除** 语句。 为了支持对数据块中的 **SQLGetData** 的调用，游标库构造一个搜索 **SELECT** 语句，以创建包含当前数据行的结果集。 在这些语句中， **WHERE** 子句将枚举存储在缓存中的每个绑定列的值，这些值为 **SQLColAttribute** 中的 SQL_DESC_SEARCHABLE 字段标识符返回 SQL_PRED_SEARCHABLE 或 SQL_PRED_BASIC。  
  
> [!CAUTION]  
>  由游标库构造的用于标识当前行的 **WHERE** 子句可能无法标识任何行、标识不同行或标识多个行。  
  
 如果定位的 update 或 delete 语句影响多个行，则游标库只会更新游标所在行的行状态数组，并返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01001 (游标操作与) 。 如果语句未标识任何行，则游标库不会更新行状态数组，并返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01001 (游标操作冲突) 。 应用程序可以调用 **SQLRowCount** 来确定已更新或删除的行数。  
  
 如果用于对 **SQLGetData** 的调用定位游标的 **SELECT** 子句标识了多个行，则 **SQLGetData** 不能保证返回正确的数据。 如果未标识任何行，则 **SQLGetData** 将返回 SQL_NO_DATA。  
  
 如果应用程序符合以下准则，则游标库构造的 **WHERE** 子句应该唯一标识当前行，但在这种情况下，这种情况下是不可能的，例如当数据源包含重复行时。  
  
-   **绑定用于唯一标识行的列。** 如果绑定列没有唯一地标识行，则游标库构造的 **WHERE** 子句可能会标识多个行。 在定位的 update 或 delete 语句中，此类子句可能导致更新或删除多行。 在对 **SQLGetData** 的调用中，此类子句可能会导致驱动程序返回错误行的数据。 绑定唯一键中的所有列可确保每一行都是唯一标识的。  
  
-   **分配足够大的数据缓冲区，这样就不会发生截断。** 游标库的缓存是使用 **SQLBindCol** 绑定到结果集的行集缓冲区中的值的副本。 如果数据放置在这些缓冲区中，则它也会在缓存中被截断。 用截断值构造的 **WHERE** 子句可能无法正确地标识数据源中的基础行。  
  
-   **为二进制 C 数据指定非 null 长度缓冲区。** 仅当 **SQLBindCol** 中的 *StrLen_or_IndPtr* 参数为非 null 时，游标库才会在其缓存中分配长度缓冲区。 当 *TargetType* 参数 SQL_C_BINARY 时，游标库需要二进制数据的长度才能从数据构造 **WHERE** 子句。 如果 SQL_C_BINARY 列没有长度缓冲区，并且应用程序调用 **SQLGetData** 或尝试执行定位的 update 或 delete 语句，则游标库将返回 SQL_ERROR 和 SQLSTATE SL014， (发出了定位请求，但并不是所有列计数字段都已) 缓冲。  
  
-   **请为可为空的列指定非 null 长度的缓冲区。** 仅当 **SQLBindCol** 中的 *StrLen_or_IndPtr* 参数为非 null 时，游标库才会在其缓存中分配长度缓冲区。 由于 SQL_NULL_DATA 存储在长度缓冲区中，因此游标库假设没有为其指定任何长度缓冲区的列是不可为 null 的。 如果没有为可为 null 的列指定长度列，则游标库将构造一个使用该列的数据值的 **WHERE** 子句。 此子句将无法正确标识该行。
