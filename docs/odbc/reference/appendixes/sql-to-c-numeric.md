---
description: 从 SQL 到 C：数字
title: SQL 到 C：数值 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], numeric
- numeric data type [ODBC], converting
- converting data from SQL to C types [ODBC], numeric
ms.assetid: 76f8b5d5-4bd0-4dcb-a90a-698340e0d36e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f18cc2ecff847b2d7d7716e2c302de2b7bb3ff2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203022"
---
# <a name="sql-to-c-numeric"></a>从 SQL 到 C：数字

数字 ODBC SQL 数据类型的标识符如下：

- SQL_DECIMAL  
- SQL_BIGINT  
- SQL_NUMERIC  
- SQL_REAL  
- SQL_TINYINT  
- SQL_FLOAT  
- SQL_SMALLINT  
- SQL_DOUBLE SQL_INTEGER  

下表显示了数字 SQL 数据可转换为的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  

|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|字符字节长度 < *BufferLength*<br /><br /> 整个 (的数目，相对于) 数字的小数位数 < *BufferLength*<br /><br /> 整个 (的数目，相对于小数) 位数 >= *BufferLength*|数据<br /><br /> 截断的数据<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|字符长度 < *BufferLength*<br /><br /> 整个 (的数目，相对于) 数字的小数位数 < *BufferLength*<br /><br /> 整个 (的数目，相对于小数) 位数 >= *BufferLength*|数据<br /><br /> 截断的数据<br /><br /> Undefined|数据的长度（以字符为长度）<br /><br /> 数据的长度（以字符为长度）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_STINYINT<br /><br /> SQL_C_UTINYINT<br /><br /> SQL_C_TINYINT<br /><br /> SQL_C_SBIGINT<br /><br /> SQL_C_UBIGINT<br /><br /> SQL_C_SSHORT<br /><br /> SQL_C_USHORT<br /><br /> SQL_C_SHORT<br /><br /> SQL_C_SLONG<br /><br /> SQL_C_ULONG<br /><br /> SQL_C_LONG<br /><br /> SQL_C_NUMERIC|不截断而转换的数据 [a]<br /><br /> 用小数位截断转换的数据 [a]<br /><br /> 数据的转换会导致整个 (丢失，而不是) 数字的小数部分 [a]|数据<br /><br /> 截断的数据<br /><br /> Undefined|C 数据类型的大小<br /><br /> C 数据类型的大小<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_FLOAT<br /><br /> SQL_C_DOUBLE|数据在要将数字转换到的数据类型的范围内 [a]<br /><br /> 数据在要将数字转换到的数据类型的范围之外 [a]|数据<br /><br /> Undefined|C 数据类型的大小<br /><br /> Undefined|不适用<br /><br /> 22003|  
|SQL_C_BIT|Data 为0或 1 [a]<br /><br /> 数据大于0，小于2，并且不等于 1 [a]<br /><br /> 数据小于0或大于等于 2 [a]|数据<br /><br /> 截断的数据<br /><br /> Undefined|1 [b]<br /><br /> 1 [b]<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> Undefined|数据的长度<br /><br /> Undefined|不适用<br /><br /> 22003|  
|SQL_C_INTERVAL_MONTH [c] SQL_C_INTERVAL_YEAR [c] SQL_C_INTERVAL_DAY [c] SQL_C_INTERVAL_HOUR [c] SQL_C_INTERVAL_MINUTE [c] SQL_C_INTERVAL_SECOND [c]|数据未截断<br /><br /> 秒的小数部分被截断<br /><br /> 数字的整个部分已截断|数据<br /><br /> 截断的数据<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_INTERVAL_YEAR_TO_MONTH SQL_C_INTERVAL_DAY_TO_HOUR SQL_C_INTERVAL_DAY_TO_MINUTE SQL_C_INTERVAL_DAY_TO_SECOND SQL_C_INTERVAL_HOUR_TO_MINUTE SQL_C_INTERVAL_HOUR_TO_SECOND|数字的整个部分已截断|Undefined|Undefined|22015|  
  
 [a] 此转换将忽略 *BufferLength* 的值。 驱动程序假设大小 **TargetValuePtr* 是 C 数据类型的大小。  
  
 [b] 这是对应的 C 数据类型的大小。  
  
 [c] 仅对精确数值数据类型 (SQL_DECIMAL、SQL_NUMERIC、SQL_TINYINT、SQL_SMALLINT、SQL_INTEGER 和 SQL_BIGINT) 支持此转换。 近似数值数据类型不支持 (SQL_REAL、SQL_FLOAT 或 SQL_DOUBLE) 。  

## <a name="sql_c_numeric-and-sqlsetdescfield"></a>SQL_C_NUMERIC 和 SQLSetDescField

 若要执行 SQL_C_NUMERIC 值的手动绑定，需要 [SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md) 。  (请注意，SQLSetDescField 已添加到 ODBC 3.0。 ) 若要执行手动绑定，必须首先获取描述符句柄。  

```cpp
if (fCType == SQL_C_NUMERIC) {   
   // special processing required for NUMERIC to get right scale & precision  
   // Modify the fields in the implicit application parameter descriptor  
   SQLHDESC hdesc=NULL;  
  
   // Use SQL_ATTR_APP_ROW_DESC for calls to SQLBindCol()  
   // Use SQL_ATTR_APP_PARAM_DESC for calls to SQLBindParameter()  
   //  
   // retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_ROW_DESC, &hdesc, 0, NULL);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);  
   if (!ODBC_CALL_SUCCESS(retcode)) {  
      printf ("\nSQLGetStmtAttr failed");  
      i = 1;  
      sqlstate[7] = '\0';  
      while (SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, i, sqlstate, &NativeError, wrkbuf, sizeof(wrkbuf), &len) != SQL_NO_DATA) {  
         printf("\niTestCase = %d Failed...Precision = %d, Scale = %d\nNativeError=%d, State=%s, \n  Message=%s",   
            iTestCase, Precision, Scale, NativeError, sqlstate, wrkbuf);  
         i++;  
      }  
      continue;  
   }  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_PRECISION, (SQLPOINTER)num.precision, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_SCALE, (SQLPOINTER)num.scale, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_DATA_PTR, (SQLPOINTER) &(num), sizeof(num));  
```
