---
description: SQLConnect 函数
title: SQLConnect 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConnect
helpviewer_keywords:
- SQLConnect function [ODBC]
ms.assetid: 59075e46-a0ca-47bf-972a-367b08bb518d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90d982db52e0656fba25eb54065ea84b751a7b18
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206135"
---
# <a name="sqlconnect-function"></a>SQLConnect 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLConnect** 建立与驱动程序和数据源的连接。 连接句柄引用有关与数据源的连接的所有信息的存储，包括状态、事务状态和错误信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLConnect(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      ServerName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      UserName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      Authentication,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *ServerName*  
 送数据源名称。 数据可以与程序位于同一台计算机上，也可以位于网络中某个位置的另一台计算机上。 有关应用程序如何选择数据源的信息，请参阅 [选择数据源或驱动程序](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)。  
  
 *NameLength1*  
 送**ServerName* 的长度（字符）。  
  
 *UserName*  
 送用户标识符。  
  
 *NameLength2*  
 送长度 **用户名* （字符）。  
  
 *身份验证*  
 送身份验证字符串通常 (密码) 。  
  
 *NameLength3*  
 送**身份验证* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLConnect** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLConnect** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|驱动程序不支持 **SQLSetConnectAttr** 中的 *将 valueptr* 参数的指定值，并将其替换为类似值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08001|客户端无法建立连接|驱动程序无法与数据源建立连接。|  
|08002|连接名称正在使用中| (DM) 指定的 *ConnectionHandle* 已用于建立与数据源的连接，并且该连接仍处于打开状态，或者用户正在浏览连接。|  
|08004|服务器拒绝了连接|由于实现定义的原因，数据源已拒绝建立连接。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与该驱动程序尝试连接到的数据源之间的通信链接失败。|  
|28000|授权规范无效|为参数 *UserName* 指定的值或为参数 *Authentication* 指定的值违反了数据源定义的限制。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误| (DM) 驱动程序管理器无法分配支持执行或完成该函数所需的内存。|  
|HY008|操作已取消|已为 *ConnectionHandle* 启用异步处理。 调用 **SQLConnect** 函数，并在其完成执行之前，在 *ConnectionHandle* 上调用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)，然后在 *ConnectionHandle* 上再次调用 **SQLConnect** 函数。<br /><br /> 或者，调用 **SQLConnect** 函数，并在其完成执行之前，从多线程应用程序中的另一个线程调用 *ConnectionHandle* 上的 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 异步执行的函数 (不是为 *ConnectionHandle* 调用了这一) ，并且在调用此函数时仍在执行。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为 "参数 *NameLength1*"、" *NameLength2*" 或 " *NameLength3* " 指定的值小于0但不等于 "SQL_NTS"。<br /><br />  (DM) 为参数 *NameLength1* 指定的值超出了数据源名称的最大长度。|  
|HYT00|超时时间已到|在与数据源建立连接之前，查询超时期限已过。 超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_LOGIN_TIMEOUT。|  
|HY114|驱动程序不支持连接级别的异步函数执行| (DM) 应用程序在建立连接之前已对连接句柄启用了异步操作。 但是，驱动程序不支持对连接句柄执行异步操作。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 数据源名称指定的驱动程序不支持该函数。|  
|IM002|找不到数据源，未指定默认驱动程序| (DM) 在系统信息中找不到参数 *ServerName* 中指定的数据源名称，也没有默认的驱动程序规范。|  
|IM003|指定的驱动程序无法连接到| (DM) 找不到系统信息的数据源规范中列出的驱动程序，或由于某些其他原因无法连接到。|  
|IM004|SQL_HANDLE_ENV 上驱动程序的 SQLAllocHandle 失败| (DM) 在 **SQLConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数， *HandleType* 为 SQL_HANDLE_ENV，驱动程序返回了一个错误。|  
|IM005|SQL_HANDLE_DBC 上驱动程序的 SQLAllocHandle 失败| (DM) 在 **SQLConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数， *HandleType* 为 SQL_HANDLE_DBC，驱动程序返回了一个错误。|  
|IM006|驱动程序的 SQLSetConnectAttr 失败|在 **SQLConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLSetConnectAttr** 函数，驱动程序返回了一个错误。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|IM009|无法连接到翻译 DLL|驱动程序无法连接到为数据源指定的转换 DLL。|  
|IM010|数据源名称太长| (DM) *\* ServerName* 超过 SQL_MAX_DSN_LENGTH 个字符。|  
|IM014|指定的 DSN 包含驱动程序和应用程序之间的体系结构不匹配| (DM) 32 位应用程序使用连接到64位驱动程序的 DSN;反之亦然。|  
|IM015|SQL_HANDLE_DBC_INFO_HANDLE 上驱动程序的 SQLConnect 失败|如果驱动程序返回 SQL_ERROR，驱动程序管理器会将 SQL_ERROR 返回到应用程序，连接将失败。<br /><br /> 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
|S1118|驱动程序不支持异步通知|当驱动程序不支持异步通知时，不能设置 SQL_ATTR_ASYNC_DBC_EVENT 或 SQL_ATTR_ASYNC_DBC_RETCODE_PTR。|  
  
## <a name="comments"></a>注释  
 有关应用程序为何要使用 **SQLConnect** 的信息，请参阅 [使用 SQLConnect 连接](../../../odbc/reference/develop-app/connecting-with-sqlconnect.md)。  
  
 在应用程序调用函数 (**SQLConnect**、 **SQLDriverConnect** 或 **SQLBrowseConnect**) 连接到驱动程序之前，驱动程序管理器不会连接到驱动程序。 在该点之前，驱动程序管理器会使用其自己的句柄并管理连接信息。 当应用程序调用连接函数时，驱动程序管理器会检查当前是否为指定的 *ConnectionHandle* 连接了驱动程序：  
  
-   如果驱动程序未连接到，驱动程序管理器将连接到该驱动程序，并使用 SQL_HANDLE_ENV 的 *HandleType* 、 **SQLAllocHandle** 和 *HandleType* 的 SQL_HANDLE_DBC， **SQLSetConnectAttr** (如果应用程序指定了任何连接属性) ，**并使用了** 驱动程序中的连接函数。 如果驱动程序为 **SQLSetConnectAttr** 返回了错误，驱动程序管理器会将 SQLSTATE IM006 (驱动程序的 **SQLSetConnectOption**) 失败，并将 SQL_SUCCESS_WITH_INFO 用于连接功能。 有关详细信息，请参阅 [连接到数据源或驱动程序](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)。  
  
-   如果指定的驱动程序已连接到 *ConnectionHandle* 上的，则驱动程序管理器仅调用驱动程序中的连接函数。 在这种情况下，驱动程序必须确保 *ConnectionHandle* 的所有连接属性都保持其当前设置。  
  
-   如果连接到其他驱动程序，则驱动程序管理器将使用 SQL_HANDLE_DBC 的 *HandleType* 调用 **SQLFreeHandle** ，然后，如果该环境中没有连接到的其他驱动程序，则它将在连接的驱动程序中调用 **SQLFreeHandle** 并 SQL_HANDLE_ENV *HandleType* ，然后断开该驱动程序的连接。 然后，它执行与驱动程序未连接时相同的操作。  
  
 然后，该驱动程序将分配句柄并初始化自身。  
  
 当应用程序调用 **SQLDisconnect** 时，驱动程序管理器将在驱动程序中调用 **SQLDisconnect** 。 但是，它不会断开驱动程序的连接。 这会为重复连接到数据源和从数据源断开连接的应用程序在内存中保留该驱动程序。 当应用程序使用 SQL_HANDLE_DBC 的 *HandleType* 调用 **SQLFreeHandle** 时，驱动程序管理器会调用 **SQLFreeHandle** ，其中 *HandleType* 的 SQL_HANDLE_DBC，然后使用驱动程序中的 SQL_HANDLE_ENV *SQLFreeHandle* HandleType  ，然后断开驱动程序的连接。  
  
 一个 ODBC 应用程序可以建立多个连接。  
  
## <a name="driver-manager-guidelines"></a>驱动程序管理器指南  
 **ServerName* 的内容会影响驱动程序管理器和驱动程序协同工作的方式，以建立与数据源的连接。  
  
-   如果 \* *ServerName* 包含有效的数据源名称，则驱动程序管理器将在系统信息中查找相应的数据源规范并连接到关联的驱动程序。 驱动程序管理器将每个 **SQLConnect** 参数传递给驱动程序。  
  
-   如果找不到数据源名称，或者 *ServerName* 为空指针，则驱动程序管理器将查找默认数据源规范并连接到关联的驱动程序。 驱动程序管理器会将 *用户名* 和 *身份验证* 参数（未修改）和 *ServerName* 参数的 "默认" 传递给驱动程序。  
  
-   如果 *ServerName* 参数为 "default"，则驱动程序管理器将查找默认数据源规范并连接到关联的驱动程序。 驱动程序管理器将每个 **SQLConnect** 参数传递给驱动程序。  
  
-   如果找不到数据源名称或者 *ServerName* 为空指针，并且不存在默认数据源规范，则驱动程序管理器将返回 SQL_ERROR， (找不到数据源名称，并且不) 指定默认驱动程序。  
  
 驱动程序管理员连接到之后，驱动程序可以在系统信息中查找其相应的数据源规范，并使用规范中特定于驱动程序的信息来完成其所需的连接信息集。  
  
 如果在数据源的系统信息中指定了默认的翻译库，则驱动程序将连接到它。 可以通过使用 SQL_ATTR_TRANSLATE_LIB 特性调用 **SQLSetConnectAttr** 来连接到不同的转换库。 可以通过使用 SQL_ATTR_TRANSLATE_OPTION 特性调用 **SQLSetConnectAttr** 来指定转换选项。  
  
 如果驱动程序支持 **SQLConnect**，则驱动程序的系统信息的驱动程序关键字部分必须包含 **ConnectFunctions** 关键字，其第一个字符设置为 "Y"。  
  
### <a name="connection-pooling"></a>连接池  
 连接池允许应用程序重用已创建的连接。 当启用连接池并调用 **SQLConnect** 时，驱动程序管理器将尝试使用连接，该连接是已为连接池指定的环境中的连接池。 此环境是一个共享环境，由使用池中的连接的所有应用程序使用。  
  
 在分配环境之前启用连接池，方法是调用 **SQLSetEnvAttr** ，将 SQL_ATTR_CONNECTION_POOLING 设置为 SQL_CP_ONE_PER_DRIVER (指定每个驱动程序的最多一个池) 或 SQL_CP_ONE_PER_HENV (每个环境最多指定一个池) 。 在这种情况下，将调用 **SQLSetEnvAttr** ，并将 *EnvironmentHandle* 设置为 null，这会使该属性成为进程级特性。 如果 SQL_ATTR_CONNECTION_POOLING 设置为 SQL_CP_OFF，将禁用连接池。  
  
 启用连接池后，将调用 **SQLAllocHandle** 的 SQL_HANDLE_ENV *HandleType* 来分配环境。 此调用分配的环境是共享环境，因为已启用连接池。 但是，在调用具有 SQL_HANDLE_DBC 的 *HandleType* 的 **SQLAllocHandle** 之前，将不会确定要使用的环境。  
  
 调用具有 SQL_HANDLE_DBC 的 *HandleType* 的 **SQLAllocHandle** ，以分配连接。 驱动程序管理器将尝试查找与应用程序设置的环境特性相匹配的现有共享环境。 如果不存在这样的环境，则会创建一个隐式 *共享环境*。 如果找到匹配的共享环境，则将环境句柄返回到应用程序，并递增其引用计数。  
  
 但是，在调用 **SQLConnect** 之前，将不会确定要使用的连接。 此时，驱动程序管理器将尝试查找连接池中与应用程序所请求的条件匹配的现有连接。 这些条件包括对 **SQLConnect** 的调用中请求的连接选项， (*服务器名称*、 *用户名* 和 *身份验证* 关键字的值) 和设置 SQL_HANDLE_DBC 的任何连接属性，因为 **SQLAllocHandle** 已调用 *HandleType 的* 。 驱动程序管理器对照池中的连接关键字和属性检查这些条件。 如果找到匹配项，则使用池中的连接。 如果未找到匹配项，则创建一个新连接。  
  
 如果 SQL_ATTR_CP_MATCH 环境特性设置为 SQL_CP_STRICT_MATCH，则匹配项对于池中要使用的连接必须是准确的。 如果 SQL_ATTR_CP_MATCH 环境特性设置为 SQL_CP_RELAXED_MATCH，则调用 **SQLConnect** 的连接选项必须匹配，但不是所有连接特性都必须匹配。  
  
 在调用 **SQLConnect** 之前由应用程序设置的连接属性与池中连接的连接属性不匹配时，将应用以下规则：  
  
-   如果在建立连接前必须设置连接属性：  
  
     如果 SQL_CP_STRICT_MATCH SQL_ATTR_CP_MATCH，则共用连接中的 SQL_ATTR_PACKET_SIZE 必须与应用程序设置的属性相同。 如果 SQL_CP_RELAXED_MATCH，则 SQL_ATTR_PACKET_SIZE 的值可能不同。  
  
     SQL_ATTR_LOGIN_VALUE 的值不会影响匹配项。  
  
-   如果在建立连接之前或之后可以设置连接属性：  
  
     如果应用程序尚未设置连接属性，但已在池中的连接上设置该属性，并且有一个默认值，则会将共用连接中的连接属性设置回默认值，并声明一个匹配项。 如果没有默认值，则不会将共用连接视为匹配项。  
  
     如果连接属性已由应用程序设置，但尚未对池中的连接进行设置，则池上的连接属性将更改为应用程序设置的，并声明匹配项。  
  
     如果连接属性已由应用程序设置，并且还在池中的连接上设置，但这些值不同，则使用应用程序的连接属性的值并声明匹配项。  
  
-   如果驱动程序特定的连接属性的值不相同并且 SQL_ATTR_CP_MATCH 设置为 SQL_CP_STRICT_MATCH，则不会使用池中的连接。  
  
 当应用程序调用 **SQLDisconnect** 断开连接时，连接将返回到连接池，并可用于重用。  
  
### <a name="optimizing-connection-pooling-performance"></a>优化连接池性能  
 如果涉及到分布式事务，则可以使用 SQLUINTEGER 位掩码 **SQL_DTC_TRANSITION_COST** 来优化连接池的性能。 引用的转换是连接属性的转换 SQL_ATTR_ENLIST_IN_DTC 从值0到非零，反之亦然。 这是指未登记到分布式事务中以登记到分布式事务中的连接，反之亦然。 根据驱动程序实现登记的方式 (设置连接属性 SQL_ATTR_ENLIST_IN_DTC) ，这些转换可能会消耗大量资源，因此应尽量避免这种转换。  
  
 驱动程序返回的值包含以下位的任意组合：  
  
-   如果设置了此项，则表示零到非零转换的开销要比从非零转换到另一个非零值大得多， (在其下一个事务) 登记以前登记的连接。 **SQL_DTC_ENLIST_EXPENSIVE**  
  
-   如果设置此属性，则表示非零到零转换的开销要比使用 SQL_ATTR_ENLIST_IN_DTC 属性已设置为零的连接大得多。 **SQL_DTC_UNENLIST_EXPENSIVE**  
  
 性能与连接使用情况之间的折衷。 如果驱动程序指示其中一个或多个转换成本高昂，驱动程序管理器的连接池程序将通过在池中保留更多连接来响应这一转换。 池中的某些连接是非事务性使用的首选，一些是事务使用的首选。 但是，如果驱动程序指示这些转换开销不高，则可以使用更少的连接，这可能会在非事务性和事务使用之间交替。  
  
 不支持 SQL_ATTR_ENLIST_IN_DTC 的驱动程序无需支持 SQL_DTC_TRANSITION_COST。 对于支持 SQL_ATTR_ENLIST_IN_DTC 但不能 SQL_DTC_TRANSITION_COST 的驱动程序，假设转换的开销并不高，就像驱动程序返回 0 (未为此值设置) 。  
  
 尽管 SQL_DTC_TRANSITION_COST 是在 ODBC 3.5 （ODBC 2）中引入的。*x* 驱动程序还可以支持它，因为无论驱动程序版本如何，驱动程序管理器都将查询此信息。  
  
### <a name="code-example"></a>代码示例  
 在下面的示例中，应用程序分配环境和连接句柄。 然后，它将连接到具有用户 ID 约翰和密码 Sesame 的 SalesOrders 数据源，并处理数据。 处理完数据后，它会断开与数据源的连接，并释放句柄。  
  
```cpp  
// SQLConnect_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR * OutConnStr = (SQLCHAR * )malloc(255);  
   SQLSMALLINT * OutConnStrLen = (SQLSMALLINT *)malloc(255);  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLCHAR*) "NorthWind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
### <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|发现和枚举连接到数据源所需的值|[SQLBrowseConnect 函数](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|断开与数据源的连接|[SQLDisconnect 函数](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|使用连接字符串或对话框连接到数据源|[SQLDriverConnect 函数](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|返回连接属性的设置|[SQLGetConnectAttr 函数](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
