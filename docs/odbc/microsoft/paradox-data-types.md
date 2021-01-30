---
description: Paradox 数据类型
title: Paradox 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], Paradox driver
- desktop database drivers [ODBC], Paradox driver
- Paradox data types [ODBC]
- Jet-based ODBC drivers [ODBC], Paradox driver
- data types [ODBC], Paradox driver
- Paradox driver [ODBC], data types
ms.assetid: 0c9e5d21-9321-49f8-a055-69459e1c9c85
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ee49ff2df24a57daa481ea650f57c09a7712400e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203505"
---
# <a name="paradox-data-types"></a>Paradox 数据类型
ODBC Paradox 驱动程序将 Paradox 数据类型映射到 ODBC SQL 数据类型。 下表列出了所有 Paradox 数据类型，并显示了它们所映射到的 ODBC SQL 数据类型。  
  
|Paradox 数据类型|ODBC 数据类型|  
|-----------------------|--------------------|  
|字符|SQL_VARCHAR|  
|自动增量 [1]|SQL_INTEGER|  
|BCD [1]|SQL_DOUBLE|  
|字节 [1]|SQL_BINARY|  
|DATE|SQL_DATE|  
|映像 [2]|SQL_LONGVARBINARY|  
|逻辑 [1]|SQL_BIT|  
|LONG [1]|SQL_INTEGER|  
|备注 [2]|SQL_LONGVARCHAR|  
|MONEY [1]|SQL_DOUBLE|  
|NUMBER|SQL_DOUBLE|  
|SHORT|SQL_SMALLINT|  
|时间 [1]|SQL_TIMESTAMP|  
|TIMESTAMP [1]|SQL_TIMESTAMP|  
  
 [1] 仅对 Paradox 版本5有效。*x*。  
  
 [2] 仅对 Paradox 版本4有效。*x* 和5。*x*。  
  
> [!NOTE]  
>  **SQLGetTypeInfo** 返回 ODBC SQL 数据类型。 对于本主题中前面列出的 ODBC SQL 数据类型，支持 *Odbc 程序员参考* 的附录 D 中的所有转换。  
  
 下表显示了对 Paradox 数据类型的限制。  
  
|数据类型|说明|  
|---------------|-----------------|  
|字符|创建零或未指定长度的字母数字列实际上将返回255字节的列。|  
|BYTES|如果使用 Paradox5 驱动程序向二进制列中插入 NULL，则它将更改为0。|  
|LONG|在 Paradox 5 中，Long 数据类型的 Paradox 驱动程序所支持的最大负值。*x* 不是-2 ^ 31 (-2147483648) ，因为它的长映射到 ODBC 数据类型 SQL_INTEGER。 支持的最大负值值实际上是-2 ^ 31 + 1 (-2147483647) 。|  
|TIMESTAMP|当 Paradox 驱动程序将某个值插入到时间戳列中后，在随后从列中进行检索时，检索到的值可能会与插入的值不同，因为舍入只需1秒。|  
  
 [数据类型限制](../../odbc/microsoft/data-type-limitations.md)中可以找到更多有关数据类型的限制。
