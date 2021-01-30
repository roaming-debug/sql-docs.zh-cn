---
title: SQLDriverConnect 函数 |Microsoft Docs
description: SQLDriverConnect 函数是 ODBC API 标准的一部分，此参考文档提供了有关其语法的信息。
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverConnect
helpviewer_keywords:
- SQLDriverConnect function [ODBC]
ms.assetid: e299be1d-5c74-4ede-b6a3-430eb189134f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58c9f2071825c6c7d8f873f300e8e5c3f46af9b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195641"
---
# <a name="sqldriverconnect-function"></a>SQLDriverConnect 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLDriverConnect** 是 **SQLConnect** 的一种替代方法。 它支持的数据源所需的连接信息超过 **SQLConnect**、对话框中的三个参数，用于提示用户输入所有连接信息，以及在系统信息中未定义的数据源。 有关详细信息，请参阅 [与 SQLDriverConnect 连接](../develop-app/connecting-with-sqldriverconnect.md)。  

## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLDriverConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLHWND         WindowHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr,  
     SQLUSMALLINT    DriverCompletion);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *WindowHandle*  
 送窗口句柄。 应用程序可以传递父窗口的句柄（如果适用）; 如果窗口句柄不适用，则为 null 指针; 否则， **SQLDriverConnect** 不会显示任何对话框。  
  
 *InConnectionString*  
 送完整的连接字符串 (参见 "注释" ) 中的语法、部分连接字符串或空字符串。  
  
 *StringLength1*  
 送**InConnectionString* 的长度（如果字符串为 Unicode，则为个字符）; 如果 STRING 为 ANSI 或 DBCS，则为字节。  
  
 *OutConnectionString*  
 输出指向已完成连接字符串的缓冲区的指针。 成功连接到目标数据源后，此缓冲区包含完整的连接字符串。 应用程序应为此缓冲区分配至少1024个字符。  
  
 如果 *OutConnectionString* 为 NULL，则 *StringLength2Ptr* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *OutConnectionString* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送**OutConnectionString* 缓冲区的长度（以字符为限）。  
  
 *StringLength2Ptr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (不包括 null 终止字符) 可在 OutConnectionString 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength*，则 OutConnectionString 中已完成的连接字符串将 \* 截断为 *BufferLength* 减去 null 终止字符的长度。  
  
 *DriverCompletion*  
 送指示驱动程序管理器或驱动程序是否必须提示获取更多连接信息的标志：  
  
 SQL_DRIVER_PROMPT、SQL_DRIVER_COMPLETE、SQL_DRIVER_COMPLETE_REQUIRED 或 SQL_DRIVER_NOPROMPT。  
  
 有关其他信息的 (，请参阅 "注释"。)   
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLDriverConnect** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *fHandleType* 的 SQL_HANDLE_DBC 和 *hHandle 的 ConnectionHandle* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE *值。* 下表列出了通常由 **SQLDriverConnect** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *OutConnectionString* 不够大，无法返回整个连接字符串，因此连接字符串已被截断。 在 **StringLength2Ptr* 中返回未截断连接字符串的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S00|连接字符串属性无效| (*InConnectionString*) 的连接字符串中指定了无效的 attribute 关键字，但驱动程序仍然能够连接到数据源。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|驱动程序不支持 **SQLSetConnectAttr** 中的 *将 valueptr* 参数指向的指定值，并将替换为类似值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S08|保存文件 DSN 时出错|*\* InConnectionString* 中的字符串包含 **FILEDSN** 关键字，但未保存该 dsn 文件。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S09|关键字无效| (DM) *\* InConnectionString* 中的字符串包含 **SAVEFILE** 关键字，但不包含 **驱动程序** 或 **FILEDSN** 关键字。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08001|客户端无法建立连接|驱动程序无法与数据源建立连接。|  
|08002|连接名称正在使用中| (DM) 指定的 *ConnectionHandle* 已用于建立与数据源的连接，并且该连接仍处于打开状态。|  
|08004|服务器拒绝了连接|由于实现定义的原因，数据源已拒绝建立连接。|  
|08S01|通信链接失败|驱动程序与驱动程序尝试连接到的数据源之间的通信链接在 **SQLDriverConnect** 函数完成处理之前失败。|  
|28000|授权规范无效|无论是 (*InConnectionString*) 的连接字符串中指定的用户标识符还是授权字符串，都违反了数据源定义的限制。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* SzMessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY000|常规错误：无效的文件 dsn| (DM) **InConnectionString* 中的字符串包含 FILEDSN 关键字，但找不到该 dsn 文件的名称。|  
|HY000|常规错误：无法创建文件缓冲区| (DM) **InConnectionString* 中的字符串包含了 FILEDSN 关键字，但无法读取该 dsn 文件。|  
|HY001|内存分配错误|驱动程序管理器无法分配支持执行或完成 **SQLDriverConnect** 函数所需的内存。<br /><br /> 驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *ConnectionHandle* 启用异步处理。 函数被调用，在完成执行之前，对 *ConnectionHandle* 调用了 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)，然后在 *ConnectionHandle* 上再次调用了 **SQLDriverConnect** 函数。<br /><br /> 或者，调用 **SQLDriverConnect** 函数，并在其完成执行之前，从多线程应用程序中的另一个线程调用 *ConnectionHandle* 上的 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 另一个异步执行函数 (未 **SQLDriverConnect** 为 *ConnectionHandle* 调用) ，并且在调用 **SQLDriverConnect** 函数时仍在执行。|  
|HY013|内存管理错误|由于内存不足，因此无法处理 **SQLDriverConnect** 函数调用，原因可能是无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *StringLength1* 指定的值小于0且与 SQL_NTS 不相等。<br /><br />  (DM) 为参数 *BufferLength* 指定的值小于0。|  
|HY092|无效的属性/选项标识符| (DM) *DriverCompletion* 参数已 SQL_DRIVER_PROMPT，并且 *WindowHandle* 参数为 null 指针。|  
|HY110|驱动程序完成无效| (DM) 为参数 *DriverCompletion* 指定的值不等于 SQL_DRIVER_PROMPT、SQL_DRIVER_COMPLETE、SQL_DRIVER_COMPLETE_REQUIRED 或 SQL_DRIVER_NOPROMPT。<br /><br />  (DM) 连接池已启用，并且为参数 *DriverCompletion* 指定的值不等于 SQL_DRIVER_NOPROMPT。|  
|HYC00|未实现的可选功能|该驱动程序不支持应用程序请求的 ODBC 行为的版本。|  
|HYT00|超时时间已到|完成到数据源的连接之前，登录超时期限已过期。 超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_LOGIN_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 对应于指定数据源名称的驱动程序不支持该函数。|  
|IM002|找不到数据源，未指定默认驱动程序| (DM) 在系统信息中找不到在连接字符串中指定的数据源名称 (*InConnectionString*) ，并且没有默认的驱动程序规范。<br /><br /> 在系统信息中找不到 (DM) ODBC 数据源和默认驱动程序信息。|  
|IM003|无法加载指定的驱动程序| (DM) 系统信息中的数据源规范中列出的驱动程序，或者找不到或无法加载 **driver** 关键字指定的驱动程序。|  
|IM004|SQL_HANDLE_ENV 上驱动程序的 **SQLAllocHandle** 失败| (DM) 在 **SQLDriverConnect** 过程中，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数并 *fHandleType* 了 SQL_HANDLE_ENV，驱动程序返回了一个错误。|  
|IM005|SQL_HANDLE_DBC 上驱动程序的 **SQLAllocHandle** 失败。| (DM) 在 **SQLDriverConnect** 过程中，驱动程序管理器调用了驱动程序的 **SQLAllocHandle** 函数并 *fHandleType* 了 SQL_HANDLE_DBC，驱动程序返回了一个错误。|  
|IM006|驱动程序的 **SQLSetConnectAttr** 失败| (DM) 在 **SQLDriverConnect** 期间，驱动程序管理器调用了驱动程序的 **SQLSetConnectAttr** 函数，驱动程序返回了一个错误。|  
|IM007|未指定数据源或驱动程序;已禁止对话|未在连接字符串中指定数据源名称或驱动程序，并且 *DriverCompletion* 已 SQL_DRIVER_NOPROMPT。|  
|IM008|对话框失败|驱动程序尝试显示其登录对话框，但失败了。<br /><br /> *WindowHandle* 为空指针，且未 SQL_DRIVER_NO_PROMPT *DriverCompletion* 。|  
|IM009|无法加载转换 DLL|驱动程序无法加载为数据源或连接指定的转换 DLL。|  
|IM010|数据源名称太长| (DM) DSN 关键字的属性值的长度超过 SQL_MAX_DSN_LENGTH 个字符。|  
|IM011|驱动程序名称太长| (DM) **DRIVER** 关键字的属性值的长度超过255个字符。|  
|IM012|DRIVER 关键字语法错误| (DM) **DRIVER** 关键字的关键字值对包含语法错误。<br /><br />  (DM) *\* InConnectionString* 中的字符串包含了 **FILEDSN** 关键字，但 .dsn 文件未包含 **DRIVER** 关键字或 **dsn** 关键字。|  
|IM014|指定的 DSN 包含驱动程序和应用程序之间的体系结构不匹配| (DM) 32 位应用程序使用连接到64位驱动程序的 DSN;反之亦然。|  
|IM015|SQL_HANDLE_DBC_INFO_HANDLE 上驱动程序的 SQLDriverConnect 失败|如果驱动程序返回 SQL_ERROR，驱动程序管理器会将 SQL_ERROR 返回到应用程序，连接将失败。<br /><br /> 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
|S1118|驱动程序不支持异步通知|当驱动程序不支持异步通知时，不能设置 SQL_ATTR_ASYNC_DBC_EVENT 或 SQL_ATTR_ASYNC_DBC_RETCODE_PTR。|  
  
## <a name="comments"></a>注释  
 连接字符串具有以下语法：  
  
 *连接字符串* ：： = *空字符串*[;] &#124; *特性*[;] &#124; *特性*; *连接字符串*  
  
 *空字符串*：： =*attribute* ：： = *attribute-关键字* = *属性-值*&#124; DRIVER = [{]*attribute-value*[}]  
  
 *attribute 关键字* ：： = DSN &#124; UID &#124; PWD &#124; *驱动程序定义-attribute 关键字*  
  
 *特性-值* ：： = *字符串*  
  
 *驱动程序定义的-attribute 关键字* ：： = *标识符*  
  
 其中， *字符字符串* 具有零个或多个字符; *标识符* 包含一个或多个字符; *attribute-关键字* 不区分大小写; *特性-值* 可能区分大小写; **DSN** 关键字的值不只包含空白。  
  
 由于连接字符串和初始化文件语法，包含字符 [] 的关键字和属性值 **{} ( # A1，;？ \*=！ @** 不要用大括号括起来。 **DSN** 关键字的值不能只包含空格，并且不能包含前导空格。 由于系统信息的语法，关键字和数据源名称不能包含反斜杠 (\\) 字符。  
  
 应用程序不必在 **DRIVER** 关键字后面的属性值前后添加大括号，除非该属性包含分号 (; ) ，在这种情况下，需要使用大括号。 如果驱动程序接收的属性值包括大括号，驱动程序不应将其删除，而是返回的连接字符串的一部分。  
  
 括在大括号中的 DSN 或连接字符串值 ({}) 包含任何字符 **[] {} ( # A3，;？ \*=！ @** 将原封不动地传递给驱动程序。 但是，使用关键字时，驱动程序管理器将在使用文件 Dsn 时返回一个错误，但会将连接字符串传递到常规连接字符串的驱动程序。 避免在关键字值中使用嵌入的大括号。  
  
 连接字符串可能包含任意数量的驱动程序定义的关键字。 由于 **driver** 关键字不使用系统信息中的信息，因此驱动程序必须定义足够的关键字，以便驱动程序只能使用连接字符串中的信息连接到数据源。  (有关详细信息，请参阅本部分后面的 "驱动程序准则"。 ) 驱动程序定义连接数据源所需的关键字。  
  
 下表描述了 **DSN**、 **FILEDSN**、 **DRIVER**、 **UID**、 **PWD** 和 **SAVEFILE** 关键字的属性值。  
  
|关键字|属性值描述|  
|-------------|---------------------------------|  
|**DSN**|**SQLDataSources** 或 **SQLDriverConnect** 的 "数据源" 对话框返回的数据源的名称。|  
|**FILEDSN**|将为数据源生成连接字符串的 dsn 文件的名称。 这些数据源称为文件数据源。|  
|**驱动器**|**SQLDrivers** 函数返回的驱动程序的说明。 例如，Rdb 或 SQL Server。|  
|**UID**|用户 ID。|  
|**PWD**|对应于用户 ID 的密码; 如果没有用户 ID (PWD =; ) 的密码，则为空字符串。|  
|**SAVEFILE**|Dsn 文件的文件名，在此文件中，关键字的属性值用于生成时，应保存成功的连接。|  
  
 有关应用程序如何选择数据源或驱动程序的信息，请参阅 [选择数据源或驱动](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)程序。  
  
 如果在连接字符串中重复任何关键字，则驱动程序将使用与该关键字的第一个匹配项相关联的值。 如果 **DSN** 和 **驱动程序** 关键字包含在相同的连接字符串中，驱动程序管理器和驱动程序将使用首先出现的任何关键字。  
  
 **FILEDSN** 和 **DSN** 关键字是互斥的：使用第一个关键字，将忽略第二个关键字。 另一方面， **FILEDSN** 和 **DRIVER** 关键字并不相互排斥。 如果在连接字符串中出现包含 **FILEDSN** 的任何关键字，则使用连接字符串中的关键字的属性值，而不是在 .dsn 文件中使用相同关键字的属性值。  
  
 如果使用了 **FILEDSN** 关键字，则在 .dsn 文件中指定的关键字用于创建连接字符串。  (有关详细信息，请参阅本部分后面的 "文件数据源"。 ) **UID** 关键字是可选的;可以只使用 **DRIVER** 关键字创建一个 .dsn 文件。 **PWD** 关键字未存储在 .dsn 文件中。 用于保存和加载 .dsn 文件的默认目录将是 **CommonFileDir** 在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ Windows\CurrentVersion 和 "ODBC\DataSources" 中指定的路径的组合。  (如果 CommonFileDir 是 "C:\Program Files\Common Files"，则默认目录为 "C:\Program Files\Common Files\ODBC\Data 源"。 )   
  
> [!NOTE]  
>  可以通过在安装程序 DLL 中调用 [SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md) 和 [SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md) 函数来直接操作 .dsn 文件。  
  
 如果使用了 **SAVEFILE** 关键字，则在建立现有连接时使用的关键字的属性值将另存为 .dsn 文件，其名称为 **SAVEFILE** 关键字的属性值。 **SAVEFILE** 关键字必须与 **DRIVER** 关键字和/或 **FILEDSN** 关键字一起使用，否则函数将返回 SQLSTATE 01S09 (关键字) 无效的 SQL_SUCCESS_WITH_INFO。 **SAVEFILE** 关键字必须出现在连接字符串中的 **DRIVER** 关键字之前，否则结果将不确定。  
  
## <a name="driver-manager-guidelines"></a>驱动程序管理器指南  
 驱动程序管理器将构造要传递给驱动程序的 **SQLDriverConnect** 函数的 *InConnectionString* 参数中的驱动程序的连接字符串。 驱动程序管理器不会修改应用程序传递给它的 *InConnectionString* 参数。  
  
 驱动程序管理器的操作基于 *DriverCompletion* 参数的值：  
  
-   SQL_DRIVER_PROMPT：如果连接字符串不包含 **driver**、 **DSN** 或 **FILEDSN** 关键字，则驱动程序管理器将显示 "数据源" 对话框。 它从对话框返回的数据源名称和应用程序传递给它的任何其他关键字构造连接字符串。 如果对话框返回的数据源名称为空，驱动程序管理器将指定关键字-值对 DSN = 默认值。  (此对话框将不会显示名为 "Default" 的数据源。 )   
  
-   SQL_DRIVER_COMPLETE 或 SQL_DRIVER_COMPLETE_REQUIRED：如果应用程序指定的连接字符串包含 **DSN** 关键字，则驱动程序管理器将复制应用程序指定的连接字符串。 否则，它所执行的操作与 SQL_DRIVER_PROMPT *DriverCompletion* 时的操作相同。  
  
-   SQL_DRIVER_NOPROMPT：驱动程序管理器复制应用程序指定的连接字符串。  
  
 如果由应用程序指定的连接字符串包含 **DRIVER** 关键字，则驱动程序管理器将复制应用程序指定的连接字符串。  
  
 使用它构造的连接字符串，驱动程序管理器会确定要使用的驱动程序，连接到该驱动程序，并将其构造的连接字符串传递给驱动程序;有关驱动程序管理器和驱动程序的交互的详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)中的 "备注" 部分。 如果连接字符串不包含 **driver** 关键字，驱动程序管理器会确定要使用的驱动程序，如下所示：  
  
1.  如果连接字符串包含 **DSN** 关键字，驱动程序管理器将从系统信息中检索与数据源相关联的驱动程序。  
  
2.  如果连接字符串不包含 **DSN** 关键字或找不到数据源，驱动程序管理器将从系统信息中检索与默认数据源关联的驱动程序。  (有关详细信息，请参阅 [默认子项](../../../odbc/reference/install/default-subkey.md)。 ) 驱动程序管理器会将连接字符串中 **DSN** 关键字的值更改为 "default"。  
  
3.  如果连接字符串中的 **DSN** 关键字设置为 "DEFAULT"，则驱动程序管理器将从系统信息中检索与默认数据源关联的驱动程序。  
  
4.  如果找不到数据源，并且找不到默认数据源，则驱动程序管理器将返回 SQL_ERROR 与 SQLSTATE IM002 (找不到数据源，并且不) 指定默认驱动程序。  
  
## <a name="file-data-sources"></a>文件数据源  
 如果由应用程序在对 **SQLDriverConnect** 的调用中指定的连接字符串包含 **FILEDSN** 关键字，且此关键字未被 **DSN** 或 **DRIVER** 关键字取代，则驱动程序管理器使用 .dsn 文件中的信息和 *InConnectionString* 参数创建连接字符串。 驱动程序管理器按如下方式继续：  
  
1.  检查 .dsn 文件的文件名是否有效。 如果不是，则它将返回 SQL_ERROR，并返回 SQLSTATE IM014 (无效的文件 DSN) 名称。 如果文件名为空字符串 ( "" ) 并且未指定 SQL_DRIVER_NOPROMPT，则将显示 " **文件打开** " 对话框。 如果文件名包含有效路径，但没有文件名或文件名无效，并且未指定 SQL_DRIVER_NOPROMPT，则将显示 " **文件打开** " 对话框，并将当前目录设置为文件名中指定的目录。 如果文件名为空字符串 ( "" ) 或文件名包含有效路径，但未指定文件名或文件名无效，并且指定了 SQL_DRIVER_NOPROMPT，则 SQL_ERROR 将使用 SQLSTATE IM014 返回， (文件 DSN) 的名称无效。  
  
2.  读取 .dsn 文件的 [ODBC] 部分中的所有关键字。 如果 **driver** 关键字不存在，它将返回 SQL_ERROR， (driver 关键字语法为) ，但 dsn 文件为 unshareable，因此只包含 **dsn** 关键字。  
  
     如果文件数据源是 unshareable，则驱动程序管理器会读取 **DSN** 关键字的值，并根据需要连接到由 unshareable 文件数据源指向的用户或系统数据源。 不执行步骤3到步骤5。  
  
3.  构造驱动程序的连接字符串。 驱动程序连接字符串是在 .dsn 文件中指定的关键字与在原始应用程序连接字符串中指定的关键字的联合。 用于构造关键字重叠的驱动程序连接字符串的规则如下所示：  
  
    -   如果应用程序连接字符串中存在 **驱动** 程序关键字，并且在 .dsn 文件和应用程序连接字符串中指定 **的驱动程序关键字不** 相同，则会忽略 .dsn 文件中的驱动程序信息，并使用应用程序连接字符串中的驱动程序信息。 如果 **DRIVER** 关键字指定的驱动程序在 .dsn 文件和应用程序的连接字符串中是相同的，则在应用程序连接字符串中指定的所有关键字都将优先于在 .dsn 文件中指定的驱动程序。  
  
    -   在新的连接字符串中，将消除 **FILEDSN** 关键字。  
  
4.  通过在注册表项中查找 driver \\ 关键字指定的 HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI<Driver Name \Driver 来加载驱动程序 \> \<Driver Name> 。   
  
5.  将驱动程序传递到新的连接字符串。  
  
 有关 .dsn 文件的示例，请参阅 [使用文件数据源进行连接](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)。  
  
## <a name="savefile-keyword"></a>SAVEFILE 关键字  
 如果由应用程序指定的连接字符串包含 **SAVEFILE** 关键字，则驱动程序管理器会将连接字符串保存在 .dsn 文件中。 驱动程序管理器按如下方式继续：  
  
1.  检查作为 **SAVEFILE** 关键字的属性值包括的 .dsn 文件的文件名是否有效。 如果不是，则它将返回 SQL_ERROR，并返回 SQLSTATE IM014 (无效的文件 DSN) 名称。 文件名的有效性由标准系统命名规则确定。 如果文件名为空字符串 ( "" ) 并且不 SQL_DRIVER_NOPROMPT *DriverCompletion* 参数，则文件名有效。 如果该文件名已经存在，则当 SQL_DRIVER_NOPROMPT *DriverCompletion* 时，该文件将被覆盖。 如果 *DriverCompletion* 是 SQL_DRIVER_PROMPT、SQL_DRIVER_COMPLETE 或 SQL_DRIVER_COMPLETE_REQUIRED，则会出现一个对话框，提示用户指定是否应覆盖该文件。 如果未输入，则显示 " **文件保存** " 对话框。  
  
2.  如果驱动程序返回 SQL_SUCCESS 且文件名不是空字符串，则驱动程序管理器会将在 *OutConnectionString* 参数中返回的连接信息写入指定的文件，该文件的格式在此部分前面的 "连接字符串" 部分中指定。  
  
3.  如果驱动程序返回 SQL_SUCCESS 并且文件名为空字符串 ( "" ) ，则驱动程序管理器将调用具有指定的 *hwnd* 的 "**文件保存** 通用" 对话框，并将 *OutConnectionString* 中返回的连接信息写入到 "File-Save 通用" 对话框中指定的文件中的文件，该文件采用本部分前面的 "连接字符串" 部分中指定的格式。  
  
4.  如果驱动程序返回 SQL_SUCCESS，它将返回包含应用程序的连接字符串的 *OutConnectionString* 参数。  
  
5.  如果驱动程序返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR，则驱动程序管理器会将 SQLSTATE 返回到应用程序。  
  
## <a name="driver-guidelines"></a>驱动程序指南  
 驱动程序将检查驱动程序管理器传递给它的连接字符串是否包含 **DSN** 或 **driver** 关键字。 如果连接字符串包含 **DRIVER** 关键字，则驱动程序无法从系统信息中检索有关数据源的信息。 如果连接字符串包含 **dsn** 关键字或不包含 **dsn** 或 **DRIVER** 关键字，则驱动程序可以按如下所示从系统信息中检索有关数据源的信息：  
  
1.  如果连接字符串包含 **DSN** 关键字，则驱动程序将检索指定数据源的信息。  
  
2.  如果连接字符串不包含 **DSN** 关键字，则找不到指定的数据源，或将 **DSN** 关键字设置为 "默认值"，驱动程序将检索默认数据源的信息。  
  
 驱动程序使用它从系统信息中检索到的任何信息来增加在连接字符串中传递给它的信息。 如果系统信息中的信息与连接字符串中的信息重复，则驱动程序将使用连接字符串中的信息。  
  
 根据 *DriverCompletion* 的值，驱动程序会提示用户输入连接信息，例如用户 ID 和密码，并连接到数据源：  
  
-   SQL_DRIVER_PROMPT：驱动程序显示一个对话框，使用连接字符串中的值和系统信息 (如果任何) 作为初始值。 当用户退出对话框时，驱动程序将连接到数据源。 它还通过 InConnectionString 中 **DSN** 或 **DRIVER** 关键字的值 \* 以及从对话框返回的信息来构造一个连接字符串。 它将此连接字符串放在 **OutConnectionString* 缓冲区中。  
  
-   SQL_DRIVER_COMPLETE 或 SQL_DRIVER_COMPLETE_REQUIRED：如果连接字符串包含足够的信息，并且该信息正确，则驱动程序将连接到数据源并将 \* *InConnectionString* 复制到 \* *OutConnectionString*。 如果任何信息缺失或不正确，则驱动程序将执行与 SQL_DRIVER_PROMPT *DriverCompletion* 时相同的操作，不同之处在于，当 *DriverCompletion* 为 SQL_DRIVER_COMPLETE_REQUIRED 时，驱动程序将禁用控件以获取连接数据源时不需要的任何信息。  
  
-   SQL_DRIVER_NOPROMPT：如果连接字符串包含足够的信息，则驱动程序将连接到数据源并将 \* *InConnectionString* 复制到 \* *OutConnectionString*。 否则，驱动程序将为 **SQLDriverConnect** 返回 SQL_ERROR。  
  
 成功连接到数据源时，驱动程序还会将 \* *StringLength2Ptr* 设置为可在 **OutConnectionString* 中返回的输出连接字符串的长度。  
  
 如果用户取消了驱动程序管理器或驱动程序显示的对话框，则 **SQLDriverConnect** 将返回 SQL_NO_DATA。  
  
 有关驱动程序管理器和驱动程序如何在连接过程中交互的信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
 如果驱动程序支持 **SQLDriverConnect**，则驱动程序的系统信息的 driver 关键字部分必须包含 **ConnectFunctions** 关键字，并将第二个字符集设置为 "Y"。  
  
## <a name="connecting-when-connection-pooling-is-enabled"></a>启用连接池时连接  
 连接池允许应用程序重用已创建的连接。 调用 **SQLDriverConnect** 时，驱动程序管理器将尝试使用连接，该连接是已为连接池指定的环境中的连接池。 有关连接池的详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
 应用程序可以在对启用了池的连接调用 SQLDisconnect 之前设置 SQL_ATTR_RESET_CONNECTION。 有关详细信息，请参阅 [SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)。  
  
 当应用程序调用 **SQLDriverConnect** 连接到共用连接时，以下限制适用：  
  
-   如果在连接字符串中指定了 **SAVEFILE** 关键字，则不会执行任何连接池处理。  
  
-   如果启用了连接池，则只能使用 SQL_DRIVER_NOPROMPT 的 *DriverCompletion* 参数调用 **SQLDriverConnect** ;如果通过任何其他 *DriverCompletion* 调用 **SQLDriverConnect** ，则会返回 SQLSTATE HY110 (无效驱动程序完成) 。  
  
## <a name="connection-attributes"></a>连接属性  
 使用 **SQLSetConnectAttr** 设置的 SQL_ATTR_LOGIN_TIMEOUT 连接属性定义了驱动程序在返回到应用程序之前等待登录请求成功连接所用的秒数。 如果系统提示用户完成连接字符串，则在驱动程序启动连接过程时，将开始每个登录请求的等待时间。  
  
 默认情况下，驱动程序会在 SQL_MODE_READ_WRITE 访问模式下打开连接。 若要将访问模式设置为 SQL_MODE_READ_ONLY，应用程序必须在调用 **SQLDriverConnect** 之前使用 SQL_ATTR_ACCESS_MODE 属性调用 **SQLSetConnectAttr** 。  
  
 如果在数据源的系统信息中指定了默认的翻译库，则驱动程序会将其加载。 可以通过使用 SQL_ATTR_TRANSLATE_LIB 特性调用 **SQLSetConnectAttr** 来加载不同的转换库。 可以通过使用 SQL_ATTR_TRANSLATE_OPTION 选项调用 **SQLSetConnectAttr** 来指定转换选项。  
  
 有关详细信息，请参阅 [与 SQLDriverConnect 连接](../../../odbc/reference/develop-app/connecting-with-sqldriverconnect.md)。  
  
```cpp  
// SQLDriverConnect_ref.cpp  
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR OutConnStr[255];  
   SQLSMALLINT OutConnStrLen;  
  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            retcode = SQLDriverConnect( // SQL_NULL_HDBC  
               hdbc,   
               desktopHandle,   
               (SQLCHAR*)"driver=SQL Server",   
               _countof("driver=SQL Server"),  
               OutConnStr,  
               255,   
               &OutConnStrLen,  
               SQL_DRIVER_PROMPT );  
  
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
  
 另请参阅 [示例 ODBC 计划](../../../odbc/reference/sample-odbc-program.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|发现和枚举连接到数据源所需的值|[SQLBrowseConnect 函数](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|连接到数据源|[SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|断开与数据源的连接|[SQLDisconnect 函数](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|返回驱动程序说明和属性|[SQLDrivers 函数](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|释放句柄|[SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
