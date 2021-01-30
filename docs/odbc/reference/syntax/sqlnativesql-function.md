---
description: SQLNativeSql 函数
title: SQLNativeSql 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLNativeSql
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLNativeSql
helpviewer_keywords:
- SQLNativeSql function [ODBC]
ms.assetid: b8efc247-27ab-4a00-92b6-1400785783fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7cc73e184b6b092e1de4eef40de8eaadd11d738b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160789"
---
# <a name="sqlnativesql-function"></a>SQLNativeSql 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLNativeSql** 返回驱动程序修改的 SQL 字符串。 **SQLNativeSql** 不执行 SQL 语句。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLNativeSql(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      InStatementText,  
     SQLINTEGER     TextLength1,  
     SQLCHAR *      OutStatementText,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   TextLength2Ptr);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *InStatementText*  
 送要转换的 SQL 文本字符串。  
  
 *TextLength1*  
 送**InStatementText* 文本字符串的长度（以字符为字符）。  
  
 *OutStatementText*  
 输出指向要在其中返回已转换的 SQL 字符串的缓冲区的指针。  
  
 如果 *OutStatementText* 为 NULL，则 *TextLength2Ptr* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *OutStatementText* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送OutStatementText 缓冲区中的字符数 \*  。 如果在调用 **SQLNativeSqlW**) 时， *\* InStatementText* 中返回的值为 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数。  
  
 *TextLength2Ptr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (不包括 null 终止) 可在 OutStatementText 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength*，则 OutStatementText 中已转换的 SQL 字符串将 \* 截断为 *BufferLength* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLNativeSql** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLNativeSql** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *OutStatementText* 不够大，无法返回整个 sql 字符串，因此 SQL 字符串已被截断。 未截断 SQL 字符串的长度在 **TextLength2Ptr* 中返回。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开|*ConnectionHandle* 未处于已连接状态。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22007|Datetime 格式无效|**InStatementText* 包含一个转义子句，该子句具有无效的日期、时间或时间戳值。|  
|24000|无效的游标状态|语句中引用的游标位于结果集的开头之前或结果集结束之后。 具有本机 DBMS 游标实现的驱动程序可能不返回此错误。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY009|空值指针的使用无效| (DM) **InStatementText* 为 null 指针。|  
|HY010|函数序列错误| (DM) 为 *ConnectionHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 参数 *TextLength1* 小于0，但不等于 SQL_NTS。|  
||| (DM) 参数 *BufferLength* 小于0，而参数 *OutStatementText* 不是 null 指针。|  
|HY109|游标位置无效|游标的当前行已被删除或尚未提取。 具有本机 DBMS 游标实现的驱动程序可能不返回此错误。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 下面的示例演示了对于包含标量函数 CONVERT 的以下输入 SQL 字符串， **SQLNativeSql** 可能返回的内容。 假设列 empid 为数据源中的整数类型：  
  
```sql  
SELECT { fn CONVERT (empid, SQL_SMALLINT) } FROM employee  
```  
  
 Microsoft SQL Server 的驱动程序可能返回以下已转换的 SQL 字符串：  
  
```sql  
SELECT convert (smallint, empid) FROM employee  
```  
  
 ORACLE 服务器的驱动程序可能返回以下已转换的 SQL 字符串：  
  
```sql  
SELECT to_number (empid) FROM employee  
```  
  
 Ingres 的驱动程序可能返回以下已转换的 SQL 字符串：  
  
```sql  
SELECT int2 (empid) FROM employee  
```  
  
 有关详细信息，请参阅 [直接执行](../../../odbc/reference/develop-app/direct-execution-odbc.md) 和 [准备好的执行](../../../odbc/reference/develop-app/prepared-execution-odbc.md)。  
  
## <a name="related-functions"></a>相关函数  
 无。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
