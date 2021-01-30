---
description: 从 C 到 SQL：时间
title: 从 C 到 SQL：时间 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], time
- time data type [ODBC]
- converting data from c to SQL types [ODBC], time
ms.assetid: a8da43c9-d9a5-45e5-bd9a-1dd633db2ee0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b53505a9f10d6f7039e0811e5f9fda8674092be1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195070"
---
# <a name="c-to-sql-time"></a>从 C 到 SQL：时间
ODBC C 数据类型的时间的标识符为：  
  
 SQL_C_TYPE_TIME  
  
 下表显示了可将时间 C 数据转换到的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|列字节长度 >= 8<br /><br /> 列字节长度 < 8<br /><br /> 数据值不是有效时间|不适用<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|列字符长度 >= 8<br /><br /> 列字符长度 < 8<br /><br /> 数据值不是有效时间|不适用<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_TIME|数据值为有效时间<br /><br /> 数据值不是有效时间|不适用<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|数据值为有效时间 [a]<br /><br /> 数据值不是有效时间|不适用<br /><br /> 22007|  
  
 [a] 时间戳的日期部分设置为当前日期，时间戳的秒小数部分设置为零。  
  
 有关 SQL_C_TYPE_TIME 结构中的有效值的信息，请参阅本附录前面的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md)。  
  
 将时间 C 数据转换为字符 SQL 数据时，生成的字符数据的格式为 "*hh*：*mm*：*ss*"。  
  
 驱动程序在转换时间 C 数据类型的数据时忽略长度/指示器值，并假定数据缓冲区的大小为时间 C 数据类型的大小。 长度/指示器值传入 **SQLPutData** 中的 *StrLen_or_Ind* 参数和在 **SQLBindParameter** 中通过 *StrLen_or_IndPtr* 参数指定的缓冲区中。 数据缓冲区是通过 **SQLPutData** 中的 *DataPtr* 参数和 **SQLBindParameter** 中的 *ParameterValuePtr* 参数指定的。
