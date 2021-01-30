---
description: SQLSetEnvAttr 函数
title: SQLSetEnvAttr 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetEnvAttr
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC]
ms.assetid: 0343241c-4b15-4d4b-aa2b-2e8ab5215cd2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed611f343275c2d777d4f390154b160aa6346094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200953"
---
# <a name="sqlsetenvattr-function"></a>SQLSetEnvAttr 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLSetEnvAttr** 设置控制环境各个方面的特性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```  
  
## <a name="arguments"></a>参数  
 *EnvironmentHandle*  
 送环境句柄。  
  
 *Attribute*  
 送要设置的属性，在 "注释" 中列出。  
  
 *ValuePtr*  
 送指向要与 *属性* 关联的值的指针。 根据 *属性* 的值， *将 valueptr* 将为32位整数值，或指向以 null 值结束的字符串。  
  
 *StringLength*  
 送如果 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数应为 **将 valueptr* 的长度。 对于字符串数据，此参数应包含字符串中的字节数。  
  
 如果 *将 valueptr* 是一个整数，则将忽略 *StringLength* 。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetEnvAttr** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_ENV 和 *EnvironmentHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetEnvAttr** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。 如果驱动程序不支持环境属性，则只能在连接时返回此错误。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|该驱动程序不支持在 *将 valueptr* 中指定的值，并将其替换为类似值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY009|空值指针的使用无效|特性参数标识了需要字符串值的环境特性，并且 *将 valueptr* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 已在 *EnvironmentHandle* 上分配连接句柄。<br /><br />  (DM) **SQL_ATTR_ODBC_VERSION** 尚未设置 **SQLSetEnvAttr** ，并且 *属性* 与 **SQL_ATTR_ODBC_VERSION** 不相等。 如果使用的是 **SQLAllocHandleStd**，则无需显式设置 **SQL_ATTR_ODBC_VERSION** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY024|属性值无效|给定指定的 *属性* 值， *将 valueptr* 中指定的值无效。|  
|HY090|字符串或缓冲区长度无效|*StringLength* 参数小于0，但未 SQL_NTS。|  
|HY092|无效的属性/选项标识符| (DM) 为参数 *属性* 指定的值对于该驱动程序支持的 ODBC 版本无效。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|为 argument *特性* 指定的值为驱动程序所支持的 odbc 版本的有效 odbc 环境特性，但驱动程序不支持该特性。<br /><br />  (DM) *属性* 参数已 SQL_ATTR_OUTPUT_NTS，并且已 SQL_FALSE *将 valueptr* 。|  
  
## <a name="comments"></a>注释  
 仅当未在环境中分配连接句柄时，应用程序才可以调用 **SQLSetEnvAttr** 。 环境的应用程序成功设置的所有环境属性都将保留，直到在环境上调用 **SQLFreeHandle** 。 可以 *在 ODBC 2.x* 中同时分配多个环境句柄。  
  
 通过 *将 valueptr* 设置的信息的格式取决于指定的 *属性*。 **SQLSetEnvAttr** 将接受以下两种不同格式中的一种：以 null 结尾的字符串或32位整数值。 每个的格式记录在属性的说明中。  
  
 没有特定于驱动程序的环境属性。  
  
 无法通过调用 **SQLSetEnvAttr** 设置连接属性。 尝试执行此操作将返回 SQLSTATE HY092 (无效的属性/选项标识符) 。  
  
|*Attribute*|*将 valueptr* 内容|  
|-----------------|-------------------------|  
|SQL_ATTR_CONNECTION_POOLING (ODBC 3.8) |在环境级别启用或禁用连接池的32位 SQLUINTEGER 值。 使用以下值：<br /><br /> SQL_CP_OFF = 关闭连接池。 这是默认值。<br /><br /> SQL_CP_ONE_PER_DRIVER = 每个驱动程序支持单个连接池。 池中的每个连接都与一个驱动程序相关联。<br /><br /> SQL_CP_ONE_PER_HENV = 每个环境支持单个连接池。 池中的每个连接都与一个环境相关联。<br /><br /> SQL_CP_DRIVER_AWARE = 使用驱动程序的连接池感知功能（如果可用）。 如果该驱动程序不支持连接池感知，SQL_CP_DRIVER_AWARE 将被忽略，并使用 SQL_CP_ONE_PER_HENV。 有关详细信息，请参阅 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)。 在某些驱动程序支持的环境中，如果某些驱动程序不支持连接池感知，SQL_CP_DRIVER_AWARE 可以在这些支持驱动程序上启用连接池感知功能，但它等效于不支持连接池感知功能的那些驱动程序上的 SQL_CP_ONE_PER_HENV 设置。<br /><br /> 通过调用 **SQLSetEnvAttr** 以将 SQL_ATTR_CONNECTION_POOLING 特性设置为 SQL_CP_ONE_PER_DRIVER 或 SQL_CP_ONE_PER_HENV 来启用连接池。 在应用程序分配要为其启用连接池的共享环境之前，必须执行此调用。 对 **SQLSetEnvAttr** 的调用中的环境句柄设置为 null，这使得 SQL_ATTR_CONNECTION_POOLING 进程级特性。 启用连接池后，应用程序会通过调用 **SQLAllocHandle** 并将 *将 inputhandle* 参数设置为 SQL_HANDLE_ENV 来分配隐式共享环境。<br /><br /> 启用连接池并为应用程序选择共享环境后，将无法为该环境重置 SQL_ATTR_CONNECTION_POOLING，因为在设置此属性时，将使用 null 环境句柄调用 **SQLSetEnvAttr** 。 如果在共享环境中已启用连接池时设置此属性，则该属性仅影响随后分配的共享环境。<br /><br /> 还可以在环境中启用连接池。 请注意以下有关环境连接池的信息：<br /><br /> -在空句柄上启用连接池是一个进程级属性。 之后分配的环境将是共享环境，并将继承进程级连接池设置。<br />-在分配环境后，应用程序仍可以更改其连接池设置。<br />-如果启用了环境连接池，并且连接的驱动程序使用驱动程序池，则环境池将优先。<br /><br /> SQL_ATTR_CONNECTION_POOLING 是在驱动程序管理器内实现的。 驱动程序无需实现 SQL_ATTR_CONNECTION_POOLING。 ODBC 2.0 和3.0 应用程序可以设置此环境属性。<br /><br /> 有关详细信息，请参阅 [ODBC 连接池](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md)。|  
|SQL_ATTR_CP_MATCH (ODBC 3.0) |一个32位 SQLUINTEGER 值，用于确定如何从连接池中选择连接。 调用 **SQLConnect** 或 **SQLDriverConnect** 时，驱动程序管理器将确定从池中重复使用的连接。 驱动程序管理器尝试将调用中的连接选项与应用程序设置的连接属性与池中连接的关键字和连接属性进行匹配。 此属性的值确定匹配条件的精度级别。<br /><br /> 以下值用于设置此属性的值：<br /><br /> SQL_CP_STRICT_MATCH = 仅重新使用与调用中的连接选项和应用程序设置的连接属性完全匹配的连接。 这是默认值。<br /><br /> SQL_CP_RELAXED_MATCH = 可以使用匹配的连接字符串关键字的连接。 关键字必须匹配，但并不是所有的连接属性都必须匹配。<br /><br /> 有关驱动程序管理器如何在连接到共用连接时执行匹配的详细信息，请参阅 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)。 有关连接池的详细信息，请参阅 [ODBC 连接池](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md)。|  
|SQL_ATTR_ODBC_VERSION (ODBC 3.0) |一个32位整数，用于确定特定功能是 ODBC 2.x *行为还是 odbc* *1.x 行为。* 以下值用于设置此属性的值：<br /><br /> SQL_OV_ODBC3_80 = 驱动程序管理器和驱动程序展示以下 ODBC 3.8 行为：<br /><br /> -驱动程序返回并 *预计 ODBC 1.x* 代码的日期、时间和时间戳。<br />-调用 **SQLError**、 **SQLGetDiagField** 或 **SQLGetDiagRec** 时，驱动程序将 *返回 ODBC 1.x* SQLSTATE 代码。<br />-对 **SQLTables** 的调用中的 *CatalogName* 参数接受搜索模式。<br />-驱动程序管理器支持 C 数据类型扩展性。 有关 C 数据类型扩展性的详细信息，请参阅 [ODBC 中的 c 数据类型](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)。<br /><br /> 有关详细信息，请参阅 [ODBC 3.8 中的新增功能](../../../odbc/reference/what-s-new-in-odbc-3-8.md)。<br /><br /> SQL_OV_ODBC3 = 驱动程序管理器和驱动程序显示以下 *ODBC 3.x* 行为：<br /><br /> -驱动程序返回并 *预计 ODBC 1.x* 代码的日期、时间和时间戳。<br />-调用 **SQLError**、 **SQLGetDiagField** 或 **SQLGetDiagRec** 时，驱动程序将 *返回 ODBC 1.x* SQLSTATE 代码。<br />-对 **SQLTables** 的调用中的 *CatalogName* 参数接受搜索模式。<br />-驱动程序管理器不支持 C 数据类型扩展性。<br /><br /> SQL_OV_ODBC2 = 驱动程序管理器和驱动程序展示以下 *ODBC 2.x 行为。* 这对于 *使用 odbc 1.x* *驱动程序的 odbc 2.x 应用程序* 特别有用。<br /><br /> -驱动程序返回并 *预计 ODBC 2.x* 代码的日期、时间和时间戳。<br />-调用 **SQLError**、 **SQLGetDiagField** 或 **SQLGetDiagRec** 时，驱动程序将 *返回 ODBC 2.x SQLSTATE 代码。*<br />-对 **SQLTables** 的调用中的 *CatalogName* 参数不接受搜索模式。<br />-驱动程序管理器不支持 C 数据类型扩展性。<br /><br /> 应用程序必须在调用具有 SQLHENV 参数的任何函数之前设置此环境特性，否则，调用将返回 SQLSTATE HY010 (函数序列错误) 。 它是特定于驱动程序的，无论这些环境标志是否存在其他行为。<br /><br /> -有关详细信息，请参阅 [声明应用程序的 ODBC 版本](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md) 和 [行为更改](../../../odbc/reference/develop-app/behavioral-changes.md)。|  
|SQL_ATTR_OUTPUT_NTS (ODBC 3.0) |一个32位整数，用于确定驱动程序如何返回字符串数据。 如果 SQL_TRUE，则驱动程序将返回以 null 结尾的字符串数据。 如果 SQL_FALSE，则驱动程序将不返回以 null 结尾的字符串数据。<br /><br /> 此属性默认为 SQL_TRUE。 对 **SQLSetEnvAttr** 的调用，以便将其设置为 SQL_TRUE 返回 SQL_SUCCESS。 对 **SQLSetEnvAttr** 的调用，以将其设置为 SQL_FALSE 返回 (未) 实现的可选功能 SQL_ERROR 和 SQLSTATE HYC00。|  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|返回环境属性的设置|[SQLGetEnvAttr 函数](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [ODBC 3.8 中的新增功能](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
