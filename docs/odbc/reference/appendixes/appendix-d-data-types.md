---
description: 附录 D：数据类型
title: 附录 D：数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], defined
- SQL data types [ODBC], defined
- data types [ODBC]
- data types [ODBC], about data types
ms.assetid: 981d49c3-3531-4543-aa75-5bd9e4f67000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2ff95858f654bef654b18105b4fda52c77cca0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212548"
---
# <a name="appendix-d-data-types"></a>附录 D：数据类型
ODBC 定义了两组数据类型： SQL 数据类型和 C 数据类型。 SQL 数据类型指示存储在数据源中的数据的数据类型。 C 数据类型指示存储在应用程序缓冲区中的数据的数据类型。  
  
 SQL 数据类型由每个 DBMS 根据 SQL-92 标准定义。 对于在 SQL-92 标准中指定的每个 SQL 数据类型，ODBC 定义类型标识符，这是作为 ODBC 函数中的参数传递或在结果集的元数据中返回的 **#define** 值。 ODBC 不支持的唯一 SQL-92 数据类型为 BIT (ODBC SQL_BIT 类型具有不同的特征) 、BIT_VARYING、TIME_WITH_TIMEZONE、TIMESTAMP_WITH_TIMEZONE 和 NATIONAL_CHARACTER。 驱动程序负责将特定于数据源的 SQL 数据类型映射到 ODBC SQL 数据类型标识符和特定于驱动程序的 SQL 数据类型标识符。 SQL 数据类型在实现描述符的 SQL_DESC_CONCISE_TYPE 字段中指定。  
  
 ODBC 定义 C 数据类型及其相应的 ODBC 类型标识符。 应用程序通过在对 **SQLBindCol** 或 **SQLGetData** 的调用中传递 *TargetType* 参数中的相应 C 类型标识符来指定将接收结果集数据的缓冲区的 C 数据类型。 它通过在对 **SQLBindParameter** 的调用中传递 *ValueType* 参数中适当的 C 类型标识符来指定包含语句参数的缓冲区的 C 类型。 C 数据类型在应用程序描述符的 SQL_DESC_CONCISE_TYPE 字段中指定。  
  
> [!NOTE]  
>  没有特定于驱动程序的 C 数据类型。  
  
 每个 SQL 数据类型都对应于一个 ODBC C 数据类型。 在从数据源返回数据之前，驱动程序将其转换为指定的 C 数据类型。 在将数据发送到数据源之前，驱动程序会将数据转换为指定的 C 数据类型。  
  
 本附录包含以下主题。  
  
-   [使用数据类型标识符](../../../odbc/reference/appendixes/using-data-type-identifiers.md)  
  
-   [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md)  
  
-   [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md)  
  
-   [数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)  
  
-   [伪类型标识符](../../../odbc/reference/appendixes/pseudo-type-identifiers.md)  
  
-   [以二进制格式传输数据](../../../odbc/reference/appendixes/transferring-data-in-its-binary-form.md)  
  
-   [间隔和数字数据类型的准则](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md)  
  
-   [公历的约束](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)  
  
-   [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)  
  
-   [将数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)  
  
-   [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)  
  
 有关 ODBC 数据类型的说明，请参阅 [odbc 中的数据类型](../../../odbc/reference/develop-app/data-types-in-odbc.md)。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。
