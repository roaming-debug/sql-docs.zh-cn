---
description: 处理字符转换时 ODBC 驱动程序行为的变化
title: ODBC 更改处理 char 转换
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 682a232a-bf89-4849-88a1-95b2fbac1467
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d74887afafcc4ab0c00881081bf8abf941ac551
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97418611"
---
# <a name="odbc-driver-behavior-change-when-handling-character-conversions"></a>处理字符转换时 ODBC 驱动程序行为的变化
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ( # A0) 的 Native CLIENT ODBC 驱动程序更改了它的实现方式 SQL_WCHAR * (NCHAR/nvarchar/nvarchar (max) # A6 和 SQL_CHAR (\* CHAR/VARCHAR/NARCHAR (max) # A10 转换。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2012 Native Client ODBC 驱动程序时，ODBC 函数（如 SQLGetData、SQLBindCol、SQLBindParameter）返回 (-4) SQL_NO_TOTAL 作为长度/指示符参数。 以前版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序返回的长度值可能不正确。  
  
## <a name="sqlgetdata-behavior"></a>SQLGetData 行为  
 许多 Windows 函数允许指定缓冲区大小为 0，且返回的长度为返回的数据大小。 以下模式对于 Windows 程序员很常见：  
  
```  
int iSize = 0;  
BYTE * pBuffer = NULL;  
GetMyFavoriteAPI(pBuffer, &iSize);   // Returns needed size in iSize  
pBuffer = new BYTE[iSize];   // Allocate buffer   
GetMyFavoriteAPI(pBuffer, &iSize);   // Retrieve actual data  
```  
  
 但是，不应在此方案中使用 **SQLGetData** 。 不应使用以下模式：  
  
```  
// bad  
int iSize = 0;  
WCHAR * pBuffer = NULL;  
SQLGetData(hstmt, SQL_W_CHAR, ...., (SQLPOINTER*)0x1, 0, &iSize);   // Get storage size needed  
pBuffer = new WCHAR[(iSize/sizeof(WCHAR)) + 1];   // Allocate buffer  
SQLGetData(hstmt, SQL_W_CHAR, ...., (SQLPOINTER*)pBuffer, iSize, &iSize);   // Retrieve data  
```  
  
 只能调用 **SQLGetData** 来检索实际数据块。 不支持使用 **SQLGetData** 获取数据大小。  
  
 下面说明当您使用不正确的模式时驱动程序变化的影响。 此应用程序查询 **varchar** 列，并将其绑定为 Unicode (SQL_UNICODE/SQL_WCHAR) ：  
  
 Query  `select convert(varchar(36), '123')`  
  
```  
SQLGetData(hstmt, SQL_WCHAR, ....., (SQLPOINTER*) 0x1, 0 , &iSize);   // Attempting to determine storage size needed  
```  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序版本|长度或指示符的结果|说明|  
|-----------------------------------------------------------------|---------------------------------|-----------------|  
|[!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] Native Client 或更早版本|6|驱动程序错误地假定将 CHAR 转换为 WCHAR 可以通过长度 * 2 来实现。|  
|[!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] Native Client（版本 11.0.2100.60）或更高版本|-4 (SQL_NO_TOTAL)|该驱动程序不再假设从 CHAR 转换为 WCHAR 或 WCHAR 到 CHAR 是 (乘法) \* 2 或 (除以) /2 操作。<br /><br /> 调用 **SQLGetData** 将不再返回预期转换的长度。 驱动程序检测 CHAR 和 WCHAR 之间的转换并返回 (-4) SQL_NO_TOTAL 替代可能不正确的 *2 或 /2 行为。|  
  
 使用 **SQLGetData** 检索数据块。 （所示的伪代码：）  
  
```  
while( (SQL_SUCCESS or SQL_SUCCESS_WITH_INFO) == SQLFetch(...) ) {  
   SQLNumCols(...iTotalCols...)  
   for(int iCol = 1; iCol < iTotalCols; iCol++) {  
      WCHAR* pBufOrig, pBuffer = new WCHAR[100];  
      SQLGetData(.... iCol ... pBuffer, 100, &iSize);   // Get original chunk  
      while(NOT ALL DATA RETREIVED (SQL_NO_TOTAL, ...) ) {  
         pBuffer += 50;   // Advance buffer for data retrieved  
         // May need to realloc the buffer when you reach current size  
         SQLGetData(.... iCol ... pBuffer, 100, &iSize);   // Get next chunk  
      }  
   }  
}  
```  
  
## <a name="sqlbindcol-behavior"></a>SQLBindCol 行为  
 Query  `select convert(varchar(36), '1234567890')`  
  
```  
SQLBindCol(... SQL_W_CHAR, ...)   // Only bound a buffer of WCHAR[4] - Expecting String Data Right Truncation behavior  
```  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序版本|长度或指示符的结果|说明|  
|-----------------------------------------------------------------|---------------------------------|-----------------|  
|[!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] Native Client 或更早版本|20|**SQLFetch** 报告数据右侧存在截断情况。<br /><br /> 长度为返回的数据长度而非存储的数据长度（假定 *2 CHAR 到 WCHAR 的转换对于符号可能不正确）。<br /><br /> 缓冲区中存储的数据为 123 \ 0。 缓冲区被保证为以 NULL 结束。|  
|[!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] Native Client（版本 11.0.2100.60）或更高版本|-4 (SQL_NO_TOTAL)|**SQLFetch** 报告数据右侧存在截断情况。<br /><br /> 长度指示 -4 (SQL_NO_TOTAL)，因为数据的其余部分未转换。<br /><br /> 缓冲区中存储的数据为 123\0。 - 缓冲区被保证为以 NULL 结束。|  
  
## <a name="sqlbindparameter-output-parameter-behavior"></a>SQLBindParameter（OUTPUT 参数行为）  
 Query  `create procedure spTest @p1 varchar(max) OUTPUT`  
  
 `select @p1 = replicate('B', 1234)`  
  
```  
SQLBindParameter(... SQL_W_CHAR, ...)   // Only bind up to first 64 characters  
```  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序版本|长度或指示符的结果|说明|  
|-----------------------------------------------------------------|---------------------------------|-----------------|  
|[!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] Native Client 或更早版本|2468|**SQLFetch** 返回的数据不可用。<br /><br /> **SQLMoreResults** 返回的数据不可用。<br /><br /> 长度指示从服务器返回的数据大小而非缓冲区中存储的数据大小。<br /><br /> 原始缓冲区包含 63 个字节和 NULL 结束符。 缓冲区被保证为以 NULL 结束。|  
|[!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] Native Client（版本 11.0.2100.60）或更高版本|-4 (SQL_NO_TOTAL)|**SQLFetch** 返回的数据不可用。<br /><br /> **SQLMoreResults** 返回的数据不可用。<br /><br /> 长度指示 (-4) SQL_NO_TOTAL，因为数据的其余部分未转换。<br /><br /> 原始缓冲区包含 63 个字节和 NULL 结束符。 缓冲区被保证为以 NULL 结束。|  
  
## <a name="performing-char-and-wchar-conversions"></a>执行 CHAR 和 WCHAR 转换  
 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] Native Client ODBC 驱动程序提供几种执行 CHAR 和 WCHAR 转换的方式。 逻辑类似于操作 blob (varchar (max) 、nvarchar (max) ... ) ：  
  
-   绑定到 **SQLBindCol** 或 **SQLBindParameter** 时，数据会保存或截断到指定的缓冲区中。  
  
-   如果未绑定，则可以通过使用 **SQLGetData** 和 **SQLParamData** 在块区中检索数据。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
