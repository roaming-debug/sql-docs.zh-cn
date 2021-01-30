---
description: Microsoft Access 数据类型
title: Microsoft Access 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Access driver
- Jet-based ODBC drivers [ODBC], Access driver
- desktop database drivers [ODBC], Access driver
- Access driver [ODBC], data types
- access data types [ODBC]
- data types [ODBC], Access driver
ms.assetid: b537348a-bea0-4bd6-84a4-52a75292957f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0e7a55c6929d63bf036c9f1585d2eb039f6fec77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205712"
---
# <a name="microsoft-access-data-types"></a>Microsoft Access 数据类型
下表显示了 Microsoft Access 数据类型、用于创建表的数据类型以及 ODBC SQL 数据类型。  
  
|Microsoft Access 数据类型|CREATETABLE) 的数据类型 (|ODBC SQL 数据类型|  
|--------------------------------|-------------------------------|------------------------|  
|BIGBINARY [1]|LONGBINARY|SQL_LONGVARBINARY|  
|BINARY|BINARY|SQL_BINARY|  
|BIT|BIT|SQL_BIT|  
|对抗|对抗|SQL_INTEGER|  
|CURRENCY|CURRENCY|SQL_NUMERIC|  
|日期/时间|DATETIME|SQL_TIMESTAMP|  
|GUID|GUID|SQL_GUID|  
|长整型|LONGBINARY|SQL_LONGVARBINARY|  
|长文本|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|"|LONGTEXT|SQL_LONGVARCHAR [2] SQL_WLONGVARCHAR [3]|  
|NUMBER (FieldSize = SINGLE) |单个|SQL_REAL|  
|NUMBER (FieldSize = DOUBLE) |DOUBLE|SQL_DOUBLE|  
|NUMBER (FieldSize = BYTE) |无符号字节|SQL_TINYINT|  
|NUMBER (FieldSize = INTEGER) |SHORT|SQL_SMALLINT|  
|NUMBER (FieldSize = LONG INTEGER) |LONG|SQL_INTEGER|  
|NUMERIC|NUMERIC|SQL_NUMERIC|  
|OLE|LONGBINARY|SQL_LONGVARBINARY|  
|TEXT|VARCHAR|SQL_VARCHAR [1] SQL_WVARCHAR [2]|  
|VARBINARY|VARBINARY|SQL_VARBINARY|  
  
 [1] 仅访问4.0 应用程序。 最大长度为4000字节。 类似于 LONGBINARY 的行为。  
  
 仅限 [2] ANSI 应用程序。  
  
 [3] 仅 Unicode 和 Access 4.0 应用程序。  
  
> [!NOTE]  
>  **SQLGetTypeInfo** 返回 ODBC 数据类型。 如果有多个 Microsoft 访问类型映射到相同的 ODBC SQL 数据类型，它将不会返回所有 Microsoft Access 数据类型。 对于上表中列出的 SQL 数据类型，支持 *ODBC 程序员参考* 的附录 D 中的所有转换。  
  
 下表显示了对 Microsoft Access 数据类型的限制。  
  
|数据类型|说明|  
|---------------|-----------------|  
|BINARY、VARBINARY 和 VARCHAR|创建零或未指定长度的 BINARY、VARBINARY 或 VARCHAR 列实际上将返回510字节的列。|  
|BYTE|即使 "FieldSize" 等于 "字节" 的 "Microsoft 访问号码" 字段没有符号，也可以在使用 Microsoft Access 驱动程序时，将负数插入该字段。|  
|CHAR、LONGVARCHAR 和 VARCHAR|字符串文本可包含任何 ANSI 字符 (1-255 decimal) 。 使用两个连续的单引号 ( "" ) 表示一个单引号 ( ") 。<br /><br /> 使用字符数据类型列中的任何特殊字符时，应使用过程来传递字符数据。|  
|DATE|日期值必须根据 ODBC 规范日期格式进行分隔，或由日期时间分隔符分隔 ( "#" ) 。 否则，Microsoft Access 会将该值视为算术表达式，而不会引发警告或错误。<br /><br /> 例如，日期 "3 月5日 1996" 必须表示为 {d ' 1996-03-05 '} 或 #03/05/1996 #;否则，如果仅提交03/05/1993，Microsoft Access 会将此值计算为3除以5除以1996。 此值向上舍入到整数0，并且从零日映射到1899-12-31，这是使用的日期。<br /><br /> 不能在日期值中使用 ( # A0) 的管道字符，即使用引号引起来也是如此。|  
|GUID|限制为 Microsoft Access 4.0 的数据类型。|  
|NUMERIC|限制为 Microsoft Access 4.0 的数据类型。|  
  
 [数据类型限制](../../odbc/microsoft/data-type-limitations.md)中可以找到更多有关数据类型的限制。
