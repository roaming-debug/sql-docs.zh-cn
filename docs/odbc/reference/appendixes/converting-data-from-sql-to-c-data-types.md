---
description: 将数据从 SQL 转换为 C 数据类型
title: 将数据从 SQL 转换为 C 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC]
- data conversions from SQL to C types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- data types [ODBC], SQL data types
- SQL data types [ODBC], converting to C types
- converting data from SQL to c types [ODBC]
- converting data from SQL to c types [ODBC], about converting
- C data types [ODBC], converting from SQL types
ms.assetid: 029727f6-d3f0-499a-911c-bcaf9714e43b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cd8b0790426d9f3461d8ff1600471867ce94b55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165373"
---
# <a name="converting-data-from-sql-to-c-data-types"></a>将数据从 SQL 转换为 C 数据类型
当应用程序调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLGetData** 时，驱动程序将从数据源中检索数据。 如果需要，它会将数据从驱动程序检索到的数据类型转换为 **SQLBindCol** 或 SQLGetData 中的 *TargetType* 参数所指定的数据类型 **。** 最后，它将数据存储在由 **SQLBindCol** 或 **SQLGetData** (中的 *TargetValuePtr* 参数指向的位置，并将 ARD) 的 SQL_DESC_DATA_PTR 字段。  
  
 下表显示了从 ODBC SQL 数据类型到 ODBC C 数据类型的支持转换。 实心圆圈表示 SQL 数据类型的默认转换 (C 数据类型，当 *TargetType* 的值 SQL_C_DEFAULT) 时，数据将转换为数据类型。 空心圆表示支持的转换。  
  
 对于 *使用 odbc 2.x* 驱动程序的 odbc 1.x 应用程序，可能不支持从特定于 *驱动程序的* 数据类型进行转换。  
  
 转换后的数据的格式不受 Windows®的 "国家/地区" 设置的影响。  
  
 以下各节中的表描述了驱动程序或数据源如何转换从数据源检索的数据;驱动程序需要支持从其支持的 ODBC SQL 数据类型转换到所有 ODBC C 数据类型。 对于给定的 ODBC SQL 数据类型，表的第一列将列出 **SQLBindCol** 和 **SQLGetData** 中 *TargetType* 参数的合法输入值。 第二列列出测试结果（通常使用 **SQLBindCol** 或 **SQLGetData** 中指定的 *BufferLength* 参数），驱动程序将执行该参数以确定是否可以转换数据。 对于每个结果，第三列和第四列都列出了 *TargetValuePtr* 参数指定的缓冲区中的值 *StrLen_or_IndPtr* ，并在驱动程序尝试转换数据后在 **SQLBindCol** 或 **SQLGetData** 中指定了这些值。  (*StrLen_or_IndPtr* 参数对应于 ARD 的 SQL_DESC_OCTET_LENGTH_PTR 字段。 ) 最后一列列出 **SQLFetch**、 **SQLFetchScroll** 或 **SQLGETDATA** 为每个结果返回的 SQLSTATE。  
  
 如果 **SQLBindCol** 或 **SQLGetData** 中的 *TARGETTYPE* 参数包含给定 odbc SQL 数据类型的表中未显示的 odbc C 数据类型的标识符，则 **SQLFETCH**、 **SQLFetchScroll** 或 **SQLGetData** 将返回 SQLSTATE 07006 (受限制的数据类型属性冲突) 。 如果 *TargetType* 参数包含指定从驱动程序特定的 SQL 数据类型转换为 ODBC C 数据类型的标识符，但驱动程序不支持此转换，则 **SQLFetch**、 **SQLFETCHSCROLL** 或 **SQLGetData** 将返回 SQLSTATE HYC00 (未实现) 的可选功能。  
  
 尽管表中没有显示，但当 SQL 数据值为 NULL 时，驱动程序将在 *StrLen_or_IndPtr* 参数所指定的缓冲区中返回 SQL_NULL_DATA。 有关在执行多个调用来检索数据时 *StrLen_or_IndPtr* 使用的说明，请参阅 [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)函数说明。 当 SQL 数据转换为字符 C 数据时，StrLen_or_IndPtr 中返回的字符数 \* 不包括 null 终止字节。 如果 *TargetValuePtr* 为 null 指针，则 **SQLGETDATA** 将返回 SQLSTATE HY009 (无效的 null 指针) ;在 **SQLBindCol** 中，此取消绑定列。  
  
 表中使用了以下术语和约定：  
  
-   **数据的字节长度** 是在 **TargetValuePtr* 中可返回的 C 数据的字节数，无论在将数据返回到应用程序之前数据是否会被截断。 对于字符串数据，不包括 null 终止字符的空间。  
  
-   **字符字节长度** 是以字符格式显示数据所需的总字节数。 这是针对节 [显示大小](../../../odbc/reference/appendixes/display-size.md)中的每个 C 数据类型定义的，只不过字符字节长度以字节为单位，而显示大小为字符。  
  
-   *斜体* 中的单词表示函数自变量或 SQL 语法的元素。 有关语法元素的语法，请参阅 " [附录 C： SQL 语法](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md)"。  
  
 本部分包含以下主题。  
  
-   [从 SQL 到 C：字符](../../../odbc/reference/appendixes/sql-to-c-character.md)  
  
-   [从 SQL 到 C：数字](../../../odbc/reference/appendixes/sql-to-c-numeric.md)  
  
-   [从 SQL 到 C：位](../../../odbc/reference/appendixes/sql-to-c-bit.md)  
  
-   [从 SQL 到 C：二进制](../../../odbc/reference/appendixes/sql-to-c-binary.md)  
  
-   [从 SQL 到 C：日期](../../../odbc/reference/appendixes/sql-to-c-date.md)  
  
-   [从 SQL 到 C：GUID](../../../odbc/reference/appendixes/sql-to-c-guid.md)  
  
-   [从 SQL 到 C：时间](../../../odbc/reference/appendixes/sql-to-c-time.md)  
  
-   [从 SQL 到 C：时间戳](../../../odbc/reference/appendixes/sql-to-c-timestamp.md)  
  
-   [从 SQL 到 C：年月间隔](../../../odbc/reference/appendixes/sql-to-c-year-month-intervals.md)  
  
-   [从 SQL 到 C：日期时间间隔](../../../odbc/reference/appendixes/sql-to-c-day-time-intervals.md)  
  
-   [从 SQL 到 C 的数据转换示例](../../../odbc/reference/appendixes/sql-to-c-data-conversion-examples.md)
