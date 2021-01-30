---
description: 从 C 到 SQL：日期
title: 从 C 到 SQL： Date |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date data type [ODBC]
- converting data from c to SQL types [ODBC], date
- data conversions from C to SQL types [ODBC], date
ms.assetid: bea087d3-911f-418b-b483-d2b5b334da19
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0724309642b8a6dc640b6159715927544d74733
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207849"
---
# <a name="c-to-sql-date"></a>从 C 到 SQL：日期
Date ODBC C 数据类型的标识符是：  
  
 SQL_C_TYPE_DATE  
  
 下表显示了日期 C 数据可转换为的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|列字节长度 >= 10<br /><br /> 列字节长度 < 10<br /><br /> 数据值不是有效日期|不适用<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|列字符长度 >= 10<br /><br /> 列字符长度 < 10<br /><br /> 数据值不是有效日期|不适用<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|数据值是有效日期<br /><br /> 数据值不是有效日期|不适用<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|数据值是有效的日期 [a]<br /><br /> 数据值不是有效日期|不适用<br /><br /> 22007|  
  
 [a] 时间戳的时间部分设置为零。  
  
 有关 SQL_C_TYPE_DATE 结构中的有效值的信息，请参阅本附录前面的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md)。  
  
 当日期 C 数据转换为字符 SQL 数据时，生成的字符数据的格式为 "*yyyy* - *mm* - *dd*"。  
  
 驱动程序在从日期 C 数据类型转换数据时忽略长度/指示器值，并假定数据缓冲区的大小为日期 C 数据类型的大小。 长度/指示器值传入 **SQLPutData** 中的 *StrLen_or_Ind* 参数和在 **SQLBindParameter** 中通过 *StrLen_or_IndPtr* 参数指定的缓冲区中。 数据缓冲区是通过 **SQLPutData** 中的 *DataPtr* 参数和 **SQLBindParameter** 中的 *ParameterValuePtr* 参数指定的。
