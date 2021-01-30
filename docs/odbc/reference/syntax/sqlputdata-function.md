---
description: SQLPutData 函数
title: SQLPutData 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPutData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPutData
helpviewer_keywords:
- SQLPutData function [ODBC]
ms.assetid: 9a60f004-1477-4c54-a20c-7378e1116713
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f76a3624222968aabf91f9aff012e9370aa8ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192559"
---
# <a name="sqlputdata-function"></a>SQLPutData 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLPutData** 允许应用程序在语句执行时向驱动程序发送参数或列的数据。 此函数可用于将部分的字符或二进制数据值发送到具有字符、二进制或数据源特定数据类型的列 (例如，SQL_LONGVARBINARY 或 SQL_LONGVARCHAR 类型) 的参数。 **SQLPutData** 支持绑定到 unicode C 数据类型，即使基础驱动程序不支持 unicode 数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLPutData(  
      SQLHSTMT     StatementHandle,  
      SQLPOINTER   DataPtr,  
      SQLLEN       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *DataPtr*  
 送指向某个缓冲区的指针，该缓冲区包含参数或列的实际数据。 数据必须为参数数据) 的 *ValueType* 参数中指定的 C 数据类型或列数据) 的 **SQLBindCol** (的 *TargetType* **参数 (。**  
  
 StrLen_or_Ind   
 送\* *DataPtr* 的长度。 指定在对 **SQLPutData** 的调用中发送的数据量。 每次调用给定的参数或列时，数据量可能会有所不同。 除非满足以下条件之一，否则将忽略 *StrLen_or_Ind* ：  
  
-   *StrLen_or_Ind* 为 SQL_NTS、SQL_NULL_DATA 或 SQL_DEFAULT_PARAM。  
  
-   在 **SQLBindParameter** 或 **SQLBindCol** 中指定的 C 数据类型为 SQL_C_CHAR 或 SQL_C_BINARY。  
  
-   C 数据类型为 SQL_C_DEFAULT，指定 SQL 数据类型的默认 C 数据类型为 SQL_C_CHAR 或 SQL_C_BINARY。  
  
 对于所有其他类型的 C 数据，如果 *StrLen_or_Ind* 不 SQL_NULL_DATA 或 SQL_DEFAULT_PARAM，则驱动程序会假定 \* *DataPtr* 缓冲区的大小是用 *ValueType* 或 *TargetType* 指定的 C 数据类型的大小并发送整个数据值。 有关详细信息，请参阅附录 D：数据类型中的 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) 。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLPutData** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLPutData** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|为 output 参数返回的字符串或二进制数据导致截断非空白字符或非空的二进制数据。 如果它是一个字符串值，则它将被右截断。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|绑定参数的 **SQLBindParameter** 中的 *ValueType* 参数标识的数据值无法转换为 **SQLBindParameter** 中 *ParameterType* 参数所标识的数据类型。|  
|07S01|默认参数的使用无效|使用 **SQLBindParameter** 设置的参数值是 SQL_DEFAULT_PARAM 的，并且相应的参数没有默认值。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22001|字符串数据，右截断|将字符或二进制值分配给列会导致截断非空白 (字符) 或非 null (二进制) 字符或字节。<br /><br /> **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型是 "Y"，为长参数发送了更多数据， (数据类型为 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或长数据源特定的数据类型) 与 **SQLBindParameter** 中的 *StrLen_or_IndPtr* 参数一起指定。<br /><br /> **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型是 "Y"，为长列发送了更多的数据 (数据类型为 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或长数据源特定数据类型) ，该数据类型是与使用 **SQLBulkOperations** 添加或更新的数据行中的列相对应的，或者已使用 **SQLSetPos** 进行更新。|  
|22003|数值超出范围|为绑定的数值参数或列发送的数据导致整个 (，而不是分配给关联的表列时要截断的数字) 部分的小数部分。<br /><br /> 对于一个或多个输入/输出参数 (，返回数值或字符串) 的数值将导致整个 (，而不是将被截断的数字) 部分的小数部分。|  
|22007|Datetime 格式无效|为绑定到日期、时间或时间戳结构的参数或列发送的数据分别是无效的日期、时间或时间戳。<br /><br /> 输入/输出或输出参数被绑定到日期、时间或时间戳 C 结构，返回的参数中的值分别为无效的日期、时间或时间戳。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22008|日期时间字段溢出|为输入/输出或输出参数计算的日期时间表达式导致日期、时间或时间戳 C 结构无效。|  
|22012|被零除|计算出的输入/输出参数或输出参数的算术表达式导致被零除。|  
|22015|间隔字段溢出|对于精确数值或间隔列或对间隔 SQL 数据类型的参数发送的数据将导致有效位丢失。<br /><br /> 为具有多个字段的间隔列或参数发送了数据，该数据已转换为数值数据类型，并且没有数值数据类型的表示形式。<br /><br /> 为列或参数数据发送的数据被分配到某个间隔 SQL 类型，并且该间隔 SQL 类型中没有 C 类型的值的表示形式。<br /><br /> 为数值 C 类型的确切数值或间隔 C 列或参数发送的数据会导致有效位丢失。<br /><br /> 为列或参数数据发送的数据被分配到一个 interval C 结构，且间隔数据结构中没有数据表示形式。|  
|22018|转换规范的字符值无效|C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列或参数中的值不是绑定 C 类型的有效文本。<br /><br /> SQL 类型是精确或近似的数字、日期时间或时间间隔数据类型;C 类型为 SQL_C_CHAR;列或参数中的值不是绑定的 SQL 类型的有效文本。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效| (DM) 参数 *DataPtr* 为 null 指针，并且参数 *StrLen_or_Ind* 不是0、SQL_DEFAULT_PARAM 或 SQL_NULL_DATA。|  
|HY010|函数序列错误| (DM) 上一个函数调用不是对 **SQLPutData** 或 **SQLParamData** 的调用。<br /><br />  (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 SQLPutData 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY019|分段发送非字符和非二进制数据|为某个参数或列调用了 **SQLPutData** ，但未使用将字符 c 数据发送到具有字符、二进制或数据源特定数据类型的列，或将二进制 c 数据发送到具有字符、二进制或数据源特定数据类型的列。|  
|HY020|尝试连接 null 值|多次调用了 **SQLPutData** ，因为调用返回 SQL_NEED_DATA，并在其中一个调用中包含 SQL_NULL_DATA 或 SQL_DEFAULT_PARAM *StrLen_or_Ind* 参数。|  
|HY090|字符串或缓冲区长度无效|参数 *DataPtr* 不是 null 指针，且参数 *StrLen_or_Ind* 小于0但不等于 SQL_NTS 或 SQL_NULL_DATA。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
 如果在 SQL 语句中发送参数的数据时调用 **SQLPutData** ，则它可以返回可由调用的函数返回的任何 SQLSTATE，以 (**SQLExecute** 或 **SQLExecDirect**) 执行该语句。 如果在发送使用 **SQLBulkOperations** 更新或添加的列的数据时调用此方法，或者使用 **SQLSetPos** 进行更新，则它可以返回 **SQLBulkOperations** 或 **SQLSetPos** 返回的任何 SQLSTATE。  
  
## <a name="comments"></a>注释  
 可以调用 **SQLPutData** 为两个用途提供数据执行数据：要在对 **SQLExecute** 或 **SQLExecDirect** 的调用中使用的参数数据，或通过调用 **SQLBulkOperations** 或通过调用 **SQLSetPos** 进行更新时要使用的列数据。  
  
 当应用程序调用 **SQLParamData** 来确定应发送的数据时，驱动程序将返回一个指示器，应用程序可以使用该指示器来确定要发送的参数数据或查找列数据的位置。 它还返回 SQL_NEED_DATA，它是应用程序的指示符，它应调用 **SQLPutData** 来发送数据。 在 **SQLPutData** 的 *DataPtr* 参数中，应用程序传递指向包含参数或列的实际数据的缓冲区的指针。  
  
 当驱动程序为 **SQLPutData** 返回 SQL_SUCCESS 时，应用程序将再次调用 **SQLParamData** 。 如果需要发送更多数据， **SQLParamData** 返回 SQL_NEED_DATA，在这种情况下，应用程序将再次调用 **SQLPutData** 。 如果已发送所有执行时数据数据，则会返回 SQL_SUCCESS。 然后，应用程序再次调用 **SQLParamData** 。 如果驱动程序返回 SQL_NEED_DATA 和 *\* ValuePtrPtr* 中的另一个指示器，则它需要另一个参数或列的数据，并且 **SQLPutData** 将再次调用。 如果驱动程序返回 SQL_SUCCESS，则已发送所有执行中的数据，并且可以执行 SQL 语句，或者可以处理 **SQLBulkOperations** 或 **SQLSetPos** 调用。  
  
 若要详细了解如何在语句执行时传递执行时数据参数数据，请参阅 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) 中的 "传递参数值" 和 [发送长数据](../../../odbc/reference/develop-app/sending-long-data.md)。 有关如何更新或添加执行时数据列数据的详细信息，请参阅[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)中的 "使用书签执行批量更新" 一[节中的](../../../odbc/reference/syntax/sqlsetpos-function.md)"使用 SQLSetPos"，以及[长数据、SQLSetPos 和 SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md)。  
  
> [!NOTE]  
>  仅当使用字符、二进制或数据源特定数据类型将字符 C 数据发送到具有字符、二进制或数据源特定数据类型的列时，应用程序才可以使用 **SQLPutData** 在部分中发送数据。 如果在任何其他条件下多次调用 **SQLPutData** ，则它将返回 SQL_ERROR 和 SQLSTATE HY019 () 中发送的非字符和非二进制数据。  
  
## <a name="example"></a>示例  
 下面的示例假定一个名为 Test 的数据源名称。 关联的数据库应具有可创建的表，如下所示：  
  
```sql  
CREATE TABLE emp4 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
```  
  
```cpp  
// SQLPutData.cpp  
// compile with: odbc32.lib user32.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define TEXTSIZE  12000  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;       
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // SQLBindParameter variables.  
   SQLLEN cbTextSize, lbytes;  
  
   // SQLParamData variable.  
   PTR pParmID;  
  
   // SQLPutData variables.  
   UCHAR  Data[] =   
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyz";  
  
   SDWORD cbBatch = (SDWORD)sizeof(Data) - 1;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Set parameters based on total data to send.  
   lbytes = (SDWORD)TEXTSIZE;  
   cbTextSize = SQL_LEN_DATA_AT_EXEC(lbytes);  
  
   // Bind the parameter marker.  
   retcode = SQLBindParameter (hstmt1,           // hstmt  
                               1,                // ipar  
                               SQL_PARAM_INPUT,  // fParamType  
                               SQL_C_CHAR,       // fCType  
                               SQL_LONGVARCHAR,  // FSqlType  
                               lbytes,           // cbColDef  
                               0,                // ibScale  
                               (VOID *)1,        // rgbValue  
                               0,                // cbValueMax  
                               &cbTextSize);     // pcbValue  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLBindParameter Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the command.  
   retcode =   
      SQLExecDirect(hstmt1, (UCHAR*)"INSERT INTO emp4 VALUES('Paul Borm', 46,'1950-11-12 00:00:00', ?)", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_NEED_DATA) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Check to see if NEED_DATA; if yes, use SQLPutData.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if (retcode == SQL_NEED_DATA) {  
      while (lbytes > cbBatch) {  
         SQLPutData(hstmt1, Data, cbBatch);  
         lbytes -= cbBatch;  
      }  
      // Put final batch.  
      retcode = SQLPutData(hstmt1, Data, lbytes);   
   }  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Make final SQLParamData call.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("Final SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Clean up.  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|返回要为其发送数据的下一个参数|[SQLParamData 函数](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
