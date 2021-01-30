---
description: 列大小、十进制数字、传输八位字节长度和显示大小-ODBC
title: 列大小、十进制数字、传输八位字节长度、显示大小 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- data types [ODBC], column size
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
- data types [ODBC], transfer octet length
ms.assetid: 723107a1-be08-4ea3-a8c0-b2c45d38d1aa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a23183fd60d947771cb041af977d518157ac573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182528"
---
# <a name="column-size-decimal-digits-transfer-octet-length-and-display-size---odbc"></a>列大小、十进制数字、传输八位字节长度和显示大小-ODBC
数据类型的特征是其列 (或参数) 大小、十进制数字、长度和显示大小。 以下 ODBC 函数为 SQL 语句中的参数或数据源中的 SQL 数据类型返回这些属性。 每个 ODBC 函数返回一组不同的这些属性，如下所示：  
  
-   **SQLDescribeCol** 返回它描述的列的列大小和十进制数字。  
  
-   **SQLDescribeParam** 返回它所描述参数的大小和小数位数。 **SQLBindParameter** 为 SQL 语句中的参数设置参数大小和十进制数字。  
  
-   目录函数 **SQLColumns**、 **SQLProcedureColumns** 和 **SQLGetTypeInfo** 返回表、结果集或过程参数中的列的属性，以及数据源中数据类型的目录属性。 **SQLColumns** 返回指定表中的列大小、十进制数字和列长度 (如基表、视图或系统表) 。 **SQLProcedureColumns** 返回过程中列的列大小、十进制数字和长度。 **SQLGetTypeInfo** 返回数据源上的 SQL 数据类型的最大列大小和最小和最大小数位数。  
  
 这些用于列或参数大小的函数返回的值对应于 ODBC 2 中定义的 "precision"。*x*。 不过，这些值不一定对应于 SQL_DESC_PRECISION 中返回的值或任何其他一个描述符字段。 对于十进制数字，这同样适用于 ODBC 2 中定义的 "scale"。*x*。 这并不一定对应于 SQL_DESC_SCALE 或任何其他一个描述符字段中返回的值，而是根据数据类型不同的描述符字段。 有关详细信息，请参阅 [列大小](../../../odbc/reference/appendixes/column-size.md) 和 [十进制数字](../../../odbc/reference/appendixes/decimal-digits.md)。  
  
 同样，"传输八位字节长度" 的值不是来自 SQL_DESC_LENGTH。 它们来自所有字符和二进制类型的描述符字段的 SQL_DESC_OCTET_LENGTH。 没有用于保存其他类型信息的描述符字段。  
  
 所有数据类型的显示大小值对应于单个描述符字段中的值，SQL_DESC_DISPLAY_SIZE。  
  
 描述符字段描述结果集的特征。 描述符字段在语句执行前不包含有关数据的有效值。 另一方面， **SQLColumns**、 **SQLProcedureColumns** 和 **SQLGetTypeInfo** 返回的列大小、十进制数字和显示大小的值将返回数据源的目录中存在的数据库对象（如表列和数据类型）的特征。 同样，在其结果集中， **SQLColAttribute** 返回数据源中的列大小、十进制数和字节数这些值不一定与 SQL_DESC_PRECISION、SQL_DESC_SCALE 和 SQL_DESC_OCTET_LENGTH 描述符字段中的值相同。  
  
 有关这些描述符字段的详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)。  
  
 相关主题：  
  
-   [列大小](../../../odbc/reference/appendixes/column-size.md)  
-   [十进制数字](../../../odbc/reference/appendixes/decimal-digits.md)  
-   [传输八位字节长度](../../../odbc/reference/appendixes/transfer-octet-length.md)  
-   [显示大小](../../../odbc/reference/appendixes/display-size.md)
