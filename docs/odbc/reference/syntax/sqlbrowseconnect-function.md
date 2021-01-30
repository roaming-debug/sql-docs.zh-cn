---
description: SQLBrowseConnect 函数
title: SQLBrowseConnect 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBrowseConnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBrowseConnect
helpviewer_keywords:
- SQLBrowseConnect function [ODBC]
ms.assetid: b7f1be66-e6c7-4790-88ec-62b7662103c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33983c9fb41625fa6479c47045ca4a04738a1463
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181072"
---
# <a name="sqlbrowseconnect-function"></a>SQLBrowseConnect 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLBrowseConnect** 支持一种迭代方法，该方法发现和枚举连接到数据源所需的属性和属性值。 对 **SQLBrowseConnect** 的每次调用都将返回属性和属性值的连续级别。 枚举所有级别后，将完成到数据源的连接，并由 **SQLBrowseConnect** 返回完整的连接字符串。 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO 的返回代码指示已指定所有连接信息，并且应用程序现在已连接到数据源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLBrowseConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *InConnectionString*  
 送浏览请求连接字符串 (参阅 "注释" ) 中的 "*InConnectionString* 参数"。  
  
 *StringLength1*  
 送**InConnectionString* 的长度（字符）。  
  
 *OutConnectionString*  
 输出指向要在其中返回浏览结果连接字符串的字符缓冲区的指针 (参阅 "注释" ) 中的 "*OutConnectionString* 参数"。  
  
 如果 *OutConnectionString* 为 NULL，则 *StringLength2Ptr* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *OutConnectionString* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送**OutConnectionString* 缓冲区的长度（以字符为字符）。  
  
 *StringLength2Ptr*  
 输出 (不包括 null 终止) 可在 OutConnectionString 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength*，则 OutConnectionString 中的连接字符串将 \* 截断为 *BufferLength* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NEED_DATA、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLBrowseConnect** 返回 SQL_ERROR、SQL_SUCCESS_WITH_INFO 或 SQL_NEED_DATA 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *句柄 ConnectionHandle* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLBrowseConnect** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *OutConnectionString* 不够大，无法返回整个浏览结果连接字符串，因此字符串被截断。 Buffer **StringLength2Ptr* 包含未截断浏览结果连接字符串的长度。  (函数返回 SQL_NEED_DATA。 ) |  
|01S00|连接字符串属性无效|在浏览请求连接字符串 (*InConnectionString*) 中指定了无效的 attribute 关键字。  (函数返回 SQL_NEED_DATA。 ) <br /><br /> 在浏览请求连接字符串中指定了 attribute 关键字 (*InConnectionString*) ，不适用于当前连接级别。  (函数返回 SQL_NEED_DATA。 ) |  
|01S02|值已更改|驱动程序不支持 **SQLSetConnectAttr** 中的 *将 valueptr* 参数的指定值，并将其替换为类似值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08001|客户端无法建立连接|驱动程序无法与数据源建立连接。|  
|08002|连接名称正在使用中| (DM) 指定的连接已用于与数据源建立连接，并且该连接已打开。|  
|08004|服务器拒绝了连接|由于实现定义的原因，数据源已拒绝建立连接。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与该驱动程序尝试连接到的数据源之间的通信链接失败。|  
|28000|授权规范无效|无论是 (*InConnectionString*) 的浏览请求连接字符串中指定的用户标识符或授权字符串，都是由数据源定义的限制。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误| (DM) 驱动程序管理器无法分配支持执行或完成该函数所需的内存。<br /><br /> 驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|通过调用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)取消了异步操作。 然后，在 *ConnectionHandle* 上再次调用原始函数。<br /><br /> 已通过在多线程应用程序中的其他线程上调用 *ConnectionHandle* 上的 **SQLCancelHandle** 取消了操作。|  
|HY010|函数序列错误| (DM) 异步执行的函数 (不是为 *ConnectionHandle* 调用了这一) ，并且在调用此函数时仍在执行。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *StringLength1* 指定的值小于0且与 SQL_NTS 不相等。<br /><br />  (DM) 为参数 *BufferLength* 指定的值小于0。|  
|HY114|驱动程序不支持连接级别的异步函数执行| (DM) 应用程序在建立连接之前已对连接句柄启用了异步操作。 但是，驱动程序不支持对连接句柄进行异步操作。|  
|HYT00|超时时间已到|完成到数据源的连接之前，登录超时期限已过期。 超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_LOGIN_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 对应于指定数据源名称的驱动程序不支持该函数。|  
|IM002|找不到数据源，未指定默认驱动程序| (DM) 在系统信息中找不到在浏览请求连接字符串 (*InConnectionString*) 中指定的数据源名称，也没有默认的驱动程序规范。<br /><br /> 在系统信息中找不到 (DM) ODBC 数据源和默认驱动程序信息。|  
|IM003|无法加载指定的驱动程序| (DM) 系统信息中的数据源规范中列出的驱动程序，或者找不到或无法加载 **driver** 关键字指定的驱动程序。|  
|IM004|驱动程序在 SQL_HANDLE _ENV 上的 **SQLAllocHandle** 失败| (DM) 在 **SQLBrowseConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数， *HandleType* 为 SQL_HANDLE_ENV，驱动程序返回了一个错误。|  
|IM005|SQL_HANDLE_DBC 上驱动程序的 **SQLAllocHandle** 失败| (DM) 在 **SQLBrowseConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数， *HandleType* 为 SQL_HANDLE_DBC，驱动程序返回了一个错误。|  
|IM006|驱动程序的 **SQLSetConnectAttr** 失败| (DM) 在 **SQLBrowseConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLSetConnectAttr** 函数，驱动程序返回了一个错误。|  
|IM009|无法加载转换 DLL|驱动程序无法加载为数据源或连接指定的转换 DLL。|  
|IM010|数据源名称太长| (DM) DSN 关键字的属性值的长度超过 SQL_MAX_DSN_LENGTH 个字符。|  
|IM011|驱动程序名称太长| (DM) DRIVER 关键字的属性值的长度超过255个字符。|  
|IM012|DRIVER 关键字语法错误| (DM) DRIVER 关键字的关键字值对包含语法错误。|  
|IM014|指定的 DSN 包含驱动程序和应用程序之间的体系结构不匹配| (DM) 32 位应用程序使用连接到64位驱动程序的 DSN;反之亦然。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
|S1118|驱动程序不支持异步通知|当驱动程序不支持异步通知时，不能设置 SQL_ATTR_ASYNC_DBC_EVENT 或 SQL_ATTR_ASYNC_DBC_RETCODE_PTR。|  
  
## <a name="inconnectionstring-argument"></a>InConnectionString 参数  
 浏览请求连接字符串具有以下语法：  
  
 *connection-string* ：： = *attribute*[ `;` ] &#124; *特性* `;` *连接字符串*;<br>
 *attribute* ：： = *attribute-关键字* `=` *属性-值*&#124; `DRIVER=` [ `{` ]*属性-值*[ `}` ]<br>
 *attribute 关键字* ：： = `DSN` &#124; `UID` &#124; `PWD` &#124; *驱动程序定义-attribute 关键字*<br>
 *特性-值* ：： = *字符串*<br>
 *驱动程序定义的-attribute 关键字* ：： = *标识符*<br>
  
 其中， *字符字符串* 具有零个或多个字符; *标识符* 包含一个或多个字符; *attribute-关键字* 不区分大小写; *特性-值* 可能区分大小写; **DSN** 关键字的值不只包含空白。 由于连接字符串和初始化文件语法，包含字符 [] 的关键字和属性值 **{} ( # A1，;？ \*应避免出现 =！ @** 。 由于系统信息中的语法，关键字和数据源名称不能包含反斜杠 (\\) 字符。 对于 ODBC 2，则为。*x* 驱动程序，需要用大括号将 driver 关键字的属性值括起来。  
  
 如果在浏览请求连接字符串中重复任何关键字，则驱动程序将使用与第一次出现的关键字相关的值。 如果 **DSN** 和 **驱动程序** 关键字包含在同一浏览请求连接字符串中，则驱动程序管理器和驱动程序将使用首先出现的任何关键字。  
  
 有关应用程序如何选择数据源或驱动程序的信息，请参阅 [选择数据源或驱动](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)程序。  
  
## <a name="outconnectionstring-argument"></a>OutConnectionString 参数  
 浏览结果连接字符串是连接属性的列表。 连接属性由 attribute 关键字和相应的属性值组成。 浏览结果连接字符串具有以下语法：  
  
 *connection-string* ：： = *attribute*[ `;` ] &#124; *属性* `;` *连接-字符串*<br>
 *attribute* ：： = [ `*` ]*attribute-关键字* `=` *属性-值*<br>
 *attribute 关键字* ：： = *ODBC-attribute* &#124; *驱动程序定义的-attribute-关键字*<br>
 *ODBC-attribute* = { `UID` &#124; `PWD` } [ `:` *本地化标识符*]*驱动程序定义的属性-关键字*：： =*标识符*[ `:` *本地化标识符*]*属性-值*：： = `{` *属性-值列表* `}` &#124; `?` (大括号是文本; 它们由驱动程序返回。 ) <br>
 *特性-值列表*： *： = 字符串*[ `:` *本地化字符串*] &#124;*字符-字符串*[ `:` *本地化字符字符串*] `,` *属性-值列表*<br>
  
 其中， *字符字符串* 和 *本地化字符的字符串* 具有零个或多个字符; *标识符* 和 *本地化标识符* 包含一个或多个字符; *attribute-关键字* 不区分大小写;和 *属性值* 可能区分大小写。 由于连接字符串和初始化文件语法、关键字、本地化标识符和包含字符 [] 的属性值 **{} ( # A1，;？ \*应避免出现 =！ @** 。 由于系统信息中的语法，关键字和数据源名称不能包含反斜杠 (\\) 字符。  
  
 根据以下语义规则使用浏览结果连接字符串语法：  
  
-   如果星号 (\*) 在 *attribute 关键字* 之前，则该 *属性* 是可选的，并且在下一次调用 **SQLBrowseConnect** 时可省略。  
  
-   属性关键字 **UID** 和 **PWD** 与 **SQLDriverConnect** 中定义的含义相同。  
  
-   *驱动程序定义的-attribute 关键字* 命名可以为其提供属性值的属性的种类。 例如，它可能是 **服务器**、 **数据库**、 **主机** 或 **DBMS**。  
  
-   *ODBC-attribute* 关键字和 *驱动程序定义的属性关键字* 包括关键字的本地化版本或用户友好版本。 应用程序可将其用作对话框中的标签。 但是，在将浏览请求字符串传递到驱动程序时，必须使用 **UID**、 **PWD** 或 *标识符* 。  
  
-   {*属性-值列表*} 是对相应的 *attribute 关键字* 有效的实际值的枚举。 请注意，大括号 ({}) 不表示选择列表; 驱动程序返回这些选项。 例如，它可能是服务器名称或数据库名称列表的列表。  
  
-   如果 *属性值* 是单个问号 (？ ) ，则单个值对应于 *attribute 关键字*。 例如，UID = 约翰;PWD = Sesame。  
  
-   对 **SQLBrowseConnect** 的每次调用仅返回满足连接过程的下一个级别所需的信息。 驱动程序将状态信息与连接句柄相关联，以便始终可以在每次调用时确定上下文。  
  
## <a name="using-sqlbrowseconnect"></a>使用 SQLBrowseConnect  
 **SQLBrowseConnect** 需要分配的连接。 驱动程序管理器加载在或中指定的驱动程序，该驱动程序与初始浏览请求连接字符串中指定的数据源名称相对应;有关此情况的详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)中的 "备注" 部分。 在浏览过程中，驱动程序可能会与数据源建立连接。 如果 **SQLBrowseConnect** 返回 SQL_ERROR，则将终止未完成的连接，并将连接返回到未连接状态。  
  
> [!NOTE]  
>  **SQLBrowseConnect** 不支持连接池。 如果在启用了连接池的情况下调用 **SQLBrowseConnect** ，则会返回 SQLSTATE HY000 (常规) 错误。  
  
 首次在连接时调用 **SQLBrowseConnect** 时，浏览请求连接字符串必须包含 **DSN** 关键字或 **DRIVER** 关键字。 如果浏览请求连接字符串包含 **DSN** 关键字，驱动程序管理器将在系统信息中查找相应的数据源规范：  
  
-   如果驱动程序管理器找到相应的数据源规范，则会加载关联的驱动程序 DLL;驱动程序可以从系统信息中检索有关数据源的信息。  
  
-   如果驱动程序管理器找不到相应的数据源规范，它将查找默认数据源规范并加载关联的驱动程序 DLL;驱动程序可以从系统信息中检索有关默认数据源的信息。 "默认" 传递给 DSN 的驱动程序。  
  
-   如果驱动程序管理器找不到相应的数据源规范并且没有默认数据源规范，它将返回 SQL_ERROR，其中包含 SQLSTATE IM002 (找不到数据源，并且不) 指定默认驱动程序。  
  
 如果浏览请求连接字符串包含 **driver** 关键字，则驱动程序管理器加载指定的驱动程序;它不会尝试在系统信息中查找数据源。 由于 **driver** 关键字不使用系统信息中的信息，因此驱动程序必须定义足够的关键字，以便驱动程序只能使用浏览请求连接字符串中的信息连接到数据源。  
  
 每次调用 **SQLBrowseConnect** 时，应用程序都会在浏览请求连接字符串中指定连接属性值。 驱动程序返回浏览结果连接字符串中的属性和属性值的连续级别;只要存在尚未在浏览请求连接字符串中枚举的连接属性，它就会返回 SQL_NEED_DATA。 应用程序使用浏览结果连接字符串的内容来生成浏览请求连接字符串，以便下一次调用 **SQLBrowseConnect**。 所有必需的属性 (不在 *OutConnectionString* 参数) 前面的星号，必须包括在下一次调用 **SQLBrowseConnect** 中。 请注意，在生成当前的浏览请求连接字符串时，应用程序不能使用以前的浏览结果连接字符串的内容;也就是说，它不能为以前级别中设置的属性指定不同的值。  
  
 枚举了所有级别的连接及其关联属性后，驱动程序将返回 SQL_SUCCESS，与数据源的连接已完成，并将完整的连接字符串返回到应用程序。 连接字符串适用于与 **SQLDriverConnect** 结合使用，并使用 SQL_DRIVER_NOPROMPT 选项来建立其他连接。 但不能在对 **SQLBrowseConnect** 的另一次调用中使用完整的连接字符串。如果再次调用 **SQLBrowseConnect** ，则必须重复整个调用序列。  
  
 如果在浏览过程中存在可恢复的非严重错误， **SQLBrowseConnect** 还会返回 SQL_NEED_DATA;例如，由应用程序提供的无效密码或特性关键字。 当返回 SQL_NEED_DATA 并且浏览结果连接字符串不变时，将发生错误，应用程序可以调用 **SQLGetDiagRec** 来返回 SQLSTATE 以获取浏览时错误。 这允许应用程序更正属性并继续浏览。  
  
 应用程序可以通过调用 **SQLDisconnect** 随时终止浏览进程。 驱动程序将终止任何未完成的连接，并将连接返回到未连接状态。  
  
 如果在连接句柄上启用了异步操作， **SQLBrowseConnect** 可能还会返回 SQL_STILL_EXECUTING。 当它返回 SQL_NEED_DATA 时，应用程序必须使用 **SQLDisconnect** 来取消浏览进程。 如果 **SQLBrowseConnect** 返回 SQL_STILL_EXECUTING，应用程序应使用 **SQLCancelHandle** 来取消正在进行的操作。 在函数 SQL_NEED_DATA 返回后调用 **SQLCancelHandle** 不起作用。  
  
 有关详细信息，请参阅 [与 SQLBrowseConnect 连接](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md)。  
  
 如果驱动程序支持 **SQLBrowseConnect**，则驱动程序的系统信息中的 driver 关键字部分必须包含第三个字符设置为 "Y" 的 **ConnectFunctions** 关键字。  
  
## <a name="code-example"></a>代码示例  
  
> [!NOTE]  
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应 `Trusted_Connection=yes` 在连接字符串中指定而不是用户 ID 和密码信息。  
  
 在下面的示例中，应用程序重复调用 **SQLBrowseConnect** 。 每次 **SQLBrowseConnect** 返回 SQL_NEED_DATA 时，它会向其传递有关 OutConnectionString 中所需数据的信息 \* 。 应用程序将 *OutConnectionString* 传递到其例程 **GetUserInput** (不显示) 。 **GetUserInput** 分析信息，生成并显示一个对话框，并返回用户在 InConnectionString 中输入的信息 \* 。 应用程序在下一次调用 **SQLBrowseConnect** 时将用户的信息传递给驱动程序。 在应用程序提供了连接到数据源的驱动程序所需的所有信息后， **SQLBrowseConnect** 将返回 SQL_SUCCESS 并且应用程序将继续。  
  
 有关通过调用 **SQLBrowseConnect** 连接到 SQL Server 驱动程序的更详细示例，请参阅 [SQL Server 浏览示例](../../../odbc/reference/develop-app/sql-server-browsing-example.md)。  
  
 例如，若要连接到数据源销售，可能会发生以下操作。 首先，应用程序将以下字符串传递给 **SQLBrowseConnect**：  
  
```  
"DSN=Sales"  
```  
  
 驱动程序管理器加载与数据源销售相关联的驱动程序。 然后，它会调用该驱动程序的 **SQLBrowseConnect** 函数，并将其从应用程序接收到的参数相同。 驱动程序返回 **OutConnectionString* 中的以下字符串：  
  
```  
"HOST:Server={red,blue,green};UID:ID=?;PWD:Password=?"  
```  
  
 应用程序将此字符串传递到其 **GetUserInput** 例程，该例程会生成一个对话框，要求用户选择红色、蓝色或绿色服务器，并输入用户 ID 和密码。 例程将以下用户指定的信息传递回 \* *InConnectionString*，应用程序会将这些信息传递给 **SQLBrowseConnect**：  
  
```  
"HOST=red;UID=Smith;PWD=Sesame"  
```  
  
 **SQLBrowseConnect** 使用此信息以 Smith 的密码 Sesame 连接到红色服务器，然后在 **OutConnectionString* 中返回以下字符串：  
  
```  
"*DATABASE:Database={SalesEmployees,SalesGoals,SalesOrders}"  
```  
  
 应用程序将此字符串传递到其 **GetUserInput** 例程，该例程会生成一个对话框，要求用户选择数据库。 用户选择 empdata，应用程序使用此字符串调用 **SQLBrowseConnect** 最终时间：  
  
```  
"DATABASE=SalesOrders"  
```  
  
 这是驱动程序连接到数据源时所需的最后一条信息。 **SQLBrowseConnect** 返回 SQL_SUCCESS，而 **OutConnectionString* 包含完成的连接字符串：  
  
```cpp  
// SQLBrowseConnect_Function.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define BRWS_LEN 100  
SQLHENV henv;  
SQLHDBC hdbc;  
SQLHSTMT hstmt;  
SQLRETURN retcode;  
SQLCHAR szConnStrIn[BRWS_LEN], szConnStrOut[BRWS_LEN];  
SQLSMALLINT cbConnStrOut;  
  
void GetUserInput(SQLCHAR * szConnStrOut, SQLCHAR * szConnStrIn) {}  
  
int main() {  
   // Allocate the environment handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);        
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      // Set the version environment attribute.  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
         // Allocate the connection handle.  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // Call SQLBrowseConnect until it returns a value other than SQL_NEED_DATA   
            // (pass data source name the first time).  If SQL_NEED_DATA is returned, call GetUserInput   
            // (not shown) to build a dialog from the values in szConnStrOut.  The user-supplied values   
            // are returned in szConnStrIn, which is passed in the next call to SQLBrowseConnect.  
  
            strcpy_s((char*)szConnStrIn, _countof(szConnStrIn), "DSN=Sales");  
            do {  
               retcode = SQLBrowseConnect(hdbc, szConnStrIn, SQL_NTS,  
                  szConnStrOut, BRWS_LEN, &cbConnStrOut);  
               if (retcode == SQL_NEED_DATA)  
                  GetUserInput(szConnStrOut, szConnStrIn);  
            } while (retcode == SQL_NEED_DATA);  
  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
               // Allocate the statement handle.  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                  // Process data after successful connection  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               SQLDisconnect(hdbc);  
            }  
         }  
         SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      }  
   }  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配连接句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|连接到数据源|[SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|断开与数据源的连接|[SQLDisconnect 函数](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|使用连接字符串或对话框连接到数据源|[SQLDriverConnect 函数](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|返回驱动程序说明和属性|[SQLDrivers 函数](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|释放连接句柄|[SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
