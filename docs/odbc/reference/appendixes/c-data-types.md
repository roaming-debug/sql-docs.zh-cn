---
description: C 数据类型
title: C 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
- C buffers [ODBC]
ms.assetid: b681d260-3dbb-47df-a616-4910d727add7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 322aa55f007e4ebf1ba3fa2b3093bf750db7e678
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051888"
---
# <a name="c-data-types"></a>C 数据类型
ODBC C 数据类型指示用于在应用程序中存储数据的 C 缓冲区的数据类型。  
  
 所有驱动程序都必须支持所有 C 数据类型。 这是必需的，因为所有驱动程序都必须支持其支持的 SQL 类型可以转换到的所有 C 类型，并且所有驱动程序都支持至少一个字符 SQL 类型。 由于可以在所有 C 类型之间转换字符 SQL 类型，因此所有驱动程序都必须支持所有 C 类型。  
  
 在带有 *TargetType* 参数的 **SQLBindCol** 和 **SQLGetData** 函数中指定 C 数据类型，并在 **SQLBindParameter** 函数中指定带有 *ValueType* 参数的。 此外，还可以通过调用 **SQLSetDescField** 来设置 ARD 或 APD 的 SQL_DESC_CONCISE_TYPE 字段，或通过使用 *类型* 参数 (和 *子类型* 参数（如果需要) 和 *DescriptorHandle* 参数设置为 ARD 或 APD 的句柄）调用 **SQLSetDescRec** 来指定。  
  
 下表列出了 C 数据类型的有效类型标识符。 该表还列出了对应于每个标识符的 ODBC C 数据类型和此数据类型的定义。  
  
|C 类型标识符|ODBC C typedef|C 类型|  
|-----------------------|--------------------|------------|  
|SQL_C_CHAR|SQLCHAR|unsigned char *|  
|SQL_C_WCHAR|SQLWCHAR|wchar_t *|  
|SQL_C_SSHORT [j]|SQLSMALLINT|short int|  
|SQL_C_USHORT [j]|SQLUSMALLINT|unsigned short int|  
|SQL_C_SLONG [j]|SQLINTEGER|long int|  
|SQL_C_ULONG [j]|SQLUINTEGER|unsigned long int|  
|SQL_C_FLOAT|SQLREAL|FLOAT|  
|SQL_C_DOUBLE|SQLDOUBLE、SQLFLOAT|double|  
|SQL_C_BIT|SQLCHAR|unsigned char|  
|SQL_C_STINYINT [j]|SQLSCHAR|signed char|  
|SQL_C_UTINYINT [j]|SQLCHAR|unsigned char|  
|SQL_C_SBIGINT|SQLBIGINT|_int64 [h]|  
|SQL_C_UBIGINT|SQLUBIGINT|无符号 _int64 [h]|  
|SQL_C_BINARY|SQLCHAR|unsigned char *|  
|SQL_C_BOOKMARK [i]|书签|无符号长整型 [d]|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|  
|所有 C 间隔数据类型|SQL_INTERVAL_STRUCT|请参阅本附录后面的 [C 间隔结构](../../../odbc/reference/appendixes/c-interval-structure.md) 部分。|  
  
 **C 类型标识符** SQL_C_TYPE_DATE [C]  
  
 **ODBC C typedef** SQL_DATE_STRUCT  
  
 **C 类型**  
  
```  
struct tagDATE_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;    
} DATE_STRUCT;[a]  
```  
  
 **C 类型标识符** SQL_C_TYPE_TIME [C]  
  
 **ODBC C typedef** SQL_TIME_STRUCT  
  
 **C 类型**  
  
```  
struct tagTIME_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
} TIME_STRUCT;[a]  
```  
  
 **C 类型标识符** SQL_C_TYPE_TIMESTAMP [C]  
  
 **ODBC C typedef** SQL_TIMESTAMP_STRUCT  
  
 **C 类型**  
  
```  
struct tagTIMESTAMP_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;[b]   
} TIMESTAMP_STRUCT;[a]  
```  
  
 **C 类型标识符** SQL_C_NUMERIC  
  
 **ODBC C typedef** SQL_NUMERIC_STRUCT  
  
 **C 类型**  
  
```  
struct tagSQL_NUMERIC_STRUCT {  
   SQLCHAR precision;  
   SQLSCHAR scale;  
   SQLCHAR sign[g];  
   SQLCHAR val[SQL_MAX_NUMERIC_LEN];[e], [f]   
} SQL_NUMERIC_STRUCT;  
```  
  
 **C 类型标识符** SQL_C_GUID  
  
 **ODBC C typedef** SQLGUID  
  
 **C 类型**  
  
```  
struct tagSQLGUID {  
   DWORD Data1;  
   WORD Data2;  
   WORD Data3;  
   BYTE Data4[8];  
} SQLGUID;[k]  
```  
  
 [a] datetime C 数据类型中的 year、month、day、hour、minute 和 second 字段的值必须符合公历的约束。  (参阅本附录后面的 [公历的约束](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md) 。 )   
  
 [b] "分数" 字段的值是十亿分之一的数目，范围为0到 999999999 (1 小于 1000000000) 。 例如，半秒的 "分数" 字段的值为500000000，对于第二个 (1 毫秒) 为1000000，为第二个 (一微秒) 为1000，为第二个 (的十亿分之一，则为1。  
  
 [c] 在 ODBC 2 中。*x*、C 日期、时间和时间戳数据类型 SQL_C_DATE、SQL_C_TIME 和 SQL_C_TIMESTAMP。  
  
 [d] ODBC 1.x *应用程序应* 使用 SQL_C_VARBOOKMARK，而不是 SQL_C_BOOKMARK。 ODBC *3.x 应用程序* 与 odbc 2 一起使用时。*x* *驱动程序，ODBC 2.X 驱动程序* 管理器会将 SQL_C_VARBOOKMARK 映射到 SQL_C_BOOKMARK。  
  
 [e] 在 little endian 模式下，数字以缩放整数的形式存储在 SQL_NUMERIC_STRUCT 结构的 *val* 字段中， (最左侧的字节) 最小有效字节。 例如，数字10.001 基数10，小数位数为4，将扩展为100010的整数。 因为这是十六进制格式的186AA，SQL_NUMERIC_STRUCT 中的值将是 "AA 86 01 00 00 .。。00 "，包含 SQL_MAX_NUMERIC_LEN **#define** 定义的字节数。  
  
 有关 **SQL_NUMERIC_STRUCT** 的详细信息，请参阅 [如何： SQL_NUMERIC_STRUCT 检索数值数据](retrieve-numeric-data-sql-numeric-struct-kb222831.md)。  
  
 [f] SQL_C_NUMERIC 数据类型的精度和小数位数字段进行，用于从应用程序进行输入以及从驱动程序输出到应用程序。 当驱动程序将数值写入 SQL_NUMERIC_STRUCT 时，它将使用自己的特定于驱动程序的默认值作为 *精度* 字段的值，并且它将使用应用程序描述符的 "SQL_DESC_SCALE" 字段中的值 (默认为 " *刻度* " 字段的 0) 。 应用程序可以通过设置应用程序描述符的 "SQL_DESC_PRECISION" 和 "SQL_DESC_SCALE" 字段为精度和小数位数提供自己的值。  
  
 [g] 如果为正，则符号字段为1，如果为负，则为0。  
  
 [h] _int64 可能不是由某些编译器提供的。  
  
 [i] _SQL_C_BOOKMARK 已在 ODBC 2.x 中弃用 *。*  
  
 [j] _SQL_C_SHORT、SQL_C_LONG 和 SQL_C_TINYINT 已在 ODBC 中由有符号和无符号类型替换： SQL_C_SSHORT 和 SQL_C_USHORT、SQL_C_SLONG 和 SQL_C_ULONG 以及 SQL_C_STINYINT 和 SQL_C_UTINYINT。 应与 ODBC 2 一起使用的 ODBC *2.x 驱动程序。**x* 应用程序应支持 SQL_C_SHORT、SQL_C_LONG 和 SQL_C_TINYINT，因为调用它们时，驱动程序管理器会将它们传递给驱动程序。  
  
 [k] SQL_C_GUID 只能转换为 SQL_CHAR 或 SQL_WCHAR。  
  
 本部分包含以下主题。  
  
-   [64 位整数结构](../../../odbc/reference/appendixes/64-bit-integer-structures.md)  
  
## <a name="see-also"></a>另请参阅  
 [ODBC 中的 C 数据类型](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)
